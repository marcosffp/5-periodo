# Introdução a Sistemas Distribuídos
**PUC Minas — ICEI — Aplicações Móveis e Distribuídas — Aula 03**
**Professor:** Cristiano de Macedo Neto

*Definições, Metas, Transparência e Arquiteturas*

*"A definição mais simples de sistema distribuído é aquela que você não consegue fazer funcionar porque uma máquina da qual você nunca ouviu falar está quebrada."* — Leslie Lamport

**🎯 Objetivos:**
- Compreender a definição formal de SD
- Identificar as metas de design fundamentais
- Dominar os tipos de transparência
- Conhecer modelos arquiteturais clássicos

---

## O que é um Sistema Distribuído?

**Segundo Tanenbaum & Van Steen:** "Um sistema distribuído é uma coleção de elementos computacionais autônomos que aparece aos seus usuários como um sistema único e coerente."

**Segundo Coulouris et al.:** "Um sistema no qual componentes de hardware ou software localizados em computadores em rede se comunicam e coordenam suas ações apenas através de troca de mensagens."

### Anatomia da Definição

**Elementos Autônomos:** cada nó opera independentemente, com seu próprio SO, memória local, CPU e relógio local (não sincronizado).

**Comunicação por Mensagens:** não existe memória compartilhada física entre nós. Toda coordenação acontece via rede (sockets, TCP/UDP).

**Ilusão de Sistema Único:** do ponto de vista do usuário, parece ser um único sistema. Ex: Google Search parece "um site", mas são milhões de servidores coordenados.

---

## Sistemas Distribuídos ao Nosso Redor

- **Web & Cloud:** Google, Netflix, AWS/Azure, WhatsApp
- **Sistemas Financeiros:** ATMs, bolsas de valores, Pix, Blockchain
- **Gaming & Entretenimento:** Fortnite/LoL, Spotify, Twitch
- **Infraestrutura Crítica:** DNS, Email (SMTP), 5G, Smart Cities

**Característica Comum:** todos escondem a complexidade do usuário. Você não sabe em qual servidor seu vídeo do Netflix está, ou em qual datacenter sua mensagem do WhatsApp foi processada.

---

## Por Que Construir Sistemas Distribuídos?

1. **Economia:** commodity hardware é mais barato que supercomputadores. 100 PCs comuns custam menos que 1 mainframe com mesma capacidade.
2. **Escalabilidade:** crescer adicionando mais máquinas. Supercomputador tem limite físico; SD pode crescer indefinidamente (em teoria).
3. **Distribuição Geográfica:** dados e processamento perto dos usuários. Reduz latência e atende regulamentações (LGPD/GDPR).
4. **Confiabilidade:** redundância — se um nó falha, outros continuam. Sem ponto único de falha.
5. **Compartilhamento de Recursos:** múltiplos usuários acessam recursos compartilhados (impressoras de rede, BDs corporativos, Dropbox).
6. **Performance:** paralelização de tarefas. MapReduce, Spark para big data. Ex: renderização de filmes (Pixar usa cluster).

---

## Metas de Design de SD

Quatro metas fundamentais, frequentemente em conflito:

- **Transparência:** ocultar complexidade do usuário
- **Abertura:** interfaces bem definidas e interoperabilidade
- **Escalabilidade:** crescer mantendo performance
- **Confiabilidade:** funcionar mesmo com falhas parciais

⚠️ **Trade-off importante:** maximizar transparência pode prejudicar performance. Escalabilidade extrema pode comprometer consistência. O design é sempre sobre balancear prioridades.

---

## Meta 1: Transparência

Fazer o sistema distribuído parecer um sistema único e coeso. O usuário não deveria saber onde está cada componente.

| Tipo | O Que Esconde | Exemplo |
|---|---|---|
| Acesso | Diferenças em representação e forma de acesso | Acessar arquivo local ou remoto com mesma API |
| Localização | Onde o recurso está fisicamente | URL `https://api.com/users` (não sabemos em qual datacenter) |
| Migração | Que um recurso foi movido para outro local | VM migrada entre hosts (live migration) |
| Relocação | Que um recurso está sendo movido enquanto em uso | Mobile IP: você se move entre antenas sem perder conexão |
| Replicação | Que existem múltiplas cópias do recurso | CDN: vídeo replicado em múltiplos servidores |
| Concorrência | Que múltiplos usuários compartilham o recurso | Google Docs: múltiplos editando simultaneamente |
| Falha | Que um componente falhou e está se recuperando | Netflix continua funcionando se 1 servidor cair (failover) |

**Caso de Estudo — Google Search:**
- Transparência de Localização: você não sabe em qual datacenter sua busca foi processada
- Transparência de Replicação: índice replicado em centenas de servidores, mas você vê um único resultado
- Transparência de Falha: se 10 servidores caem, sua busca ainda funciona

### Limites da Transparência

Transparência total é **impossível e às vezes indesejável**:
- **Latência de rede:** chamada remota sempre será mais lenta que local
- **Falhas parciais:** rede pode falhar, mas memória local não
- **Semântica diferente:** operações distribuídas têm comportamento diferente

**Princípio de Design:** exponha custos quando necessário. Desenvolvedores precisam saber quando estão fazendo operações caras (rede, I/O) para otimizar adequadamente. `async/await` deixa claro que pode demorar; nomear funções como `get_user_remote` sinaliza que é chamada de rede.

**Checkpoint 1:** Google Drive — salva um arquivo e acessa de outro dispositivo. A resposta correta é **B — Transparência de Replicação**: o arquivo foi replicado para múltiplos servidores sem o usuário saber.

---

## Meta 2: Escalabilidade

Sistema é escalável se mantém performance efetiva quando recursos e usuários aumentam significativamente.

### Três Dimensões de Escalabilidade

1. **Tamanho (Size Scalability):** adicionar mais usuários e recursos. Desafio: algoritmos centralizados não escalam (O(n²)). Solução: sharding, particionamento, load balancing.
2. **Geográfica (Geographic Scalability):** usuários e recursos espalhados geograficamente. Desafio: latência da velocidade da luz (Brasil-Japão ≈ 300ms). Solução: CDN, edge computing, replicação regional.
3. **Administrativa (Administrative Scalability):** gerenciar sistema que abrange múltiplas organizações. Desafio: políticas conflitantes, segurança, confiança. Solução: federação, padrões abertos, descentralização.

### Técnicas para Escalabilidade

| Técnica | Como Funciona | Exemplo |
|---|---|---|
| Particionamento (Sharding) | Dividir dados entre múltiplos servidores | MongoDB: usuários A-M no servidor1, N-Z no servidor2 |
| Replicação | Copiar dados/serviços para múltiplos nós | Banco de dados com 3 réplicas (master-slave) |
| Caching | Armazenar cópias de dados frequentes perto do usuário | CDN, Redis, browser cache |
| Assincronismo | Processar tarefas em background, não bloquear | Fila de mensagens (RabbitMQ, Kafka) |

### Gargalos de Escalabilidade

- **Serviço Centralizado:** único servidor processa tudo → solução: load balancing, múltiplas réplicas
- **Dados Centralizados:** todos os dados em um único banco → solução: sharding, NoSQL distribuído (Cassandra)
- **Algoritmos Centralizados:** precisam de estado global → solução: algoritmos descentralizados (gossip, DHT)

**Caso Real — Twitter Fail Whale (2008–2010):** arquitetura monolítica com MySQL centralizado não suportou o crescimento. Solução: migração para microserviços + Cassandra + Redis.

**Checkpoint 2:** API que vai de 50ms para 5s com 10.000 usuários simultâneos. Causa mais provável: **B — Gargalo de escalabilidade**, provavelmente servidor ou banco de dados único.

---

## Modelos Arquiteturais de SD

### 1. Cliente-Servidor
Mais comum. Servidor fornece serviço (sempre ativo, IP fixo); cliente solicita e inicia comunicação.
- ✅ Simples de implementar, centralização facilita gerenciamento e backup
- ❌ Ponto único de falha, gargalo de escalabilidade

**Variações:** N-Tier (Cliente → Web Server → App Server → DB), Microserviços.

### 2. Peer-to-Peer (P2P)
Descentralizado. Todos os nós têm capacidades equivalentes — cada nó é cliente E servidor, sem autoridade central.
- ✅ Alta escalabilidade, sem ponto único de falha, resistente a censura
- ❌ Complexo de implementar, difícil garantir segurança, peers entram/saem a qualquer momento

**Caso de Estudo — BitTorrent:** arquivo dividido em pedaços; cada peer baixa de múltiplos outros e serve pedaços que já tem. Quanto mais pessoas baixando, **mais rápido fica** — paradoxalmente.

### 3. Híbrido
Combinação: servidor para discovery/coordenação + P2P para transferência de dados. Exemplos: Napster (original), Spotify, Skype (atual).

**Checkpoint 3:** sistema de compartilhamento para universidade com 50.000 alunos que deve funcionar mesmo se servidor cair. Resposta: **B — P2P ou Híbrido** — tolera falha do servidor e escala com número de peers.

---

## Paradigmas de Comunicação

| Paradigma | Características | Exemplo/Tecnologia |
|---|---|---|
| RPC | Chamada de função remota parece local; síncrono, bloqueante | gRPC, Apache Thrift, Java RMI |
| REST (HTTP/JSON) | Baseado em recursos (URLs); stateless, request-response | APIs Web, microserviços |
| Message Queues | Assíncrono, desacoplado; producer-consumer | RabbitMQ, Kafka, AWS SQS |
| Publish-Subscribe | Broadcast para múltiplos interessados; event-driven | Redis Pub/Sub, MQTT (IoT) |
| Streaming | Fluxo contínuo de dados; real-time | WebSocket, gRPC streaming, Kafka Streams |

**Síncrono (RPC, REST):** cliente espera resposta; simples de raciocinar; pode bloquear.
**Assíncrono (Message Queue):** cliente não espera; desacoplamento temporal; maior complexidade.

**Escolhendo o paradigma:** REST para APIs públicas e CRUD simples; gRPC para comunicação interna e performance crítica; Message Queue para processamento em background e alta carga; WebSocket para chat e notificações real-time.

---

## Resumo: Fundamentos de SD

- **Definição:** coleção de elementos autônomos que aparece como sistema único, com comunicação por mensagens e sem memória compartilhada
- **4 Metas:** Transparência · Abertura · Escalabilidade · Confiabilidade
- **7 Tipos de Transparência:** Acesso · Localização · Migração · Relocação · Replicação · Concorrência · Falha
- **3 Dimensões de Escalabilidade:** Tamanho · Geográfica · Administrativa
- **Arquiteturas:** Cliente-Servidor · P2P · Híbrido
- **Comunicação:** RPC · REST · Message Queues · Pub/Sub · Streaming

**Próxima Aula:** Comunicação entre processos em SD — RPC, RMI, REST e gRPC — paradigmas e trade-offs.

---

**📚 Bibliografia:**
- TANENBAUM, A.S.; VAN STEEN, M. *Distributed Systems: Principles and Paradigms*. 3. ed. Caps. 1 e 2.
- COULOURIS, G. et al. *Distributed Systems: Concepts and Design*. 5. ed. Caps. 1, 2 e 5.
- KLEPPMANN, M. *Designing Data-Intensive Applications*. O'Reilly, 2017. Cap. 1.
- WALDO, J. et al. *A Note on Distributed Computing*. Sun Microsystems, 1994.