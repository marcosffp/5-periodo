# RESUMO — Arquiteturas Orientadas a Mensagens (MOM)

## 1. Contexto: O que é Arquitetura de Software?

Antes de falar de mensageria, a aula revisita o conceito de Arquitetura de Software, reunindo várias definições de referência:

> **Roger Pressman**: "É a **organização** fundamental de um sistema, composta por **componentes** (módulos), seus **relacionamentos** e os princípios que governam seu projeto e evolução. Ela define a **estrutura geral** do software, como seus elementos interagem entre si e a visão de alto nível do sistema."

> **Mark Richards e Neal Ford** (*Fundamentos da Arquitetura de Software*): "A **estrutura fundamental** de um sistema de software, incluindo os **componentes**, suas **responsabilidades**, as **relações entre esses componentes** e os princípios que orientam sua evolução."

> **Martin Fowler**: "É um conjunto de **decisões** difíceis de serem mudadas no futuro."

> **Bass, Clements e Kazman**: "Arquitetura de Software é a **estrutura ou estruturas de um sistema**, compostas por **elementos de software**, suas propriedades visíveis externamente e as **relações** entre esses elementos."

Três alertas importantes que conectam essas definições:
- **Arquitetura != apenas estrutura estática** (envolve também como o sistema evolui e se comporta).
- **Estrutura != código** (é uma decisão de nível mais alto que a implementação).
- **Elementos != classes apenas** (podem ser serviços, módulos, processos, etc.).
- **Relações != chamadas de método somente** (incluem comunicação assíncrona, eventos, mensagens, etc.).

> **Prof. Filipe Nhimi**: "Softwares de qualquer porte vão apresentar **problemas** como **consequência** de **decisões ruins** tomadas em sua modelagem arquitetural."

### 1.1 Principais desafios (técnicos e de negócio)

| Técnicos | De negócio |
|---|---|
| Aplicações legadas | Segurança |
| Infraestrutura | Observabilidade |
| Integrações | Modelo de negócio |
| Performance | Legislação |
| Escalabilidade | Inovação |
| Disponibilidade | Necessidades dos clientes |
| Confiabilidade | Custos |
| Usabilidade | Prazos |
| Interoperabilidade | Comprometimento dos *stakeholders* |

### 1.2 Por que isso importa: sistemas de alta complexidade e volumetria

Empresas como **iFood, OLX, Nubank, Uber, Google, YouTube, Netflix, WhatsApp e Mercado Livre** representam o que há de mais comum entre aplicações modernas de grande escala: **alta complexidade e alta volumetria**. A escolha arquitetural é justamente o que define se os desafios de escalabilidade, resiliência e disponibilidade serão **gerenciáveis ou catastróficos**.

---

## 2. Estudo de caso: E-commerce XPTO

A aula usa um e-commerce fictício (XPTO) para mostrar, na prática, a evolução de arquitetura monolítica → distribuída → assíncrona (MOM).

### 2.1 Arquitetura Monolítica

```
Usuário --compra--> [ HTTP | PEDIDOS  PAGAMENTO ]
                     [      | ESTOQUE  FRAUDES  ]
                     [      | EMAIL    LOGS     ]
                     [      | NOTA FISCAL       ]
```

Todos os módulos (Pedidos, Pagamento, Estoque, Fraudes, Email, Logs, Nota Fiscal) vivem **dentro do mesmo processo/aplicação**, atrás de uma única porta HTTP.

**Problemas da arquitetura monolítica:**
- **Escalabilidade limitada** — é preciso escalar tudo junto, mesmo que só um módulo precise de mais recursos.
- **Deploy crítico** — uma mudança em qualquer módulo pode derrubar o sistema inteiro.
- **Baixa manutenibilidade** — código grande, acoplado e complexo.
- **Performance instável** — um módulo lento (ex.: envio de e-mail) afeta a performance de todos os outros.
- **Falha única** — qualquer erro interrompe todo o e-commerce.
- **Dificuldade de inovação** — o time fica preso a uma linguagem/tecnologia.
- **Time lento** — muitos desenvolvedores mexendo no mesmo código gera conflitos.
- **Banco único como gargalo** — concorrência, deadlocks, lentidão.

### 2.2 Primeira tentativa: escalonamento com réplicas + Load Balancer

```
Usuário --compra--> [Load Balancer] --> Réplica 1 (monolito completo) --\
                                     --> Réplica 2 (monolito completo) ---> Banco de Dados (compartilhado)
```

Cria-se mais de uma instância do **mesmo monolito** atrás de um balanceador de carga, todas apontando para o **mesmo banco de dados**. Isso melhora um pouco a capacidade de atender requisições simultâneas, mas **não resolve** os problemas estruturais: o banco continua centralizado e compartilhado, e qualquer módulo problemático ainda está acoplado a todos os outros dentro de cada réplica.

### 2.3 Segunda tentativa: Arquitetura Distribuída ("monolito fatiado")

O monolito é **quebrado em serviços distribuídos**: Pedidos → Estoque → Fraudes → Pagamento → Nota Fiscal (fluxo sequencial via HTTP), com o serviço de **Email** sendo chamado a partir de Pedidos.

**Vantagens observadas:**
- Escalabilidade por módulo (cada serviço escala independentemente).
- Deploy independente.
- Isolamento parcial de falhas.
- Manutenção mais simples.
- Observabilidade mais clara.
- Domínios separados.
- Menor acoplamento e redução de falhas em relação ao monolito original.

**Porém — os problemas que persistem:**
- **Alto acoplamento temporal via HTTP** — cada serviço chama o próximo e **espera a resposta**.
- **Cascata de latência e falhas** — se um serviço no meio do fluxo (ex.: Fraudes) ficar lento ou cair, **todo o fluxo de compra trava**.
- **Orquestração rígida** — a sequência de chamadas é fixa e síncrona.
- **Baixa resiliência** — uma falha em qualquer componente compromete um fluxo importante de negócio.
- **Não absorve picos** — sob alta carga, a cadeia síncrona de chamadas amplifica a lentidão.

> Esse resultado é conhecido como **"Monolito Distribuído"**: tecnicamente são vários serviços, mas o **acoplamento temporal** (tudo síncrono, via HTTP) faz o sistema se comportar como um monolito em termos de disponibilidade e resiliência — só que agora distribuído em rede, o que **piora** a latência.

### 2.4 O problema relatado pelo negócio

> "Meu negócio tem perdido vendas em função de erros quando tem sobrecarga na aplicação. Vejo pela plataforma de monitoramento que às vezes alguns serviços falham e todo o fluxo de vendas é comprometido. Temos relatos de lentidão em alguns momentos, e-mails que não são entregues e pedidos processados parcialmente sem a correta emissão da nota fiscal."

> "Preciso que minha arquitetura seja confiável! **Está tudo muito acoplado, tem falhas eventuais e principalmente sob carga.** Não posso continuar perdendo vendas. Está comprometendo o negócio e a credibilidade da plataforma. Decidimos reescrever a solução. Dado o cenário, o que podemos usar em termos de Arquitetura de Software?"

Esse é o gancho para a solução apresentada na aula: **Arquitetura Orientada a Mensagens (MOM)**.

---

## 3. A solução: Middleware Orientado a Mensagens (MOM)

### 3.1 Definição formal

> "**Middleware Orientado a Mensagens (Message-Oriented Middleware – MOM)** é uma infraestrutura tecnológica que permite a aplicação do **estilo arquitetural orientado a mensagens**, em que sistemas comunicam entre si por meio de troca de **mensagens assíncronas**, utilizando mecanismos como **filas, tópicos ou streams**, implementados por um **broker** de mensageria, para **desacoplar emissores e consumidores**."

### 3.2 Como funciona

O MOM atua como uma **camada intermediária** entre quem produz e quem consome mensagens:

```
Publisher --mensagem--> [ Message Broker ] --mensagem--> Subscriber 1
                                            --mensagem--> Subscriber 2
                                            --mensagem--> ...
                                            --mensagem--> Subscriber N
```

Três camadas de entendimento:
- **Infraestrutura**: o próprio MOM (o conceito/middleware).
- **Componente dessa infraestrutura**: o **Message Broker**, que é o elemento concreto que recebe, armazena e roteia as mensagens.
- **Mecanismos de comunicação**: **Queue (fila)**, **Topic (tópico)** e **Stream**, que são as formas pelas quais o broker organiza e entrega as mensagens.

### 3.3 Analogia: o Message Broker como os Correios

> O **Message Broker** funciona como os **Correios**: recebe encomendas e cartas de remetentes, **armazena**, monta **rotas** para **entrega** ou **retirada** pelos destinatários.

Detalhando o papel do Message Broker:
- **Recebe mensagens** de produtores (produtor app → broker).
- **Armazena e persiste mensagens**.
- **Entrega mensagens** aos consumidores (broker → consumer apps).
- **Roteia mensagens** para os destinos corretos.
- **Absorve picos de carga** (atua como buffer entre quem produz e quem consome).
- **Isola falhas entre serviços** (se um consumidor cair, o produtor não é afetado).
- **Permite reprocessamento** (em streams, mensagens podem ser lidas novamente).
- **Implementa mecanismos avançados**: retry, backoff, **dead-letter queue (DLQ)**, idempotência, ordenação, checkpoints, offset tracking.

### 3.4 Os três modelos de comunicação via MOM

#### 3.4.1 Point-to-Point (Fila)

```
Producers --> [ Queue: msg msg msg msg ] --> Consumers (concorrentes)
```

- Um (ou vários) produtor(es) envia(m) mensagens para uma **fila**.
- Permite **múltiplos consumidores concorrentes**, mas **cada mensagem é processada por um único consumidor** (não há duplicação de processamento).
- O consumidor **remove a mensagem da fila** após processá-la.
- Modelo geralmente **FIFO**.
- Padrão mais usado em **tarefas assíncronas de alta carga**.
- Garantia de entrega (**"at least once"**, **"at most once"**, etc.).
- **Acoplamento fraco** entre produtor e consumidor.
- **Backpressure natural** — a fila cresce quando o consumidor está lento, evitando sobrecarga em cascata.

#### 3.4.2 Publish/Subscribe (Tópico)

- O produtor publica em um **tópico**.
- **Todos os assinantes (subscriptions) interessados recebem** uma cópia da mensagem — comunicação **1 → N**.
- Base para sistemas **event-driven** e de **broadcast**.
- **Baixo acoplamento** entre quem publica e quem consome.
- Ideal para **integrações e reações a eventos**.

Existem três variações de padrão Pub/Sub:
- **Many-to-one**: vários publishers enviam para um único tópico/subscription, consumido por um único subscriber.
- **Many-to-many**: vários publishers enviam para um tópico com múltiplas subscriptions, cada uma entregando a múltiplos subscribers.
- **One-to-many**: um publisher envia para um tópico com múltiplas subscriptions, cada uma entregando a um subscriber diferente — ou seja, **a mesma mensagem é entregue a vários destinos independentes**.

#### 3.4.3 Event Streaming

```
Produtor --> [ Kafka Cluster: Broker 1 | Broker 2 | Broker 3 ] --> Consumidor
```

- As **mensagens não são removidas** após o consumo.
- **Consumidores independentes podem reprocessar eventos** a qualquer momento (cada um mantém sua própria posição de leitura).
- Suporta **reprocessamento de eventos** e garante **ordenação**.
- **Alto throughput** e **persistência longa**.
- Fundamental em **arquiteturas reativas e big data**.
- Soluções como **Kafka, Pulsar e Kinesis** usam **logs distribuídos imutáveis**.

### 3.5 Ferramentas comuns de MOM

**RabbitMQ** e **Apache ActiveMQ** (fila/tópico tradicionais), **Apache Kafka** (event streaming), **Amazon SQS**, **Azure Service Bus** e **Google Cloud Pub/Sub**.

---

## 4. Voltando ao E-commerce XPTO: Arquitetura Assíncrona com MOM

```
Usuário --compra--> [ HTTP ] --> [ BROKER ] <--consome/produz eventos--> PEDIDOS
                                                                       --> ESTOQUE
                                                                       --> EMAIL
                                                                       --> FRAUDES
                                                                       --> PAGAMENTO
                                                                       --> NOTA FISCAL
```

Em vez de **Pedidos chamar diretamente Estoque, Fraudes, Pagamento, etc. via HTTP** (como na arquitetura distribuída/monolito fatiado), cada serviço agora **se comunica com o broker**, publicando e consumindo eventos de forma assíncrona. Nenhum serviço chama o outro diretamente — todos **dependem apenas do broker**.

### 4.1 Exemplo prático: evento "Nova Venda"

```
Produtor --mensagem--> [ topico_nova_venda ] --> Consumer: Envio de E-mail
                                              --> Consumer: Reserva de Estoque
                                              --> Consumer: Cobrança
                                              --> Consumer: Faturamento
```

Quando uma venda é criada, o serviço de **Pedidos publica um único evento** no `topico_nova_venda`. Vários consumidores independentes (Envio de E-mail, Reserva de Estoque, Cobrança, Faturamento) **recebem cópias desse evento e processam cada um na sua própria velocidade**, sem que o produtor precise saber quem são, quantos são, ou esperar que terminem. É um exemplo direto do padrão **Publish/Subscribe (1 → N)** resolvendo exatamente o problema relatado pelo dono do e-commerce (e-mails não entregues, pedidos parciais, lentidão sob carga).

---

## 5. Quando usar MOM

- **Desacoplamento entre componentes** — produtores e consumidores não precisam se conhecer.
- **Alta escalabilidade** — cada consumidor escala de forma independente.
- **Comunicação assíncrona** — quem produz não espera quem consome.
- **Tolerância a falhas e resiliência** — a falha de um consumidor não derruba o fluxo inteiro.
- **Processamento distribuído** — múltiplos consumidores podem processar em paralelo.
- **Filas para *workloads* pesados** — absorvem picos de carga (backpressure).

> **A ideia central é substituir integrações síncronas sensíveis** (onde uma falha em qualquer ponto da cadeia compromete o fluxo todo) **por comunicação assíncrona via broker**.

---

## 6. Comparação: Fila vs Tópico vs Event Streaming

| Característica | Fila (Point-to-Point) | Tópico (Pub/Sub) | Event Streaming |
|---|---|---|---|
| **Cardinalidade** | 1 mensagem → 1 consumidor (entre vários concorrentes) | 1 mensagem → N assinantes | N consumidores independentes leem o mesmo log |
| **Remoção da mensagem** | Removida após o consumo | Entregue a cada assinante (modelo de entrega) | Não removida — persistência longa |
| **Reprocessamento** | Não | Limitado | Sim, cada consumidor controla seu próprio offset |
| **Ordenação** | Geralmente FIFO | Depende da implementação | Garantida (ex.: por partição no Kafka) |
| **Uso típico** | Tarefas assíncronas de alta carga | Notificar múltiplos serviços sobre um mesmo evento | Streaming, big data, arquiteturas reativas |
| **Exemplos de ferramentas** | RabbitMQ (fila), Amazon SQS | RabbitMQ (tópico), Google Cloud Pub/Sub, Azure Service Bus | Apache Kafka, Pulsar, Kinesis |

---

## 7. Conclusão

> **"No Silver Bullet"** (Fred Brooks, 1931–2022): **não existe tecnologia capaz de eliminar de forma mágica a complexidade inerente do software.**

> **MOM não é uma solução mágica**, e sim **uma decisão arquitetural** que traz benefícios claros quando aplicada corretamente — mas que **também introduz novos desafios** como **monitoramento, idempotência, orquestração e governança de mensagens**.

Resumindo a evolução vista na aula:
- **Monolito**: simples, mas escalabilidade, deploy e manutenção ruins; banco como gargalo.
- **Distribuído ("monolito fatiado")**: melhora escalabilidade e deploy por módulo, **mas mantém acoplamento temporal síncrono via HTTP**, gerando o anti-padrão "Monolito Distribuído" — cascata de falhas e latência.
- **Assíncrono com MOM**: elimina o acoplamento temporal substituindo chamadas síncronas por **troca de mensagens via broker** (fila, tópico ou stream), tornando o sistema **resiliente, escalável e tolerante a falhas** — ao custo de mais complexidade operacional (monitoramento, idempotência, ordenação, governança).

---

## 8. O que mais cai em prova

- **Diferença entre Fila (Point-to-Point) e Tópico (Publish/Subscribe)**: na fila, **uma mensagem é processada por um único consumidor**; no tópico, **todos os assinantes recebem uma cópia** (comunicação 1→N).
- **Event Streaming** se diferencia dos dois anteriores porque **as mensagens não são removidas** — permitindo **reprocessamento** por consumidores independentes (Kafka, Pulsar, Kinesis).
- **O papel do Message Broker**: recebe, armazena, roteia e entrega mensagens; absorve picos de carga; isola falhas; implementa retry, DLQ, idempotência, ordenação. Analogia: **Correios**.
- **Por que a arquitetura distribuída síncrona (via HTTP) é chamada de "Monolito Distribuído"**: porque, apesar de ser tecnicamente composta por serviços separados, o **alto acoplamento temporal** faz com que uma falha em qualquer serviço da cadeia **comprometa todo o fluxo de negócio** — o sistema se comporta como um monolito em termos de disponibilidade.
- **Quando usar MOM**: desacoplamento, escalabilidade, comunicação assíncrona, tolerância a falhas, processamento distribuído, absorção de picos (backpressure).
- **MOM não é bala de prata ("No Silver Bullet")**: resolve o acoplamento temporal, mas introduz desafios de monitoramento, idempotência, orquestração e governança de mensagens.
- **Ferramentas associadas a cada modelo**: RabbitMQ/ActiveMQ/SQS (fila e tópico tradicionais), Kafka/Pulsar/Kinesis (event streaming), Azure Service Bus e Google Cloud Pub/Sub (mensageria gerenciada em nuvem, fila e tópico).
