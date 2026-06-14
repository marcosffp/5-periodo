# RESUMO — Estilos Arquiteturais: Pipes-and-Filters, Publish/Subscribe e Space-Based

## 1. Contexto Geral

Dentro do panorama dos "10 tipos de arquitetura de software", esta aula foca em três estilos com propósitos bem diferentes:

- **Pipes-and-Filters**: organiza o **processamento de dados** em uma sequência de etapas independentes.
- **Publish/Subscribe (Pub/Sub)**: organiza a **comunicação** entre componentes através de eventos/mensagens, sem acoplamento direto entre quem envia e quem recebe.
- **Space-Based (SBA)**: organiza o **estado e o processamento de toda a aplicação**, distribuindo-os entre vários nós para eliminar o banco de dados centralizado como gargalo.

Em outras palavras: Pipes-and-Filters resolve "como processar dados em etapas", Pub/Sub resolve "como componentes se avisam sobre eventos sem se conhecerem", e Space-Based resolve "como escalar o sistema inteiro sem depender de um único banco/servidor central".

---

## 2. Pipes-and-Filters

### 2.1 Definição formal

> **Bass, Clements e Kazman**: "o estilo arquitetural Pipes-and-Filters é caracterizado por uma sequência de componentes independentes (*filters*) conectados por canais de comunicação (*pipes*), onde cada filtro processa dados de entrada e produz dados de saída para o próximo estágio do fluxo."

A ideia central é **decompor um processamento complexo em uma cadeia de transformações simples e independentes**, onde a saída de uma etapa é a entrada da próxima.

### 2.2 Analogias para fixar o conceito

- **Café por filtragem (pour-over)**: a água passa por etapas sequenciais (moagem → água quente → filtro de papel → extração) até chegar ao café pronto — cada etapa transforma o "dado" recebido da etapa anterior.
- **Estação de tratamento de água**: água bruta → coagulação/floculação → sedimentação → filtro → desinfecção → armazenamento → consumo. Cada estágio é um "filtro" especializado, e a água (o dado) flui entre eles por "tubulações".
- **Sistema hidráulico de freios de um carro**: as tubulações (pipes) literalmente transportam o fluido entre os componentes (filtros), ilustrando o papel dos pipes como canais de transporte.

### 2.3 Componentes do estilo

| Componente | Papel |
|---|---|
| **Filtros (filters)** | Cada filtro executa uma tarefa específica e independente — transformar, validar ou processar dados antes de encaminhá-los ao próximo estágio. É comum existirem múltiplos níveis de filtros: o primeiro processa os dados vindos da bomba e os repassa ao segundo filtro, que continua o processamento, e assim por diante. |
| **Tubulações (pipes)** | Servem como canais para a movimentação de dados entre os filtros, conectando cada componente em sequência e garantindo transferência fluida. Nos diagramas, são representadas por setas conectando os componentes. |
| **Bombas (pumps)** | Iniciam o processo atuando como fontes de dados — injetam dados no sistema e dão início ao fluxo através do pipeline (ex.: o repositório de código-fonte que alimenta uma pipeline de CI/CD). |
| **Destinos (sinks)** | São os pontos finais onde os dados processados são coletados ou utilizados. Após passar por todos os filtros, os dados chegam ao destino, completando sua jornada no pipeline. |
| **Processamento paralelo** | A arquitetura também suporta uma estrutura paralela, onde dois (ou mais) pipelines independentes operam lado a lado. Cada pipeline tem sua própria bomba, processa os dados através de uma série de filtros e termina em destinos separados — permitindo processar simultaneamente diferentes fluxos de dados sem conflito. |

**Fluxo geral**: `Pump → Pipe → Filter → Pipe → Filter → ... → Pipe → Sink`

### 2.4 Exemplos práticos

1. **Pipeline de CI/CD (Azure DevOps)**: estágios como "Build Acadêmico API" → "Backup PRD" → "Deploy PRD" / "RollBack PRD" representam **filtros**, conectados por **pipes** (as transições entre estágios). O repositório de código-fonte (Azure Repos) funciona como a **bomba (pump)** que injeta os dados (o código) no pipeline.

2. **Logstash (pilha ELK)**: "O Logstash é um pipeline open source de processamento de dados do lado do servidor que faz a ingestão de dados de inúmeras fontes, transforma-os e envia-os para o seu 'esconderijo' favorito." Fluxo: `LOG → Logstash (processamento de dados) → Elasticsearch (armazenamento) → Kibana (visualização)`. Aqui o Logstash é o **filtro central** que recebe logs (pump), transforma-os e os entrega ao destino (sink = Elasticsearch/Kibana).

3. **Pipeline de Middlewares do ASP.NET Core**: a requisição HTTP do cliente passa por uma sequência de middlewares (filtros), cada um executando uma responsabilidade específica, antes de chegar ao endpoint/controlador, e a resposta volta percorrendo os mesmos middlewares na **ordem inversa**.

   Fluxo: `Cliente → Requisição HTTP → [Logging → Autenticação → Autorização → Tratamento de Erros → Endpoint/Controlador] → Resposta HTTP`

   ```csharp
   var builder = WebApplication.CreateBuilder(args);
   var app = builder.Build();

   // Ordem dos middlewares importa!
   app.UseMiddleware<LoggingMiddleware>();
   app.UseMiddleware<AuthenticationMiddleware>();
   app.UseMiddleware<AuthorizationMiddleware>();
   app.UseMiddleware<ExceptionHandlingMiddleware>();

   app.MapControllers();
   app.Run();
   ```

   Como funciona:
   1. A requisição HTTP entra no pipeline.
   2. Cada middleware executa sua lógica.
   3. O middleware chama o próximo com `await next()`.
   4. A resposta retorna na ordem inversa, passando pelos middlewares novamente.

### 2.5 Vantagens e Desvantagens

| Vantagens | Desvantagens |
|---|---|
| Alta modularidade | Overhead de transformação/transporte de dados entre os filtros |
| Reutilização de filtros em outros pipelines | Pode aumentar a latência total (cada etapa soma tempo) |
| Facilidade de manutenção e evolução (filtros isolados) | Difícil gerenciamento de estado global/compartilhado |
| Possibilidade de processamento paralelo | Nem todo problema se encaixa em um fluxo sequencial |
| Boa composição de pipelines (encadear/recombinar filtros) | |

### 2.6 Trade-offs do Estilo

1. **Manutenibilidade vs Performance**: a separação em vários filtros melhora a manutenção e a evolução do sistema (cada filtro pode ser alterado isoladamente), **porém** cada etapa adicional pode aumentar o overhead de processamento — quanto mais filtros, mais "paradas" no caminho dos dados.

2. **Paralelismo vs Consistência de Estado**: o processamento paralelo (pipelines independentes rodando lado a lado) melhora o desempenho geral, **porém** dificulta o gerenciamento de estado compartilhado — não há um lugar único e confiável para armazenar/sincronizar estado entre as execuções paralelas.

3. **Escalabilidade vs Complexidade de Coordenação**: paralelizar os filtros melhora o throughput (mais dados processados por unidade de tempo), **mas** aumenta os desafios de sincronização e observabilidade — é mais difícil rastrear onde um dado está e detectar falhas em um fluxo distribuído entre vários filtros paralelos.

### 2.7 Quando o estilo se aplica

> **Nem sempre o sistema inteiro adota Pipes-and-Filters como estilo arquitetural predominante.** Muitas vezes o padrão aparece apenas em **partes específicas da arquitetura**, como processamento de requisições HTTP (middlewares), integração de sistemas, *streaming* de dados ou *pipelines* de build/CI-CD.

Ou seja: é comum um sistema ser, por exemplo, predominantemente em camadas ou microsserviços, mas usar Pipes-and-Filters internamente em um módulo específico (como o pipeline de middlewares de um framework web).

### 2.8 Conclusão (Pipes-and-Filters)

> O estilo arquitetural **Pipes-and-Filters favorece modularidade, reutilização e composição de processamento** através do encadeamento de componentes independentes. É especialmente adequado para **fluxos de transformação de dados, pipelines e processamento sequencial**, **porém pode introduzir overhead, aumento de latência e maior complexidade operacional conforme o pipeline cresce.**

---

## 3. Publish/Subscribe (Pub/Sub)

### 3.1 Classificação do padrão

> **Publish/Subscribe não é tradicionalmente classificado como um estilo arquitetural clássico**, mas em algumas soluções seu uso pode se tornar tão predominante que passa a definir características arquiteturais importantes do sistema.

Resumindo: Pub/Sub é, antes de tudo, um **padrão de comunicação** — muito usado em **arquiteturas orientadas a eventos (EDA — Event Driven Architecture)** — e não um "estilo" no mesmo sentido de Pipes-and-Filters ou Space-Based. Ainda assim, ele é apresentado aqui porque influencia fortemente a forma como os componentes de um sistema se comunicam.

### 3.2 Como funciona

O fluxo básico do Pub/Sub envolve três papéis:

- **Publisher (publicador)**: produz e envia mensagens/eventos para um canal.
- **Channel/Topic (broker / canal de tópico)**: intermediário que recebe as mensagens dos publishers e as distribui aos interessados. É o **Pub/Sub message broker**.
- **Subscriber (assinante)**: registra interesse em um tópico/canal e recebe automaticamente as mensagens publicadas nele, **sem se comunicar diretamente com o publisher**.

Esquema: `Publishers → Channel/Topic (broker) → Subscribers (1, 2, 3, ...)`

Um mesmo publisher pode alimentar múltiplos tópicos, e um mesmo tópico pode ter múltiplos subscribers — todos recebem a mensagem de forma independente e assíncrona.

### 3.3 Vantagens e Desvantagens

| Vantagens | Desvantagens |
|---|---|
| **Baixo acoplamento**: publisher e subscriber não se conhecem nem dependem um do outro | **Dificulta o rastreamento do fluxo** de uma mensagem/evento através do sistema |
| **Comunicação assíncrona**: o publisher não precisa esperar o subscriber processar | **Depuração (debugging) mais difícil** — erros podem ocorrer "em algum lugar" depois do envio |
| Favorece **escalabilidade**, **flexibilidade** e **integração distribuída** | Maior **assincronicidade** pode dificultar o **controle de consistência** entre componentes |

### 3.4 Conclusão (antecipada — será detalhado em EDA)

> O modelo Publish-Subscribe promove **baixo acoplamento e comunicação assíncrona** entre produtores e consumidores de eventos, **favorecendo escalabilidade, flexibilidade e integração distribuída**. **Entretanto**, o aumento da assincronicidade pode **dificultar rastreamento de fluxo**, depuração e controle de consistência entre os componentes do sistema.

> Este padrão será aprofundado no módulo de **EDA (Event Driven Architecture)**.

---

## 4. Space-Based Architecture (SBA)

### 4.1 Definição formal

> **Mark Richards e Neal Ford** (em *Fundamentos da Arquitetura de Software*): "**Space-Based** é um estilo arquitetural **projetado para alcançar alta escalabilidade e alta disponibilidade através da remoção de restrições de banco de dados centralizados**."

### 4.2 Por que o SBA existe: o problema da arquitetura tradicional

Para entender o SBA, é útil comparar dois extremos:

- **Arquitetura monolítica (centralizada)**: simples, poucos recursos, **porém com vários pontos únicos de falha** — todas as requisições passam por um único servidor de aplicação, que por sua vez depende de um único banco de dados centralizado.
- **Arquitetura distribuída**: mais complexa, **porém com redundância, alta disponibilidade e tolerância a falhas**.

Os problemas centrais da arquitetura tradicional centralizada são:
- O **banco de dados vira gargalo** (todas as requisições competem pelo mesmo recurso).
- **Baixa escalabilidade horizontal**.
- **Ponto único de falha** (se o servidor/banco cai, o sistema todo cai).
- **Indisponibilidade afeta todo o sistema**.
- **Maior latência** (toda leitura/escrita depende de ir ao disco/banco).

O SBA nasce exatamente para resolver esses problemas.

### 4.3 Origem histórica

A **arquitetura baseada em espaço (SBA)** foi originalmente **inventada e desenvolvida na Microsoft em 1997-98**. Internamente, era conhecida como a **plataforma de cache distribuído Youkon (YDC)**. Os primeiros grandes projetos web baseados nela foram o **MSN Live Search** (lançado em setembro de 1999).

### 4.4 Ideia central

> "Espalhar as informações e o estado da aplicação entre múltiplos nós do sistema, **ao invés de manter tudo centralizado em um único servidor ou banco de dados**."

Em vez de buscar os dados sempre em um banco centralizado no disco, **o sistema mantém partes dos dados diretamente na memória RAM de vários servidores distribuídos**.

Princípios-chave:
- **Distribuir processamento**
- **Distribuir estado**
- **Evitar o banco central como gargalo**
- **Manter dados próximos da aplicação** (na memória, não no disco)
- **Utilizar memória RAM distribuída**

**Exemplos de dados que vivem nesse "espaço"**: sessão de usuários, carrinho de compras, saldo temporário, posição de jogador online, autenticação, conexões ativas, etc. — ou seja, dados de **estado de curta duração e alto acesso**, que não precisam (no momento) estar em um banco relacional.

### 4.5 Características do estilo

- Altíssima escalabilidade (horizontal)
- Alta disponibilidade
- Baixa latência (dados ficam na memória)
- Forte uso de memória
- Processamento distribuído
- Redução/eliminação de gargalos centrais
- Tolerância a falhas
- Elasticidade sob demanda

### 4.6 Componentes (Diagrama SBA)

| Componente | Papel |
|---|---|
| **Processing Unit (PU)** | Nó da aplicação. Executa a lógica de negócio e mantém parte do estado em memória (cache local + componentes de processamento). |
| **In-Memory Data Grid** | Memória RAM distribuída entre as PUs — é o "coração" do espaço de dados. Armazena dados como sessões, carrinhos, estoques em tempo real, tokens de autenticação. |
| **Data Replication Engine** | Garante que o dado que está na RAM da "Máquina A" seja replicado/copiado para a "Máquina B", mantendo as cópias sincronizadas entre PUs. |
| **Virtualized Middleware** | Camada de serviços distribuídos que engloba os quatro "grids" abaixo, fornecendo replicação, sincronização, balanceamento e comunicação entre nós. |
| **Messaging Grid** | Infraestrutura responsável pela comunicação assíncrona e troca de mensagens entre as PUs e demais componentes distribuídos. **É ele quem cria o conceito de "espaço de dados"**. |
| **Data Grid** | O conjunto distribuído das memórias RAM utilizadas pelas PUs para armazenar e compartilhar estado e dados do sistema. |
| **Processing Grid** | Distribui a carga computacional entre múltiplas Processing Units para aumentar throughput e escalabilidade. |
| **Deployment Manager** | Monitora a saúde das instâncias de PU e escala o sistema automaticamente (sobe/derruba PUs conforme a demanda). |
| **Data Reader / Data Writer** | Componentes que leem eventos do sistema e persistem (ou sincronizam) dados de longo prazo no **Database** tradicional — o banco deixa de ser o ponto central de toda operação e passa a ser usado para persistência durável/assíncrona. |

### 4.7 Como o SBA funciona (passo a passo)

1. As requisições chegam e são distribuídas entre múltiplas Processing Units.
2. Cada PU processa localmente, usando dados próximos (cache/memória).
3. O Virtualized Middleware garante replicação, sincronização, balanceamento e comunicação eficiente entre nós.
4. O Data Grid mantém o estado distribuído na memória RAM, sem depender de um banco central como gargalo.
5. A camada de mensageria/event streaming permite comunicação assíncrona e desacoplada (filas/tópicos) entre PUs e outros sistemas.

### 4.8 Exemplo prático: E-commerce de alta escala (Black Friday)

Cenário: um grande e-commerce precisa suportar picos massivos de acesso durante uma Black Friday.

- Cada **Processing Unit (PU)** roda módulos de **Catálogo** e **Carrinho**, com seu próprio **In-Memory Data Grid** (cache de sessões, carrinhos, estoque em tempo real) e um **Data Replication Engine** que replica esse estado para as outras PUs.
- O **Virtualized Middleware** coordena Messaging Grid (eventos como "pedido criado", "pagamento aprovado"), Data Grid (estado distribuído), Processing Grid (execução distribuída, ex.: cálculo de recomendações) e Deployment Manager (monitoramento e auto-scaling).

**Fluxo de um pedido**:
1. Usuário adiciona produtos ao carrinho e finaliza o pedido.
2. A requisição chega a uma Processing Unit (PU).
3. A PU grava o estado (ex.: carrinho, pedido) no In-Memory Data Grid.
4. A mudança é replicada para outras PUs pelo Data Replication Engine.
5. Um evento "Pedido Criado" é publicado no Messaging Grid.
6. Outras PUs ou serviços consomem esse evento e executam ações em paralelo (ex.: reserva de estoque, cálculo de frete, envio de e-mail) usando o Processing Grid.
7. Dados importantes são persistidos no banco via Data Writer (persistência de longo prazo).
8. O usuário recebe a resposta; o sistema continua escalando conforme a carga.

**Por que essa arquitetura?**
- **Escalabilidade**: basta adicionar mais PUs.
- **Baixa latência**: dados ficam na memória, próximos do processamento.
- **Alta disponibilidade**: dados replicados entre nós.
- **Resiliência**: a falha de um nó não derruba o sistema.
- **Alta performance**: processamento paralelo e eventos assíncronos.

### 4.9 Trade-offs do Estilo

1. **Disponibilidade vs Consistência (modelo AP do CAP)**: o estilo **assume reduzir a consistência forte imediata** para **priorizar disponibilidade mesmo quando ocorrer uma partição na rede** — ou seja, é um modelo **AP (Availability + Partition Tolerance)** do teorema CAP, em detrimento da consistência forte (C).

2. **Performance vs Consumo de Memória**: ao priorizar **manter dados próximos da aplicação** (na RAM), tem-se um **aumento de performance**, mas **consequentemente um maior consumo de memória** — afinal, os dados (e suas réplicas) precisam estar carregados em RAM em vários nós simultaneamente.

3. **Latência vs Complexidade de Sincronização**: a latência é reduzida ao aproximar os dados do processamento, **porém** isso **aumenta o desafio de manter múltiplas cópias sincronizadas** entre os nós (replicação de estado distribuído).

### 4.10 Quando usar / Quando não usar

| Quando usar | Quando NÃO usar |
|---|---|
| **Marketplaces**: grande volume de usuários e transações (catálogo, busca, carrinho, pedidos distribuídos globalmente) | **Sistema pequeno**: overhead e complexidade do SBA não trazem benefícios reais |
| **Sistemas financeiros**: negociação, consultas e operações em alta frequência, com tolerância a falhas e baixa latência | **Baixa concorrência**: poucos usuários/requisições → uma arquitetura mais simples já é suficiente |
| **Games online**: ambientes massivos e persistentes, com muitos jogadores simultâneos e estado distribuído | **Consistência forte obrigatória**: se o negócio exige ACID estrito em tempo real, o SBA pode não ser adequado |
| **Streaming**: entrega de conteúdo para milhões de usuários com baixa latência e alta disponibilidade | **Equipe pequena**: SBA requer conhecimento especializado para operar e manter ambientes distribuídos complexos |
| **IoT**: milhões de dispositivos enviando/recebendo dados continuamente | **Orçamento limitado**: infraestrutura distribuída, ferramentas e operação têm custo mais alto |
| **Sistemas com pico extremo**: eventos sazonais/campanhas que geram picos massivos de acesso | |
| **Aplicações SaaS massivas**: multi-tenant, com necessidade de crescimento contínuo e elasticidade | |

> **Em resumo**: use SBA quando o objetivo principal for **escalar sem limites**, garantindo **alta disponibilidade e resiliência**. Evite SBA quando o **custo, a complexidade e os requisitos de consistência** superarem os benefícios de escalabilidade e resiliência.

### 4.11 Conclusão (Space-Based)

> O estilo arquitetural **Space-Based busca eliminar gargalos centralizados** através da distribuição de processamento e estado entre múltiplas instâncias da aplicação. **É altamente eficiente para sistemas com grande volume de acessos e necessidade de escalabilidade extrema**, **porém aumenta significativamente a complexidade de sincronização, observabilidade e gerenciamento da infraestrutura distribuída.**

---

## 5. SBA vs EDA vs Microsserviços

Três abordagens **complementares**, que resolvem **problemas diferentes** e **podem coexistir na mesma solução**:

| Aspecto | SBA (Space-Based) | EDA (Event-Driven) | Microsserviços |
|---|---|---|---|
| **Foco** | Escalabilidade extrema, baixa latência e alta disponibilidade | Comunicação assíncrona, desacoplamento e reatividade | Modularização funcional, independência e entrega contínua |
| **Ideia central** | Distribuir processamento, estado e memória para eliminar gargalos centrais | Componentes publicam eventos; outros componentes consomem esses eventos | Dividir o sistema em pequenos serviços independentes e coesos por domínio |
| **O que distribui** | Estado, memória e processamento | Eventos e mensagens | Responsabilidades de negócio (serviços) |
| **Comunicação** | Entre nós (middleware distribuído) | Assíncrona (eventos via broker) | Síncrona (APIs) e/ou assíncrona (eventos) |
| **Consistência** | Geralmente eventual (consistência relaxada) | Eventual (por natureza assíncrona) | Depende da implementação de cada serviço |
| **Problema que resolve** | "Meu sistema não escala porque tudo depende de um componente central" | "Quero desacoplar componentes e reagir a acontecimentos do sistema" | "Meu sistema monolítico está difícil de evoluir e manter" |
| **Palavra-chave** | Distribuição de estado e processamento | Comunicação orientada a eventos | Separação funcional do domínio |
| **Exemplos de uso** | Sistemas financeiros de alta frequência, e-commerce massivo, jogos online, IoT, redes sociais, streaming | Integrações, pipelines de dados, notificações, monitoramento, IoT, processos de negócio | Aplicações complexas, domínios grandes, produtos com evolução contínua |

> **Eles podem coexistir!** Não são alternativas excludentes. É comum ver arquiteturas como: Microsserviços + EDA, SBA + EDA, Microsserviços + SBA, SBA + Microsserviços + EDA.

> **Resumo final**: Microsserviços organiza **responsabilidades de negócio** (serviços), EDA organiza a **comunicação entre componentes** e SBA organiza a **distribuição de processamento e estado** — cada abordagem resolve um problema diferente, e **cada uma pode estar presente na mesma solução, em camadas/partes diferentes do sistema.**

---

## 6. Conclusão Geral da Aula

Os três estilos vistos resolvem necessidades distintas e raramente competem entre si:

- **Pipes-and-Filters** organiza **como os dados fluem e são transformados** dentro de um processamento (modularidade e composição), mas pode aumentar latência/overhead se houver muitas etapas.
- **Publish/Subscribe** organiza **como os componentes se comunicam de forma desacoplada e assíncrona** (não é um estilo "completo" por si só, mas um padrão de comunicação típico de EDA), ao custo de dificultar rastreamento e debug.
- **Space-Based** organiza **como o estado e o processamento de todo o sistema são distribuídos** para escalar sem depender de um banco central, ao custo de muito mais complexidade operacional e consistência relaxada (AP).

---

## 7. O que mais cai em prova

- **Identificar os componentes de cada estilo**:
  - Pipes-and-Filters: **filtro, pipe (tubulação), pump (bomba), sink (destino)**.
  - Pub/Sub: **publisher, channel/topic (broker), subscriber**.
  - Space-Based: **Processing Unit (PU), Data Grid, Messaging Grid, Processing Grid, Deployment Manager, Data Replication Engine**.
- **Pipes-and-Filters nem sempre é o estilo predominante de um sistema** — costuma aparecer em partes específicas (processamento HTTP/middlewares, integração, streaming, pipelines de build/CI-CD).
- **Pub/Sub não é um estilo arquitetural clássico**, e sim um **padrão de comunicação** típico de **EDA (Event Driven Architecture)**.
- **Trade-offs principais**:
  - Pipes-and-Filters: **manutenibilidade vs performance**, **paralelismo vs consistência de estado**, **escalabilidade vs complexidade de coordenação**.
  - Space-Based: **disponibilidade vs consistência (modelo AP do CAP)**, **performance vs consumo de memória**, **latência vs complexidade de sincronização**.
- **Origem do Space-Based**: criado/desenvolvido na **Microsoft em 1997-98**, internamente chamado de **Youkon (YDC)**, com o **MSN Live Search (1999)** como um dos primeiros grandes usos.
- **Quando usar/evitar SBA**: usar em cenários de escalabilidade extrema (marketplaces, sistemas financeiros, games online, streaming, IoT, picos extremos, SaaS massivo); evitar em sistemas pequenos, baixa concorrência, consistência forte obrigatória, equipes pequenas ou orçamento limitado.
- **Diferença entre SBA, EDA e Microsserviços**: SBA distribui **estado e processamento**; EDA organiza a **comunicação por eventos**; Microsserviços separa **responsabilidades de negócio**. Os três **podem coexistir** na mesma arquitetura.
