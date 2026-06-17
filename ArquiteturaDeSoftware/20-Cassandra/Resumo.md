# RESUMO — Apache Cassandra

## 1. Contexto histórico: por que o Cassandra nasceu

Em **2007**, engenheiros do **Facebook** buscavam um sistema capaz de armazenar dados para a crescente plataforma de mensagens da empresa. Nenhum banco existente atendia aos requisitos de escala. A solução foi combinar o melhor de dois papers acadêmicos publicados por grandes empresas:

```
                      Cassandra
                     /         \
          Google Bigtable    Amazon Dynamo
          ─────────────────  ─────────────────
          Column Families    Consistent Hashing
          Memtables          Partitioning
          SSTables           Replication
```

O resultado: um sistema com **estruturas de dados eficientes** e **consistência eventual**, em que as atualizações se propagam até que todas as réplicas correspondam com o passar do tempo.

> **Avinash Lakshman** — inventor do Cassandra e co-inventor do Amazon Dynamo — foi o principal arquiteto do projeto no Facebook.

Em 2008 o Cassandra foi liberado como open source e, em 2010, passou para a **Apache Software Foundation** — eliminando o lock-in com fornecedor e permitindo personalização pela comunidade.

---

## 2. Características principais

### 2.1 Software livre
Código aberto sob a Apache Software Foundation. Pode-se usar suporte da comunidade ou serviços comerciais gerenciados (DataStax Astra, Amazon Keyspaces, etc.).

### 2.2 Alto desempenho
O mecanismo de armazenamento usa um **caminho de gravação (write path)** com três componentes:
- **Commit Log** (disco): log de confirmação para durabilidade e recuperação.
- **Memtable** (RAM): tabela na memória para escritas ultrarrápidas.
- **SSTable** (disco): Sorted String Table — arquivo imutável gerado quando a Memtable é descarregada.

Dados acessados com frequência ficam em cache. A **compactação** é uma função de manutenção automática que reorganiza SSTables para eficiência a longo prazo.

### 2.3 Disponibilidade ajustada
De acordo com o Teorema CAP, sistemas distribuídos só podem garantir duas das três propriedades (C, A, P). O Cassandra oferece **níveis de consistência ajustáveis por operação** — permitindo priorizar disponibilidade ou consistência conforme o caso de uso.

### 2.4 Escalabilidade linear
O Cassandra **aumenta capacidade adicionando novos nós sem interrupção do serviço**. Ao adicionar nós, redistribui automaticamente dados e tráfego pelo cluster, aumentando o throughput proporcionalmente. Não exige atualizações verticais caras.

### 2.5 Replicação sem dificuldades
Replica dados em múltiplos nós e datacenters. Usuários locais têm baixa latência. Não há ponto único de falha. Implementado em **Java** e executado na **JVM**. Integra-se com Kubernetes e AWS.

### 2.6 Interface familiar — CQL
As equipes usam o **Cassandra Query Language (CQL)**, que espelha a sintaxe do SQL para definir keyspaces, tabelas e chaves primárias. Ferramenta interativa: **cqlsh** (CQL shell).

#### CQL vs. SQL — diferenças fundamentais

| Dimensão | SQL | CQL (Cassandra) |
|---|---|---|
| **Estrutura de dados** | Tabelas normalizadas | Dados desnormalizados, alinhados com partition keys |
| **Consistência** | Integridade rigorosa (ACID) | Consistência eventual com níveis configuráveis (BASE) |
| **Escalabilidade** | Vertical (mais hardware) | Horizontal (mais nós no cluster) |
| **Operações** | Otimizado para transações e JOINs | Consultas em tempo real e escritas de alto volume — sem JOINs |

> Desenvolvedores que migram do SQL adaptam rapidamente a sintaxe do CQL, mas **devem repensar as estratégias de modelagem de dados** para aproveitar a abordagem distribuída do Cassandra.

---

## 3. Arquitetura do Cluster

### 3.1 O que é um Cluster Cassandra

Um **cluster** é um conjunto de servidores (nós) executando Cassandra e trabalhando juntos como um único sistema distribuído:

```
Cliente/Aplicação --> [ Node 1 ] <--> [ Node 2 ] <--> [ Node 3 ] <--> [ Node 4 ]
                        (P2P — todos os nós se comunicam entre si)
```

**Regras fundamentais:**
- Todos os nós são **iguais** — não existe master nem servidor central (**Peer-to-Peer**).
- Qualquer nó pode receber leitura ou escrita.
- Dados são distribuídos e replicados automaticamente.

### 3.2 Datacenters

Nós podem estar em locais físicos diferentes (datacenters/regiões) e ainda fazerem parte do mesmo cluster:

```
Datacenter BH                     Datacenter SP
[ Node 1 ] [ Node 2 ] [ Node 3 ]  [ Node 4 ] [ Node 5 ] [ Node 6 ]
```

Exemplo de keyspace com replicação por datacenter:
```cql
CREATE KEYSPACE saeweb
WITH replication = {
  'class' : 'NetworkTopologyStrategy',
  'BH'    : 3,
  'SP'    : 2
};
-- Resultado: 3 cópias em BH + 2 cópias em SP = 5 cópias no total
```

### 3.3 Keyspace e Replication Factor

A hierarquia de organização é:

```
Cluster (Infraestrutura) --> Keyspace (Política de replicação) --> Tabelas (Dados)
```

O **Keyspace** é o contêiner de nível superior — análogo a um banco de dados em RDBMS. Ele define:
- **Fator de replicação (RF)**: quantas cópias de cada dado existirão.
- **Estratégia de replicação**: como as cópias são distribuídas.
- **Datacenters** participantes.

**Regras importantes do Replication Factor:**
- RF **não cria nós** — apenas define quantas cópias serão armazenadas nos nós existentes.
- RF não pode ser maior que o número de réplicas disponíveis (ex.: cluster com 4 nós não aceita RF=10).
- Com `NetworkTopologyStrategy`, a soma dos RFs por datacenter não pode exceder o total de nós do datacenter.

### 3.4 Configurações comuns de cluster

| Quantidade de Nós | RF recomendado | Tolerância a falhas | Observação |
|---|---|---|---|
| 1 nó | 1 | 0 nós | Apenas desenvolvimento/testes |
| 3 nós | 3 | 1 nó | Mínimo recomendado para produção |
| 4 nós | 3 | 1 nó | Bom equilíbrio entre custo e HA |
| 6 nós | 3 | 2 nós | Ambiente mais robusto |
| 9+ nós | 3 | 2 nós | Aumenta capacidade e desempenho |
| N nós | N-1 | N-2 nós | Máxima redundância (custo alto) |

**Boas práticas:** usar `NetworkTopologyStrategy` em múltiplos datacenters; RF=3 na maioria dos casos; monitorar e adicionar nós conforme o crescimento.

---

## 4. Modelagem de Dados: Partition Key e Clustering Key

### 4.1 O conceito central

No Cassandra, a chave primária tem dois componentes com papéis distintos:

```cql
PRIMARY KEY (partition_key, clustering_key)
--           └─────────────┘  └────────────┘
--           Define em qual nó  Define ordenação
--           o dado vai ficar   dentro da partição
```

### 4.2 Partition Key

> **"A Partition Key é o endereço dos dados dentro do cluster."**

- Define **em qual nó** os dados serão armazenados.
- É usada para calcular um **hash** → gera um **token** → localiza o nó responsável no ring.
- Para consultas por Partition Key, o Cassandra lê **apenas** o nó onde os dados estão (diferentemente de bancos relacionais, que usam índices para varrer qualquer lugar).

**Exemplo prático:**
```cql
CREATE TABLE user_messages (
  user_id    UUID,
  message_id TIMEUUID,
  text       TEXT,
  PRIMARY KEY (user_id, message_id)
);
-- Partition Key = user_id  → qual nó?
-- Clustering Key = message_id → ordenação dentro da partição
```

Inserções com o **mesmo** `user_id` vão para o **mesmo nó**. Inserções com `user_id` diferente podem ir para **nós diferentes**.

### 4.3 Hot Partition — o erro clássico

Usar uma Partition Key com poucos valores únicos concentra **todos os dados no mesmo nó**, degradando o desempenho:

```cql
-- RUIM: se empresa_id = 1 tiver 500 milhões de logs,
-- tudo vai para o mesmo nó → Hot Partition
CREATE TABLE logs (
  empresa_id  INT,
  data        TIMESTAMP,
  descricao   TEXT,
  PRIMARY KEY (empresa_id)
);

-- BOM: (empresa_id, ano_mes) gera partições diferentes por mês
CREATE TABLE logs_por_mes (
  empresa_id  INT,
  ano_mes     TEXT,
  data        TIMESTAMP,
  descricao   TEXT,
  PRIMARY KEY ((empresa_id, ano_mes), data)
);
```

**Regra de ouro da Partition Key:** distribuição equilibrada entre nós + evitar Hot Partitions + ser utilizada nas consultas mais frequentes.

### 4.4 Clustering Key

- **Opcional** — define a **ordenação dos dados dentro da partição**.
- Permite ordenar registros por data, ID, etc. diretamente no armazenamento.

```cql
CREATE TABLE cities (
  country     TEXT,
  city        TEXT,
  population  INT,
  PRIMARY KEY (country, city)
) WITH CLUSTERING ORDER BY (data_venda DESC);
-- country = Partition Key (qual nó)
-- city    = Clustering Key (ordenação dentro do nó)
```

---

## 5. Consistent Hashing e Token Ring

### 5.1 O problema

Como o Cassandra decide em qual nó salvar um dado — e como redistribui os dados quando um novo nó é adicionado sem mover tudo?

### 5.2 A solução: Consistent Hashing

> **"O Hashing Consistente é a tecnologia que permite ao Cassandra distribuir dados entre os nós do cluster sem precisar mover tudo quando um novo servidor é adicionado."**

O Cassandra usa o algoritmo **Murmur3Partitioner** por padrão:
- Aplica a função hash na **Partition Key**.
- Produz um inteiro de **64 bits**: de **-2⁶³ até 2⁶³ − 1** (−9.223.372.036.854.775.808 até +9.223.372.036.854.775.807).
- O espaço numérico enorme garante **distribuição equilibrada e mínimas colisões**.

### 5.3 Token Ring

Os tokens formam um **anel circular (Token Ring)**. Cada nó é responsável por uma faixa de tokens:

```
hash(Partition Key) → token → posição no ring → nó responsável
```

Exemplo com RF=3:
```
Dado hasheado para o Node 2
→ cópia principal no Node 2
→ replica nos próximos 2 nós no sentido horário: Node 3 e Node 4
```

Quando um novo nó é adicionado ao cluster, ele assume uma faixa de tokens de nós vizinhos — apenas uma fração dos dados é movida, não todos.

---

## 6. Gossip Protocol

### 6.1 O que é

O **Protocolo Gossip** é o mecanismo de descoberta e comunicação entre nós do cluster. Periodicamente, **cada nó escolhe aleatoriamente outros nós** e envia suas informações. As informações se espalham por todo o cluster de forma rápida e eficiente, **sem necessidade de um servidor central**.

### 6.2 O que os nós trocam (Gossip Message)

| Informação | Detalhe |
|---|---|
| **Alive nodes** | Quais nós estão ativos |
| **Dead nodes** | Se um nó falhar, os outros são avisados rapidamente |
| **Partition ranges** | Quais faixas do token ring cada nó é responsável |
| **Schema versions** | Todos os nós compartilham as versões do schema |
| **Outros estados** | Configurações, load, capacidades do cluster |

**Resultado:** todos os nós têm uma visão consistente do cluster e detectam falhas rapidamente, **sem ponto único de falha**.

---

## 7. Consistência Ajustável (Tunable Consistency)

O Cassandra não é "sempre consistente" nem "sempre disponível" de forma absoluta — ele permite **configurar o nível de consistência por operação**, navegando no espectro do Teorema CAP:

| Nível | Comportamento | Trade-off |
|---|---|---|
| **ONE** | Aguarda confirmação de **1 réplica** | Mais rápido, consistência mais fraca |
| **QUORUM** | Aguarda confirmação da **maioria das réplicas** | Equilíbrio entre velocidade e consistência |
| **ALL** | Aguarda confirmação de **todas as réplicas** | Consistência mais forte, mais lento |

> O mesmo cluster Cassandra pode usar ONE para um serviço de feed de redes sociais (onde consistência eventual é aceitável) e QUORUM para operações de cobrança (onde mais garantias são necessárias).

**Trade-off central do Cassandra:**

```
[Disponibilidade + Escalabilidade + Tolerância a Falhas]  vs.  [Consistência forte]
```

---

## 8. Write Path — Como o Cassandra grava dados

```
Client → Write Request
            │
            ├──────────────────> Commit Log (Disco) ← durabilidade / recuperação
            │
            └──────────────────> Memtable (RAM) ← escrita ultrarrápida
                                      │
                                 [quando atinge o limite: 256MB / 512MB / 1GB]
                                      │
                                    Flush
                                      │
                                      ▼
                                 SSTable (Disco) ← Sorted String Table (imutável)
```

**Pontos-chave:**
- O Cassandra **não grava imediatamente na SSTable** — a gravação vai primeiro para o Commit Log e para a Memtable simultaneamente (em paralelo).
- O Commit Log garante que nenhum dado seja perdido se o nó cair antes do Flush.
- O **Flush** é a transferência da Memtable para a SSTable, disparado quando a Memtable atinge o tamanho configurado.
- SSTables são **imutáveis** — nunca são alteradas; novos dados criam novas SSTables, e a **compactação** as mescla periodicamente.

---

## 9. Read Path — Como o Cassandra lê dados

```
Client → Read Request
            │
            1. Verifica Memtable (RAM) ← dados mais recentes
            │
            2. Se não encontrar → verifica SSTables (Disco)
                   usando Bloom Filters e índices
                   para localizar registros sem varrer tudo
```

**Por que verificar a Memtable primeiro?**

> Porque os dados mais recentes ainda podem **não ter sido gravados na SSTable** — o Flush pode não ter acontecido. Se a leitura fosse direto para disco, dados recentes estariam invisíveis.

**Bloom Filter:** estrutura probabilística que responde "este dado existe nesta SSTable?" sem ler o arquivo inteiro — reduz I/O de disco drasticamente em leituras de dados ausentes.

Esta abordagem permite **combinar alta velocidade de escrita com leituras eficientes** em grandes volumes de dados.

---

## 10. Modelagem de Dados no Cassandra

### 10.1 A diferença fundamental

| | Banco Relacional | Cassandra |
|---|---|---|
| **Princípio** | Modelamos os dados | **Modelamos as consultas** |
| **Normalização** | Redundância mínima | **Redundância intencional** |
| **JOINs** | Necessários para reunir dados | **Não existem** |
| **Consistência** | ACID | BASE (Eventually Consistent) |
| **Leitura** | Mais custosa (JOINs, índices) | **Muito rápida** (tabela por consulta) |
| **Escrita** | Uma operação | **Mais custosa** (atualiza várias tabelas) |

### 10.2 Exemplo prático: e-commerce

**No banco relacional**, o mesmo e-commerce seria modelado com 4 tabelas normalizadas e JOINs na consulta:

```sql
SELECT c.nome, p.id, ip.quantidade, pr.nome
FROM clientes c
JOIN pedidos p       ON p.cliente_id = c.id
JOIN itens_pedido ip ON ip.pedido_id = p.id
JOIN produtos pr     ON pr.id = ip.produto_id
WHERE c.id = 1;
```

**No Cassandra**, cria-se **uma tabela por padrão de consulta** — os dados são repetidos intencionalmente:

```cql
-- Consulta 1: listar pedidos de um cliente
CREATE TABLE pedidos_por_cliente (
  cliente_id INT, pedido_id UUID, data_pedido TIMESTAMP,
  cliente_nome TEXT, produto TEXT, quantidade INT, preco_unitario DECIMAL,
  PRIMARY KEY (cliente_id, data_pedido)
) WITH CLUSTERING ORDER BY (data_pedido DESC);

-- Consulta 2: listar pedidos de uma data
CREATE TABLE pedidos_por_data (
  data_pedido DATE, pedido_id UUID, cliente_id INT, ...
  PRIMARY KEY (data_pedido, pedido_id)
);

-- Consulta 3: listar pedidos de um produto
CREATE TABLE pedidos_por_produto (
  produto TEXT, pedido_id UUID, ...
  PRIMARY KEY (produto, pedido_id)
);

-- Consulta no Cassandra (sem JOIN!):
SELECT * FROM pedidos_por_cliente
WHERE cliente_id = 1
ORDER BY data_pedido DESC;
```

> **"No Cassandra, não modelamos os dados. Modelamos as consultas."**

**Trade-off da modelagem:** Performance de Escrita/Leituras ↔ Redundância de Dados. Você ganha leituras ultrarrápidas e sem JOINs; em troca, uma atualização de dados pode exigir atualizar várias tabelas.

---

## 11. Cassandra como DBaaS na Nuvem

O Cassandra pode ser consumido como serviço gerenciado (sem operar infraestrutura):

| Provedor | Serviço | Destaque |
|---|---|---|
| **AWS** | Amazon Keyspaces | Gerenciado, compatível com CQL, Multi-AZ |
| **Google Cloud** | Cloud Firestore (Cassandra API) | API Cassandra sobre Firestore |
| **Microsoft Azure** | Azure Managed Instance for Apache Cassandra | Baseado em instâncias Cassandra |
| **DataStax** | DataStax Astra DB | 100% compatível, multi-cloud (AWS/GCP/Azure) |
| **IBM Cloud** | IBM Cloud Databases for Apache Cassandra | Open source, alta disponibilidade |

Todos oferecem **compatibilidade com CQL** — migração de aplicações existentes é direta.

---

## 12. Quando usar (e quando não usar) Cassandra

| Use Cassandra | NÃO use Cassandra |
|---|---|
| Alto throughput de escrita (100k+ rps) | Necessidade de consultas flexíveis (ad-hoc) |
| Escrita >> leitura | Necessidade de consistência forte (ACID) |
| Padrões de consulta previsíveis e limitados | Transações financeiras críticas |
| Alta disponibilidade global (múltiplos DCs) | Relacionamentos complexos com JOINs |
| Redes sociais, IoT, mensageria, monitoramento | — |

---

## 13. Conclusão

> **"O Apache Cassandra é um banco de dados NoSQL distribuído projetado para armazenar grandes volumes de dados com alta disponibilidade, escalabilidade horizontal e tolerância a falhas. Diferentemente dos bancos relacionais, ele modela os dados com base nas consultas esperadas, utilizando desnormalização e replicação para eliminar JOINs e maximizar o desempenho. Sua arquitetura sem servidor mestre, combinada com particionamento e replicação automática dos dados, permite que o sistema continue operando mesmo diante da falha de nós."**

Os quatro pilares que fazem o Cassandra funcionar:
1. **Consistent Hashing**: distribui dados entre nós sem mover tudo ao escalar.
2. **Replicação**: cópias em múltiplos nós e DCs garantem disponibilidade e tolerância a falhas.
3. **Gossip Protocol + arquitetura sem líder**: cluster descentralizado, resiliente e auto-recuperável.
4. **Consistência ajustável**: equilíbrio por operação entre consistência e disponibilidade.

---

## 14. O que mais cai em prova

- **Origem do Cassandra**: criado no Facebook em 2007 por Avinash Lakshman. Combinação de **Amazon Dynamo** (consistent hashing, partitioning, replication) + **Google Bigtable** (column families, memtables, SSTables).
- **Peer-to-Peer**: não existe master nem servidor central no Cassandra. Todos os nós são iguais e qualquer nó pode receber leitura ou escrita.
- **Partition Key**: define em qual nó do cluster os dados serão armazenados. Calculada via hash → token → ring. É o "endereço" do dado no cluster.
- **Clustering Key**: define a ordenação dos dados **dentro** de uma partição (opcional).
- **Hot Partition**: erro clássico de modelagem — Partition Key com poucos valores únicos concentra todos os dados em um nó, degradando o desempenho.
- **Consistent Hashing (Murmur3Partitioner)**: produz inteiro de 64 bits (-2⁶³ a 2⁶³-1). Distribui dados no Token Ring sem precisar mover tudo ao adicionar nós.
- **Gossip Protocol**: nós trocam informações (alive/dead nodes, partition ranges, schema versions) periodicamente, sem servidor central. Resultado: visão consistente do cluster e detecção rápida de falhas.
- **Tunable Consistency — ONE / QUORUM / ALL**: ONE = 1 réplica (mais rápido, menos consistente); QUORUM = maioria (equilíbrio); ALL = todas as réplicas (mais consistente, mais lento).
- **Write Path**: escrita simultânea em **Commit Log** (disco, durabilidade) + **Memtable** (RAM, performance). Quando Memtable atinge o limite → **Flush** → **SSTable** (disco, imutável).
- **Read Path**: busca primeiro na **Memtable** (dados recentes podem não ter sofrido Flush ainda), depois nas **SSTables** (usando **Bloom Filters** para evitar varredura completa).
- **"No Cassandra, não modelamos os dados. Modelamos as consultas."**: cada padrão de consulta gera uma tabela. Sem JOINs. Redundância intencional. Leituras muito rápidas; escritas mais custosas por atualizar várias tabelas.
- **Trade-off principal**: Disponibilidade + Escalabilidade + Tolerância a Falhas **vs.** Consistência forte. E na modelagem: Performance de Leitura/Escrita **vs.** Redundância de Dados.
- **Replication Factor (RF)**: não cria nós — define quantas cópias serão armazenadas. RF=3 é o recomendado para produção (1 cópia principal + 2 réplicas, tolera falha de 1 nó).
- **Keyspace**: contêiner de nível superior (análogo a "banco de dados" no RDBMS). Define fator de replicação, estratégia de replicação e datacenters.
