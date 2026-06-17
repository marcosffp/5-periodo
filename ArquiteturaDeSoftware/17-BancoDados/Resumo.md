# RESUMO — Escolhendo Banco de Dados

## 1. Contexto histórico: o modelo relacional

Em **1970–1972**, o Dr. **Edgar Frank Codd** (1923–2003), cientista da computação britânico trabalhando na **IBM**, criou o **modelo relacional** para gerenciamento de banco de dados. Durante décadas, esse modelo dominou quase que absolutamente a indústria — e a pergunta que a aula propõe é:

> **"Por que o mundo criou tantos bancos de dados diferentes se o relacional já existia?"**

A resposta está na evolução das arquiteturas de software: o relacional resolve muito bem os problemas de sistemas monolíticos centralizados, mas **entra em conflito com as necessidades de sistemas distribuídos de alta escala**.

---

## 2. Banco de Dados em Sistemas Monolíticos

### 2.1 Arquitetura básica

```
Usuário --> [ Aplicação ASP.NET MVC ] --> [ SQL Server ]
             (Máquina Virtual)             (Máquina Virtual)
```

Na forma mais simples de um sistema monolítico, há uma única aplicação comunicando-se com um único banco de dados relacional. Simples, direto, fácil de operar.

### 2.2 Escalando o monolito: o gargalo aparece

Quando o sistema cresce, a primeira estratégia é replicar a aplicação com um Load Balancer:

```
Usuário --> [Load Balancer] --> VM 1 (Aplicação) ---\
                            --> VM 2 (Aplicação) ------> [SQL Server] ← gargalo e ponto único de falha
                            --> VM N (Aplicação) ---/
                                     |
                                  [Redis]
                                 (cache)
```

O **Redis** ou **Memcached** entra como camada de cache para aliviar leituras no banco, mas o problema fundamental permanece: **o banco de dados relacional é o gargalo e o ponto único de falha de todo o sistema**.

### 2.3 Solução para o monolito: separação de leitura e escrita

A evolução natural dentro do mundo monolítico é separar as operações de escrita das de leitura:

```
                           [Redis Cache]
                                |
Usuário --> [Load Balancer] --> [Pool de VMs] --ESCRITA--> SQL Server (PRIMÁRIO)
                                                                   |
                                                           REPLICAÇÃO (Always On / Log Shipping)
                                                                   |
                                               --LEITURA--> SQL Server (RÉPLICA - somente leitura)
```

- **Escrita**: todas as operações de gravação vão para o banco **primário**.
- **Leitura**: consultas são direcionadas para a **réplica de leitura**, reduzindo a carga no primário.
- Isso aumenta a capacidade de leitura, mas **o primário ainda é o gargalo para escritas**.

### 2.4 A regra do monolito

> **Em arquiteturas monolíticas, a simplicidade operacional dita a regra:**
> - **Relacional (RDBMS)**: fonte única da verdade com transações ACID.
> - **Redis/Memcached**: camada de cache para aliviar o disco.
> - **Consistência Forte**: o estado é sempre garantido.

---

## 3. Banco de Dados em Sistemas Distribuídos

### 3.1 A transição para o mundo distribuído

Quando a **escala horizontal** e a **disponibilidade global** tornam o monolito inviável, a arquitetura precisa ser repensada. E nesse novo mundo, **a regra do jogo é outra**: as garantias absolutas do banco relacional passam a ser um custo que o sistema distribuído não pode pagar sem abrir mão de disponibilidade ou desempenho.

### 3.2 O Teorema CAP

Para entender a escolha de banco de dados em sistemas distribuídos, é essencial dominar o **Teorema CAP**, que afirma que um sistema distribuído só pode garantir **dois** dos três atributos simultaneamente:

| Propriedade | Sigla | O que significa |
|---|---|---|
| **Consistency** (Consistência) | C | Todos os nós veem os mesmos dados ao mesmo tempo |
| **Availability** (Disponibilidade) | A | O sistema responde mesmo diante de falhas |
| **Partition Tolerance** (Tolerância a Partições) | P | O sistema continua funcionando mesmo com falhas de rede entre nós |

> **Nota crítica**: em redes reais, **P é obrigatório** — falhas de rede são inevitáveis em sistemas distribuídos. Portanto, **a escolha real é entre Consistência (C) ou Disponibilidade (A)**.

### 3.3 O Teorema PACELC

O CAP descreve o comportamento sob falha de partição, mas **não diz nada sobre o comportamento em operação normal**. O **Teorema PACELC** estende o CAP para cobrir os dois cenários:

```
                    Partição (P)?
                   /             \
                SIM              NÃO
                 |                |
              Escolha           Escolha (ELSE)
             /       \          /       \
    Disponibilidade  Consistência   Latência  Consistência
         (A)              (C)          (L)        (C)
          └────── CAP ──────┘
```

- **Se há partição (P)**: o sistema escolhe entre **Disponibilidade (A)** ou **Consistência (C)** → lado CAP.
- **Senão (E — Else, operação normal)**: o sistema escolhe entre **Latência baixa (L)** ou **Consistência (C)** → lado LC.

Os dois perfis resultantes são:
- **PA/EL**: prioriza disponibilidade sob partição e baixa latência em operação normal → maioria dos bancos NoSQL.
- **PC/EC**: prioriza consistência sempre, mesmo com maior latência → bancos relacionais e sistemas financeiros.

---

## 4. Perfil de carga: escrita ou leitura?

Uma das perguntas mais importantes ao escolher um banco de dados em um sistema distribuído é: **o sistema é predominantemente de leitura ou de escrita?**

> **"A resposta muda completamente a arquitetura do banco de dados."**

| Aplicação | Perfil predominante | Proporção aproximada |
|---|---|---|
| **Instagram** | Altíssima leitura de feed, stories, perfis e mídia | **95% leitura / 5% escrita** |
| **WhatsApp** | Escrita intensa de mensagens + leitura quase imediata | **60% leitura / 40% escrita** |
| **Netflix** | Streaming é praticamente leitura contínua | **99% leitura / 1% escrita** |
| **Mercado Livre** | Muitas consultas de catálogo e busca | **90% leitura / 10% escrita** |
| **iFood** | Muito catálogo, rastreamento e leitura de status | **85% leitura / 15% escrita** |
| **OLX** | Navegação e pesquisa dominam | **95% leitura / 5% escrita** |
| **Nubank** | Transações críticas e consultas constantes | **70% leitura / 30% escrita** |

Observação: a **esmagadora maioria** das aplicações reais é **predominantemente de leitura**, o que favorece bancos com perfil **PA/EL** (disponibilidade e baixa latência).

---

## 5. Escolhas reais: como as grandes empresas usam bancos de dados

```
Instagram  ---> Cassandra (PA/EL) + RocksDB
WhatsApp   ---> Cassandra (PA/EL)
Netflix    ---> Cassandra (PA/EL) + PostgreSQL
Mercado Livre -> Cassandra (PA/EL) + MySQL
iFood      ---> MySQL (PA/EL)
OLX        ---> MySQL (PA/EL)
Nubank     ---> PostgreSQL (PC/EC) ← operações financeiras críticas exigem consistência
```

O **Cassandra** domina os casos de altíssima leitura e escala global porque é **PA/EL** por natureza: prioriza disponibilidade e baixa latência, aceitando consistência eventual.

O **Nubank** é o caso contrário: transações financeiras **não podem aceitar inconsistência**, então o perfil **PC/EC** do PostgreSQL é mandatório — consistência mesmo que isso custe latência.

---

## 6. Taxonomia dos bancos de dados

### 6.1 Categorias e ferramentas

| Categoria | Ferramentas | PACELC | Quando usar |
|---|---|---|---|
| **Relacionais (SQL)** | SQL Server, PostgreSQL, MySQL, Oracle | **PC/EC** | Consistência forte, transações ACID, dados estruturados |
| **NoSQL — Documentos** | MongoDB, CouchDB | **PA/EL** | Dados semi-estruturados, esquema flexível, alta leitura |
| **Chave-Valor / Em Memória** | Redis, Memcached, Amazon DynamoDB | **PA/EL** | Cache, sessões, filas, acesso ultrarrápido por chave |
| **Wide Column / Colunar** | Cassandra, ArangoDB | **PA/EL** | Alto volume de escrita distribuída, séries de dados por linha |
| **Grafos** | TigerGraph, Amazon Neptune | **PA/EL** | Relacionamentos complexos (redes sociais, recomendações, fraudes) |
| **Séries Temporais** | TimescaleDB, Prometheus | **PA/EL** | Métricas, monitoramento, IoT, dados com dimensão temporal |
| **Busca e Análise** | Elasticsearch | **PA/EL** | Busca full-text, análise de logs, pesquisa facetada |
| **Multimodelo** | Azure Cosmos DB | **PA/EL** | Múltiplos modelos de acesso (doc, grafo, chave-valor) em um só serviço |
| **Nuvem / Gerenciado** | DynamoDB, Neptune, Cosmos DB, Bigtable | **PA/EL** | Escalabilidade gerenciada, sem operação de infraestrutura |
| **Extensões** | PostGIS (geoespacial), Graphite (métricas) | PC/EC / PA/EL | Complementos especializados sobre bancos existentes |
| **Armazenamento de Objetos** | Amazon S3, Azure Blob Storage | **PA/EL** | Arquivos, imagens, vídeos, backups — não é banco de dados transacional |

### 6.2 O padrão PACELC por categoria

A regra geral é direta:
- **Bancos Relacionais** → **PC/EC**: consistência mesmo com maior latência.
- **Todos os outros** (NoSQL, colunar, grafos, séries temporais, busca, cloud) → **PA/EL**: disponibilidade e baixa latência, consistência eventual.

A única exceção prática: **PostGIS** (extensão geoespacial sobre PostgreSQL) herda o perfil **PC/EC** do PostgreSQL.

---

## 7. Banco de Dados em Sistemas Monolíticos vs. Distribuídos

| Dimensão | Monolítico | Distribuído |
|---|---|---|
| **Banco preferido** | Relacional (SQL) | Varia por domínio (NoSQL, colunar, grafo, etc.) |
| **Consistência** | Forte (ACID) | Eventual (BASE) na maioria dos casos |
| **Escalabilidade** | Vertical (mais hardware) / réplicas de leitura | Horizontal (mais nós) |
| **Gargalo** | Banco primário (ponto único de falha) | Distribuído entre nós — sem ponto único |
| **Cache** | Redis/Memcached como camada auxiliar | Redis/DynamoDB como componente de design |
| **Regra de ouro** | Simplicidade operacional | Escolha adequada por domínio de negócio |
| **PACELC típico** | PC/EC | PA/EL (com exceções financeiras em PC/EC) |

---

## 8. Conclusão

> **"Não existe o melhor banco de dados. Existe o banco mais adequado para os problemas que sua arquitetura precisa resolver."**

A evolução histórica explica tudo:
1. **O modelo relacional** (Codd, 1970–72) nasceu para resolver problemas de sistemas centralizados — e faz isso muito bem, com consistência forte e transações ACID.
2. **A escala horizontal e a disponibilidade global** quebraram a premissa de centralização: sistemas distribuídos precisam tolerar falhas de rede (P obrigatório), e isso exige ceder consistência ou disponibilidade.
3. **O surgimento dos bancos NoSQL** não foi capricho — foi resposta a limitações concretas do relacional em cenários de alta escala distribuída.
4. **O Teorema CAP** e sua extensão **PACELC** são as ferramentas teóricas para justificar a escolha: em redes reais, a decisão é entre consistência (C) e disponibilidade (A); em operação normal, entre consistência (C) e latência (L).
5. **O perfil de carga** (leitura vs. escrita) é o dado mais prático: a maioria das aplicações reais é **leitura-heavy**, o que favorece bancos PA/EL; operações financeiras críticas exigem PC/EC.

---

## 9. O que mais cai em prova

- **Por que surgiram tantos bancos de dados se o relacional já existia**: escalabilidade horizontal e disponibilidade global de sistemas distribuídos impõem restrições que o relacional, projetado para ambientes centralizados, não consegue atender sem sacrificar desempenho ou disponibilidade.
- **Teorema CAP**: Consistência (C), Disponibilidade (A), Tolerância a Partições (P) — em sistemas distribuídos, P é obrigatório; **a escolha real é entre C e A**.
- **Teorema PACELC** — extensão do CAP para operação normal: **PA/EL** (disponibilidade e baixa latência) vs. **PC/EC** (consistência mesmo com latência). Bancos relacionais → PC/EC; maioria dos NoSQL → PA/EL.
- **Perfil de carga (leitura vs. escrita)**: define a arquitetura do banco. Netflix = 99% leitura; Nubank = 30% escrita com transações críticas. A resposta muda completamente a escolha tecnológica.
- **Separação de leitura e escrita no monolito**: banco **primário** para escritas + **réplica** para leituras (Always On / Log Shipping no SQL Server). Melhora a capacidade de leitura, mas o primário ainda é gargalo para escritas.
- **Redis/Memcached**: camada de cache em memória — alivia o banco de dados em ambos os contextos (monolito e distribuído), mas não substitui o banco transacional.
- **Cassandra**: banco colunar, PA/EL por design — altíssima disponibilidade e escrita distribuída. Usado por Instagram, WhatsApp, Netflix. Aceita consistência eventual.
- **PostgreSQL/MySQL**: relacionais, PC/EC — consistência forte. PostgreSQL é escolha do Nubank para operações financeiras justamente por isso.
- **Categorias de banco NoSQL**: documentos (MongoDB, CouchDB), chave-valor/memória (Redis, DynamoDB), colunar/wide column (Cassandra), grafos (TigerGraph, Neptune), séries temporais (TimescaleDB, Prometheus), busca (Elasticsearch), multimodelo (Cosmos DB).
- **"Não existe o melhor banco de dados — existe o mais adequado"**: a conclusão-chave da aula. A escolha é sempre contextual: domínio de negócio, perfil de carga, necessidade de consistência, escala e tolerância a falhas.
