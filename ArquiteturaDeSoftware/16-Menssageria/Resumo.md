# Arquiteturas Orientadas a Mensagens

## Contexto

Arquitetura de Software é a organização fundamental de um sistema: seus componentes, como eles se relacionam e os princípios que guiam sua evolução. Não é só código ou classes — é uma decisão estrutural difícil de reverter. Sistemas de alta complexidade e volumetria (iFood, Nubank, Netflix) enfrentam desafios sérios de escalabilidade, resiliência e disponibilidade, e a escolha arquitetural define se esses desafios serão gerenciáveis ou catastróficos.

## Evolução Arquitetural: do Monolito ao Assíncrono

**Monolito:** tudo em um único sistema — pedidos, pagamento, estoque, e-mail, etc. Simples no início, mas escalar exige replicar tudo, um bug derruba o sistema inteiro, e equipes grandes disputam o mesmo código. O banco de dados vira gargalo.

**Distribuído ("monolito fatiado"):** os módulos viram serviços separados, comunicando-se via HTTP. Ganha-se escalabilidade por módulo e deploy independente, mas o problema persiste: a comunicação é síncrona — se o serviço de Fraudes cair, o fluxo de compra inteiro para. É chamado de "monolito distribuído" justamente porque o acoplamento temporal permanece alto, gerando cascata de falhas e latência.

**Assíncrono com MOM:** a solução real. Os serviços não se chamam diretamente — eles trocam mensagens por meio de um intermediário chamado broker. O produtor publica a mensagem e segue em frente; os consumidores processam no próprio ritmo. Isso elimina o acoplamento temporal e torna o sistema resiliente.

## MOM — Middleware Orientado a Mensagens

MOM (Message-Oriented Middleware) é a infraestrutura que viabiliza comunicação assíncrona entre serviços. O componente central é o **Message Broker**, que recebe mensagens dos produtores, armazena, roteia e entrega aos consumidores. Analogia útil: funciona como os Correios — o remetente entrega o pacote e não precisa esperar o destinatário estar disponível.

O broker absorve picos de carga, isola falhas entre serviços e implementa mecanismos como retry, dead-letter queue (DLQ para mensagens com falha), idempotência e ordenação.

Há três modelos principais de comunicação via MOM:

**Fila (Point-to-Point):** um produtor envia para uma fila, um único consumidor processa cada mensagem e a remove. Modelo FIFO, garante entrega ("at least once"), ideal para tarefas pesadas e assíncronas. Gera backpressure natural — a fila cresce se o consumidor estiver lento, evitando sobrecarga.

**Tópico (Publish/Subscribe):** o produtor publica em um tópico e todos os assinantes interessados recebem uma cópia. Comunicação 1→N, base para sistemas event-driven. Ideal para notificar múltiplos serviços sobre um mesmo evento (ex.: nova venda notifica e-mail, estoque, cobrança e faturamento simultaneamente).

**Event Streaming:** as mensagens não são removidas após consumo. Consumidores independentes podem reprocessar eventos a qualquer momento. Alto throughput, persistência longa, ordenação garantida. Usado em arquiteturas reativas e big data. Ferramentas: Kafka, Pulsar, Kinesis.

Ferramentas comuns de MOM: RabbitMQ e ActiveMQ (fila/tópico tradicionais), Kafka (streaming), Amazon SQS, Azure Service Bus, Google Cloud Pub/Sub.

## Conclusão

MOM resolve o problema central do distribuído síncrono: o acoplamento temporal. Ele deve ser usado quando se precisa de desacoplamento real, resiliência a falhas, absorção de picos e escalabilidade independente por serviço. Porém, não é bala de prata — introduz desafios de monitoramento, idempotência, governança de mensagens e orquestração.

**Para prova, fixe:** diferença entre fila (um consumidor) e tópico (vários consumidores); o papel do broker; por que o distribuído síncrono é chamado de "monolito distribuído"; e quando usar MOM.