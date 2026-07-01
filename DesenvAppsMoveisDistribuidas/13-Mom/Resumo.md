# Middlewares e MOM — Resumo para Prova

## O que é Middleware e por que existe

Middleware é uma camada de software que fica entre o sistema operacional e as aplicações distribuídas. Ele resolve o problema da heterogeneidade: permite que um cliente Java fale com um servidor Python, que sistemas em máquinas diferentes se comuniquem sem saber os IPs um do outro, e que serviços continuem funcionando mesmo com protocolos e plataformas distintos.

As três transparências que o middleware oferece:
- **Transparência de acesso** — chamadas locais e remotas têm a mesma sintaxe.
- **Transparência de localização** — a aplicação não sabe o IP do serviço; o middleware resolve.
- **Independência de plataforma** — Java fala com C++, Python fala com Go.

---

## As Três Famílias de Middleware

**ORB-Based (Object Request Broker):** objetos distribuídos se invocam independentemente de linguagem e SO. O broker medeia cada invocação via IDL neutro. Exemplo: CORBA. Alto acoplamento, síncrono.

**RPC-Based (Remote Procedure Call):** abstrai comunicação como chamadas de procedimento locais. Síncrono por padrão. Stubs gerados de contrato (IDL/Protobuf). Exemplos: gRPC, Apache Thrift. Alto acoplamento.

**MOM (Message-Oriented Middleware):** comunicação via troca de mensagens assíncronas em filas ou tópicos. Produtor e consumidor completamente desacoplados. Exemplos: RabbitMQ, Kafka, AWS SQS. Baixo acoplamento, assíncrono.

**Hierarquia de acoplamento:** CORBA (mais acoplado) → gRPC → RabbitMQ → Kafka (menos acoplado).

---

## ORB e CORBA — O que cai na prova

CORBA (1991, padronizado pelo OMG) permite que objetos distribuídos se invoquem independentemente de linguagem ou SO. O coração é o **ORB (Object Request Broker)**, que roteia cada invocação via protocolo **IIOP** (Internet Inter-ORB Protocol, sobre TCP/IP).

O fluxo completo:
```
Cliente Java → Stub (Proxy) → ORB → IIOP/TCP → Skeleton → Servidor C++
```

O contrato entre cliente e servidor é definido em **IDL (Interface Definition Language)** — uma linguagem neutra. O compilador IDL gera:
- **Stub** → fica no **cliente**, simula o objeto remoto localmente.
- **Skeleton** → fica no **servidor**, recebe a chamada do ORB e invoca a implementação real.

**Ponto de prova:** Stub ≠ Skeleton. Stub é do lado do cliente. Skeleton é do lado do servidor. Confundir é o erro mais comum.

CORBA caiu em desuso por excesso de complexidade, mas o conceito de IDL + stubs foi herdado pelo gRPC (Protobuf) e Apache Thrift.

---

## MOM — O Desacoplamento Triplo

O princípio central do MOM é o **desacoplamento triplo** (Eugster et al., 2003):

- **Desacoplamento de espaço:** produtor não conhece o endereço do consumidor. Zero acoplamento de IP/endpoint.
- **Desacoplamento de tempo:** o consumidor pode estar offline quando a mensagem é publicada. O broker armazena até que seja processada.
- **Desacoplamento de sincronização:** o produtor não bloqueia esperando resposta. Envia e continua.

Compare com RPC síncrono:
```
RPC:   Cliente → BLOQUEADO → espera resposta do Servidor
MOM:   Produtor → publica → livre! | Broker guarda | Consumidor processa quando quiser
```

**Para a prova:** saiba associar cada tipo de desacoplamento à sua definição. A opção "consumidor pode estar offline" = desacoplamento de **tempo**. "Produtor não conhece IP" = desacoplamento de **espaço**. "Produtor não bloqueia" = desacoplamento de **sincronização**.

---

## Dois Modelos de MOM: Filas (P2P) vs. Tópicos (Pub/Sub)

### Filas Point-to-Point (P2P)
Uma mensagem tem **exatamente um consumidor**. Após leitura com ACK, é removida da fila. Garante processamento único.

```
Produtor → FILA → Consumidor A  (Consumidor B nunca recebe essa mensagem)
```

- Uso: processamento de pedidos, job queues, tarefas em background, pagamentos.
- Escala via **Competing Consumers**: múltiplos workers competem pelas mensagens da fila. Cada tarefa vai ao primeiro disponível.
- **Sem ACK → reentrega:** se o consumidor não confirmar (ACK), o broker reentrega a mensagem.

### Tópicos Pub/Sub
Mensagem publicada é entregue a **todos os assinantes**. Cada um recebe cópia independente. Produtor não conhece quem consome.

```
Publisher → TÓPICO orders → StockService
                           → EmailService
                           → AuditService
```

- Uso: notificações em tempo real, múltiplos serviços downstream, IoT (MQTT), Event-Driven Architecture.
- Adicionar um novo assinante não afeta os existentes.

### Tabela comparativa

| Característica | Fila P2P | Tópico Pub/Sub |
|---|---|---|
| Destinatários | Exatamente 1 | N assinantes |
| Mensagem após consumo | Removida | Retida por TTL |
| Reprocessamento | Não | Limitado |
| Ordenação | Global (1 consumer) | Não garantida |
| Exemplo | RabbitMQ Direct, AWS SQS | RabbitMQ Fanout, MQTT |

---

## RabbitMQ — Arquitetura e Exchange

RabbitMQ implementa **AMQP 0-9-1**. O modelo de roteamento é:

```
Producer → Exchange → Binding → Queue → Consumer
```

O **Exchange** é quem decide para qual(is) fila(s) a mensagem vai, baseado na **routing key** e nas **bindings** configuradas.

### Tipos de Exchange

**Direct Exchange:** roteia para a fila cuja binding key é **idêntica** à routing key. Ex.: `order.created` vai para Queue A.

**Fanout Exchange:** **ignora** a routing key. Distribui cópia a **todas as filas vinculadas**. Padrão broadcast — equivale ao Pub/Sub.

**Topic Exchange:** roteamento por **padrão com curinga**: `order.*` captura `order.created` e `order.paid`; `#.critical` captura qualquer coisa que termine em `.critical`.

**Headers Exchange:** roteamento por atributos do cabeçalho AMQP, sem usar routing key. Raramente usado.

**Para a prova:** Fanout = broadcast para todas. Direct = correspondência exata. Topic = padrão com `*` e `#`.

---

## Apache Kafka — Log Distribuído

Kafka foi criado no LinkedIn em 2011 para processar bilhões de eventos de log por dia. A diferença fundamental em relação ao MOM clássico: Kafka usa um **log de commit distribuído imutável**, não uma fila de destruição.

### A abstração do Log

Mensagens são escritas em sequência num log **append-only**, particionado. Mensagens **nunca são apagadas por consumo** — permanecem pelo período de retenção configurado (padrão: 7 dias).

Cada consumidor controla seu próprio **offset** (posição no log) e pode reler, processar em paralelo ou retroceder — impossível num MOM clássico.

```
Tópico "orders" · 3 partições:
P0: [0][1][2][3] ← novo
P1: [0][1]
P2: [0][1][2]

Consumer Group A → offset=2 em P0 (leu até msg 2)
Consumer Group B → offset=0 em P0 (ainda não leu nada)
```

### Componentes Principais

- **Producer:** publica em tópicos. Escolhe partição por chave ou round-robin.
- **Consumer Group:** cada partição vai a exatamente 1 membro do grupo. Paralelismo automático. Grupos diferentes são independentes entre si.
- **Broker/Cluster:** armazena partições. Réplicas garantem durabilidade.
- **Topic + Partition:** tópico = canal lógico. Partições = paralelismo e escala horizontal.

### Por que o offset é poderoso

- **Group A** pode estar no offset 10 enquanto **Group B** está no offset 3 — o log não é afetado.
- Um novo serviço pode entrar e reler o histórico completo desde o offset 0.
- Após um bug, é possível fazer **replay** e reprocessar eventos passados.

---

## RabbitMQ vs. Kafka — Quando usar cada um

| Dimensão | RabbitMQ | Apache Kafka |
|---|---|---|
| Modelo central | Fila com roteamento por Exchange | Log distribuído particionado |
| Mensagem após consumo | Removida após ACK | Retida por período configurável |
| Múltiplos consumers | Competing consumers (load balance) | Consumer groups independentes; reprocessamento |
| Ordenação | Global por fila com 1 consumer | Garantida **por partição**, não globalmente |
| Protocolos | AMQP, STOMP, MQTT | Protocolo Kafka sobre TCP |
| Complexidade operacional | Média | Alta |

**Use RabbitMQ quando:**
- Roteamento complexo por routing key, headers ou padrões
- Task queue — cada tarefa processada exatamente 1×
- Mensagens com TTL ou prioridade
- DLQ e retry granular com dead-letter exchange

**Use Kafka quando:**
- Alto throughput contínuo (telemetria, log, clickstream)
- Múltiplos sistemas precisam reler os mesmos eventos (reprocessamento, auditoria)
- Event sourcing ou CQRS
- Integração de dados com CDC (Change Data Capture)

**Atenção:** Kafka **não garante** ordem global — garante apenas ordem **por partição**. Isso é frequentemente cobrado em provas.

---

## Padrões Arquiteturais com MOM

### Competing Consumers
Múltiplos workers competem pelas mensagens de uma fila. Cada mensagem vai a **exatamente um**. Escala horizontal de workers sem alterar o produtor.

### Event-Driven Architecture (EDA)
Serviços comunicam-se apenas por eventos em tópicos. Nenhum serviço sabe da existência dos outros — máximo desacoplamento. O OrderService publica `order.created` e os demais (Stock, Email, Audit) consomem de forma independente.

### Dead Letter Queue (DLQ)
Mensagens não processadas (erro, expiradas, rejeitadas após N tentativas) vão para uma fila especial de análise. Essencial em produção — evita perda silenciosa de dados.

```
Fila Principal → ❌ falha N tentativas → DLQ → análise / reprocessamento manual
```

No RabbitMQ: configura-se `x-dead-letter-exchange` na fila original.
No Kafka: cria-se retry topics (`payment-retry-1/2/3`) com backoff exponencial antes do `payment-dlq`.

### CQRS + Event Sourcing
Separação de leitura (Query) e escrita (Command). Eventos são armazenados como log imutável. O estado atual é derivado reproduzindo os eventos desde o início. Kafka é a plataforma natural para isso.

---

## Armadilhas comuns para a prova

- Usar **fila P2P** quando múltiplos serviços precisam do evento → correto é **tópico**.
- Kafka garante ordem **por partição**, não globalmente.
- Não configurar DLQ → falhas silenciosas em produção.
- Escolher Kafka para workloads simples → overhead operacional desnecessário.
- Tratar Kafka como banco de dados primário — é log, não SGBD.
- Confundir **stub (cliente)** com **skeleton (servidor)** no CORBA.
- Desacoplamento de **tempo** ≠ desacoplamento de **sincronização**: tempo = consumidor pode estar offline; sincronização = produtor não bloqueia.

---

## Hierarquia geral dos middlewares (do mais ao menos acoplado)

| Tipo | Paradigma | Acoplamento | Exemplos |
|---|---|---|---|
| ORB | Objeto distribuído / IDL | Alto | CORBA, Java RMI |
| RPC | Chamada de função remota | Alto | gRPC, Apache Thrift |
| MOM Filas | Point-to-point assíncrono | Baixo–Médio | RabbitMQ, AWS SQS |
| MOM Log/Tópicos | Log / Pub-Sub distribuído | Muito Baixo | Apache Kafka, AWS Kinesis |
