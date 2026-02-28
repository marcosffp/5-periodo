# Introdução a Sistemas Distribuídos

-- Link do mateiral: <https://cristianoneto.online/puc/damd/aula03>
> *"A definição mais simples de sistema distribuído é aquela que você não consegue fazer funcionar porque uma máquina da qual você nunca ouviu falar está quebrada."* — Leslie Lamport

---

## O que é um Sistema Distribuído?

A aula apresenta duas definições complementares. Para **Tanenbaum & Van Steen**, é "uma coleção de elementos computacionais autônomos que aparece aos seus usuários como um sistema único e coerente." Para **Coulouris et al.**, é "um sistema no qual componentes localizados em computadores em rede se comunicam e coordenam suas ações apenas através da troca de mensagens."

Três aspectos são centrais nessa definição. **Elementos Autônomos:** cada nó opera independentemente, com seu próprio SO, memória, CPU e relógio local (não sincronizado). **Comunicação por Mensagens:** não existe memória compartilhada física entre nós — toda coordenação acontece via rede, usando sockets TCP/UDP. **Ilusão de Sistema Único:** do ponto de vista do usuário, o sistema parece ser um único todo coeso, como o Google Search, que são milhões de servidores mas parece "um site só".

---

### Sistemas Distribuídos ao Nosso Redor

A aula contextualiza SD com exemplos reais do dia a dia: Google (bilhões de páginas em milhões de servidores), Netflix (CDN distribuída globalmente), WhatsApp (2 bilhões de usuários em tempo real), Pix, Blockchain, jogos como Fortnite e LoL, Spotify, DNS, e até infraestrutura crítica como redes 5G e Smart Cities. O ponto comum em todos eles é que **escondem a complexidade da distribuição do usuário final**.

---

### Por que Construir Sistemas Distribuídos?

Dado que são complexos, por que não usar um supercomputador único? A aula apresenta 6 razões:

**Economia:** commodity hardware em quantidade é mais barato que um mainframe com a mesma capacidade. Google usa servidores "baratos" em larga escala. **Escalabilidade:** um SD cresce adicionando máquinas (escala horizontal), enquanto um supercomputador tem limite físico. **Distribuição Geográfica:** colocar dados e processamento perto dos usuários reduz latência (limitada pela física da luz) e atende regulamentações como a LGPD/GDPR. **Confiabilidade:** redundância elimina o ponto único de falha — se um nó cai, outros continuam. **Compartilhamento de Recursos:** impressoras de rede, bancos de dados corporativos e serviços como Dropbox. **Performance:** tarefas podem ser paralelizadas entre nós, como o MapReduce e a renderização de filmes da Pixar.

---

### As 4 Metas Fundamentais de Design

Todo SD deve perseguir quatro metas, que frequentemente entram em conflito entre si:

**1. Transparência:** ocultar a complexidade da distribuição do usuário. **2. Abertura:** interfaces bem definidas e interoperabilidade entre componentes. **3. Escalabilidade:** crescer mantendo performance. **4. Confiabilidade:** funcionar mesmo com falhas parciais.

O ponto crítico é o **trade-off**: maximizar transparência pode prejudicar performance; escalabilidade extrema pode comprometer consistência. Design de SD é sempre sobre balancear prioridades.

---

### Meta 1: Transparência (7 Tipos)

A transparência visa fazer o SD parecer um sistema único e coeso. Existem 7 tipos:

- **Acesso:** esconde diferenças na representação de dados — acessar um arquivo local ou remoto com a mesma API.
- **Localização:** esconde onde o recurso está fisicamente — uma URL como `api.com/users` não revela em qual datacenter está.
- **Migração:** esconde que um recurso foi movido — uma VM migrada entre hosts sem o usuário perceber.
- **Relocação:** esconde que o recurso está sendo movido enquanto está em uso — Mobile IP, onde você troca de antena sem perder a conexão.
- **Replicação:** esconde que existem múltiplas cópias — CDN com vídeo replicado em vários servidores.
- **Concorrência:** esconde que múltiplos usuários compartilham o recurso — Google Docs com várias pessoas editando ao mesmo tempo.
- **Falha:** esconde que um componente falhou e está se recuperando — Netflix continua funcionando quando um servidor cai (failover automático).

**Exemplo prático — Google Search:** transparência de localização (não sabe em qual datacenter foi processada a busca), de replicação (índice replicado em centenas de servidores) e de falha (10 servidores caem e a busca continua funcionando).

#### Limites da Transparência

Transparência total é impossível e às vezes indesejável. Latência de rede sempre existe, falhas parciais se comportam diferente de falhas locais, e esconder demais pode levar a anti-patterns — uma função que parece fazer uma consulta local mas na verdade é uma chamada remota com 500ms de latência. O princípio de design é: **exponha custos quando necessário**, usando nomes e async/await para deixar claro que uma operação é cara.

---

### Meta 2: Escalabilidade (3 Dimensões)

Um SD pode escalar em três eixos:

**Tamanho (Size Scalability):** adicionar mais usuários e recursos. O desafio são algoritmos centralizados com complexidade O(n²). A solução é sharding, particionamento e load balancing.

**Geográfica (Geographic Scalability):** usuários e recursos espalhados pelo mundo. O desafio é a latência imposta pela velocidade da luz (Brasil-Japão ≈ 300ms). A solução é CDN, edge computing e replicação regional — a Netflix tem cache em ISPs locais no Brasil.

**Administrativa (Administrative Scalability):** gerenciar sistemas que abrangem múltiplas organizações. O desafio são políticas conflitantes, segurança e confiança. A própria Internet é o exemplo: ninguém "gerencia" tudo. A solução é federação, padrões abertos e descentralização.

#### Técnicas para Escalabilidade

- **Sharding/Particionamento:** dividir dados entre servidores (ex.: usuários A-M no servidor1, N-Z no servidor2).
- **Replicação:** copiar dados/serviços para múltiplos nós (master-slave).
- **Caching:** armazenar cópias de dados frequentes perto do usuário (CDN, Redis, browser cache).
- **Assincronismo:** processar tarefas em background com filas de mensagens (RabbitMQ, Kafka).

#### Gargalos de Escalabilidade

Três tipos comuns: **Serviço Centralizado** (um único servidor — solução: load balancer + réplicas), **Dados Centralizados** (um único banco de dados — solução: sharding, NoSQL distribuído como Cassandra) e **Algoritmos Centralizados** (precisam de estado global — solução: gossip protocols, DHT).

**Caso real — Twitter Fail Whale (2008-2010):** o Twitter caía com frequência por ter arquitetura monolítica com MySQL centralizado. A solução foi migrar para microserviços + Cassandra + Redis.

---

### Modelos Arquiteturais

Existem três modelos principais para organizar os componentes de um SD:

**Cliente-Servidor:** o mais comum. Servidor sempre ativo com IP fixo fornece serviços; clientes iniciam a comunicação. Vantagens: simples, fácil gerenciamento e controle de acesso centralizado. Desvantagens: ponto único de falha, gargalo de escalabilidade. Variações: N-Tier (múltiplas camadas) e Microserviços.

**Peer-to-Peer (P2P):** totalmente descentralizado. Todos os nós têm capacidades equivalentes e cada um é cliente E servidor simultaneamente. Vantagens: alta escalabilidade, sem ponto único de falha, resistente à censura. Desvantagens: complexo de implementar, difícil garantir segurança. Exemplo clássico — **BitTorrent:** o arquivo é dividido em pedaços e cada peer baixa de múltiplos outros peers enquanto serve os pedaços que já tem. O paradoxo: quanto mais pessoas baixando, mais rápido fica.

**Híbrido:** combina os dois modelos — um servidor central para discovery/coordenação e P2P para transferência de dados. Exemplos: Napster original, Spotify, Skype atual.

---

### Paradigmas de Comunicação

Como processos em máquinas diferentes se comunicam? A aula apresenta cinco paradigmas:

- **RPC (Remote Procedure Call):** chamada de função remota que parece local, síncrono e bloqueante. Tecnologias: gRPC, Thrift, Java RMI.
- **REST (HTTP/JSON):** baseado em recursos via URLs, stateless e request-response. Usado em APIs Web e microserviços.
- **Message Queues:** assíncrono e desacoplado, no modelo produtor-consumidor. Tecnologias: RabbitMQ, Kafka, AWS SQS.
- **Publish-Subscribe:** broadcast para múltiplos interessados, orientado a eventos. Tecnologias: Redis Pub/Sub, MQTT (IoT).
- **Streaming:** fluxo contínuo de dados em tempo real. Tecnologias: WebSocket, gRPC streaming, Kafka Streams.

A escolha do paradigma segue o requisito: REST para APIs públicas e CRUD simples; gRPC para comunicação interna com performance crítica; Message Queue para processamento em background; WebSocket para chat e notificações em tempo real.

---

### Sobre a Revisão (aula03/revisao)

Infelizmente a página da revisão retornou um **erro de permissão de acesso**, não sendo possível recuperar seu conteúdo. É provável que seja um quiz interativo similar ao da aula 02, com questões sobre os temas desta aula: definição de SD, metas de design, tipos de transparência, escalabilidade e modelos arquiteturais.

---