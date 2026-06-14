# RESUMO — Estilo Arquitetural Cliente-Servidor e Orientado a Serviços (SOA)

## 1. Contexto Geral

A aula trata de **dois estilos arquiteturais** que aparecem em praticamente todo sistema moderno:

- **Cliente-Servidor**: o estilo mais básico de comunicação distribuída — quase tudo que existe hoje (web, mobile, microsserviços) é construído **sobre** ele.
- **SOA (Service-Oriented Architecture)**: um estilo que organiza o sistema como um **conjunto de serviços independentes**, pensado para resolver problemas de **integração** entre sistemas heterogêneos (legados, ERPs, CRMs, bancos de dados, apps mobile etc.).

Na prática, eles **não competem entre si**: SOA é construído *sobre* o modelo cliente-servidor. O cliente-servidor define "quem pede e quem responde"; o SOA define "como organizar e integrar os vários servidores/serviços que respondem".

---

## 2. Arquitetura Cliente-Servidor

### 2.1 Definição

> Segundo Bass, Clements e Kazman: a arquitetura cliente-servidor é um estilo arquitetural no qual o sistema é **estruturado em dois tipos principais de componentes**: **clientes**, que **solicitam** serviços, e **servidores**, que **fornecem** esses serviços por meio de uma rede.

Ideia central: separar **quem consome** (cliente) de **quem provê** (servidor), comunicando-se por rede (ex: internet). Um cliente (navegador, app mobile, desktop) envia uma requisição; o servidor processa e devolve uma resposta.

### 2.2 Por que é a base de "tudo"

- **Todos os sistemas web** usam o modelo cliente-servidor como base de comunicação — mesmo que, **internamente**, combinem outros estilos mais complexos (camadas, MVC, microsserviços, orientado a eventos etc.).
- Mesmo arquiteturas avançadas como **microsserviços** continuam, na essência, sendo várias relações cliente-servidor (um serviço chama o outro como cliente, e é chamado como servidor).

### 2.3 Variações do estilo: 2-tier, 3-tier e N-tier

A diferença entre essas variações é **quantas camadas (tiers) existem entre o cliente e o banco de dados**, e onde fica a lógica de negócio.

| Variação | Como funciona | Onde fica a lógica de negócio | Exemplo típico |
|---|---|---|---|
| **2-tier** | Cliente acessa **diretamente** o banco de dados | Misturada no cliente (ou no banco, via stored procedures) | Aplicações desktop antigas que conectam direto no BD |
| **3-tier** | Cliente → **Servidor de aplicação** → Banco de dados | Concentrada no servidor de aplicação | Aplicações web tradicionais (ex: app Java/Node que acessa o BD) |
| **N-tier** | Cliente → Servidor Web/API → **Serviços de Negócio** → Banco de dados (múltiplas camadas) | Distribuída em várias camadas especializadas | Sistemas corporativos complexos, com múltiplos serviços e camadas de persistência |

**Por que isso importa:** quanto mais camadas (tiers), mais fácil é separar responsabilidades e **escalar** partes específicas do sistema isoladamente — mas também aumenta a complexidade e a quantidade de "saltos" de rede entre as camadas.

### 2.4 Vantagens

- **Centralização** de dados e regras de negócio (tudo fica controlado em um único lugar).
- **Facilidade de manutenção e atualização**: muda-se o servidor, e todos os clientes passam a usar a versão nova automaticamente.
- **Maior controle de segurança** (autenticação, autorização e validação concentradas no servidor).
- **Clientes mais simples** (o cliente só precisa saber "pedir", não implementar a lógica).
- **Consistência de dados facilitada** (uma única fonte de verdade).
- **Facilidade de gerenciamento e monitoramento** (um ponto central para observar).

### 2.5 Desvantagens

- **Ponto único de falha (SPOF)**: se o servidor cair, todos os clientes ficam sem serviço.
- **Possível gargalo de desempenho**: todas as requisições passam pelo mesmo servidor.
- **Dependência da rede**: latência e disponibilidade da rede afetam diretamente a experiência.
- **Escalabilidade limitada** sem mecanismos adicionais (balanceamento de carga, réplicas, cache).
- **Maior carga de processamento no servidor** (ele concentra praticamente todo o trabalho pesado).
- **Necessidade de infraestrutura extra** para garantir alta disponibilidade.

### 2.6 Trade-offs (o "preço" das decisões)

Em arquitetura, **toda decisão troca um atributo de qualidade por outro** — não existe "almoço grátis". No estilo cliente-servidor, dois trade-offs centrais são:

1. **+ Simplicidade ↔ − Disponibilidade**
   Ter um único servidor centralizando tudo é **simples** de implementar e entender, mas esse mesmo servidor pode se tornar o **ponto único de falha** da arquitetura — se ele cair, o sistema todo para.

2. **+ Centralização ↔ − Escalabilidade**
   Centralizar a lógica e os dados no servidor **simplifica o controle e a manutenção** (uma única fonte de verdade, fácil de auditar), porém **pode virar gargalo** quando o volume de requisições cresce. Para escalar, é necessário adicionar **réplicas, balanceamento de carga**, etc. — ou seja, abandonar parte da simplicidade original em troca de escalabilidade.

> **Resumo do trade-off:** quanto mais você centraliza (simplicidade, controle), menos escalável e menos disponível o sistema tende a ser por padrão — a escalabilidade/disponibilidade precisa ser *adicionada* depois, com custo de complexidade extra.

---

## 3. Arquitetura Orientada a Serviços (SOA)

### 3.1 Definição

> Segundo Bass, Clements e Kazman: a Arquitetura Orientada a Serviços (SOA) é um estilo arquitetural no qual a **funcionalidade do sistema é organizada como um conjunto de serviços interoperáveis**, **fracamente acoplados** e **acessíveis por meio de interfaces bem definidas**.

Em outras palavras: em vez de um sistema monolítico ou de poucos servidores gigantes, o sistema é dividido em **vários serviços autônomos**, cada um responsável por uma parte do negócio (ex: pagamento, estoque, entrega), que se comunicam por meio de **contratos/interfaces padronizados** (ex: APIs, mensagens).

SOA se consolidou como uma das principais abordagens para promover **interoperabilidade entre sistemas** — especialmente útil quando uma organização tem vários sistemas diferentes (legados, ERPs, CRMs, bancos de dados, apps web e mobile) que precisam **conversar entre si**.

### 3.2 Princípios básicos da SOA

Esses princípios definem "o que faz um bom serviço" dentro de uma arquitetura SOA:

- **Baixo acoplamento**: serviços dependem o mínimo possível uns dos outros — mudanças internas em um serviço não devem quebrar os outros.
- **Autonomia do serviço**: cada serviço controla sua própria lógica e seus próprios dados/recursos.
- **Abstração do serviço**: o serviço expõe **o que faz**, mas esconde **como faz** (detalhes de implementação ficam ocultos).
- **Reusabilidade**: um mesmo serviço pode ser reutilizado por diferentes aplicações/processos.
- **Composição de serviços**: serviços menores podem ser combinados para formar processos de negócio maiores.
- **Sem estado (statelessness)**: idealmente, os serviços não guardam estado de uma chamada para outra, facilitando escalabilidade.
- **Visibilidade do serviço**: o serviço pode ser **descoberto** e entendido por quem precisa consumi-lo (ex: catálogos/documentação de API).
- **Padronização do contrato de serviço**: a interface de comunicação (contrato) segue padrões bem definidos, conhecidos por todos que consomem o serviço.
- **Heterogeneidade**: serviços escritos em linguagens/tecnologias diferentes podem coexistir e se comunicar (ex: um serviço em Java falando com outro em Ruby).

### 3.3 O ESB (Enterprise Service Bus)

#### O que é

O **ESB** era o **coração da integração** no SOA tradicional: um **middleware** (intermediário) que fica **entre** os sistemas/serviços, permitindo que aplicações muito diferentes (legados em C, ERPs em Java, CRMs em Ruby, bancos de dados, apps web e mobile) se comuniquem **sem precisar conhecer os detalhes umas das outras**.

#### Funções do ESB

- **Roteamento**: decide para onde uma mensagem deve ir.
- **Transformação**: converte formatos de mensagem (ex: XML ↔ SOAP ↔ JSON).
- **Mediação de protocolos**: traduz entre diferentes protocolos de comunicação (SOAP, RMI, HTTP, JMS/MQ, HTTPS etc.).
- **Segurança**: aplica regras de autenticação/autorização nas mensagens que trafegam por ele.
- **Orquestração leve** (em alguns casos): pode coordenar fluxos simples entre serviços.

#### O que o ESB **não** é

- **Não é uma API** que expõe endpoints diretamente para o cliente final.
- **Não é um back-end de domínio** (não é "o serviço de Pedido" ou "o serviço de Pagamento").

#### ESB x Web Service/API — a diferença de papel

| Característica | ESB | Web Service / API |
|---|---|---|
| **Papel** | Integração | Expor lógica de negócio |
| **Quem consome** | Outros serviços/sistemas | Clientes (web/mobile) |
| **Posição** | Central (no meio da comunicação) | Borda (no domínio) |
| **Lógica de negócio** | Deve **evitar** ter | Deve **aplicar** |

> O ESB **não é um back-end de negócio**; é uma **infraestrutura de integração** que intermedia a comunicação entre serviços.

#### Analogia: o ESB é uma secretária executiva

Pense no ESB como uma **secretária executiva muito técnica**, que fala vários "idiomas" (protocolos) e consegue colocar pessoas em contato — mas **não toma decisões estratégicas** sobre o andamento do trabalho (não decide a ordem das etapas de um processo de negócio). Ela só **viabiliza a comunicação de forma inteligente**.

Isso leva à pergunta natural: **se a secretária (ESB) não decide o fluxo de negócio, quem decide?** Resposta: o **orquestrador** (ver seção 3.5).

#### O problema do "God ESB"

Quando o ESB acumula **responsabilidades demais** (roteamento + transformação + segurança + regras de negócio + orquestração pesada...), ele se transforma no chamado **"God ESB"**, trazendo problemas como:

- **Centralização excessiva**
- **Gargalo de performance** (tudo passa por ele)
- **Acoplamento forte** (todo mundo depende dele para se comunicar)
- **Dificuldade de evolução** (mudar o ESB afeta todo o ecossistema)

**Resultado prático:** o mercado começou a **quebrar essas responsabilidades** em peças menores e mais especializadas (ver próxima seção).

### 3.4 Em que o ESB se transformou na prática

Hoje, as responsabilidades que o ESB clássico acumulava foram **distribuídas** entre duas categorias de ferramentas:

1. **Comunicação entre serviços** → ferramentas de **mensageria/eventos** (substituem o "barramento" do ESB):
   - **Filas** (ex: Amazon SQS, RabbitMQ)
   - **Tópicos/Eventos** (ex: Apache Kafka, Azure Service Bus)

2. **O que o ESB fazia na "borda" virou papel do API Gateway**:
   - Authentication / Authorization
   - Routing
   - Rate limiting
   - Caching
   - Request Transformation

> **Conclusão da evolução:** a tecnologia "provou" que era melhor separar essas responsabilidades em peças especializadas (API Gateway para a borda, Kafka/RabbitMQ/SQS para comunicação entre serviços) do que manter um único ESB monolítico fazendo tudo.

### 3.5 Orquestração vs Coreografia

Esses dois conceitos respondem à pergunta: **"quem coordena o fluxo de um processo de negócio que envolve vários serviços?"**

#### Orquestração

> É um **estilo de coordenação/interação** entre serviços em que **um componente central (orquestrador)** controla o fluxo do processo. Pode ser aplicado em diferentes arquiteturas (SOA, microsserviços, e até em cenários orientados a eventos).

**Analogia:** o orquestrador é o **maestro** que conduz a orquestra — ele define a ordem em que cada instrumento (serviço) toca, aplica as regras da "partitura" (regras de negócio) e consolida o resultado final.

**Responsabilidades do orquestrador:**
- Define a **ordem** das chamadas.
- Aplica as **regras de negócio**.
- **Consolida** os resultados.

#### Coreografia

> É um **estilo de interação entre serviços** em que **não há um controlador central** — cada serviço **reage a eventos** de forma autônoma. Também pode ser aplicado em SOA, microsserviços e arquiteturas orientadas a eventos.

**Analogia:** como uma **coreografia de dança** — cada bailarino (serviço) sabe seu papel e reage ao que acontece ao redor, sem um maestro central dando ordem a cada passo.

#### Comparação direta

| Aspecto | Orquestração | Coreografia |
|---|---|---|
| **Controle** | Centralizado (orquestrador) | Distribuído |
| **Fluxo** | Explícito (definido em um lugar) | Implícito (emerge das reações a eventos) |
| **Acoplamento** | Médio | Baixo |
| **Complexidade** | Mais simples de **entender** | Mais difícil de **rastrear** |
| **Privilegia** | **Controle** | **Autonomia** |

#### Quando usar cada uma (exemplo real)

- **Coreografia**: boa para **eventos simples** (ex: "pedido criado", "pagamento aprovado") — cada serviço reage de forma independente.
- **Orquestração**: melhor para **fluxos críticos** que precisam de controle rígido (ex: cancelamento, rollback, padrão **SAGA** — ver seção 3.6).

#### Fluxo de chamadas: Orquestrador + ESB trabalhando juntos

Num cenário real, **orquestrador** e **ESB/mensageria** atuam em **camadas diferentes e complementares**:

- O **Orquestrador** (processo de negócio) **controla o fluxo**: por exemplo, define a ordem `pagamento → estoque → entrega`, aplica regras de negócio e consolida os resultados.
- O **ESB** (ou seu equivalente moderno de mensageria) **viabiliza a comunicação**: recebe, transforma (se necessário), roteia e encaminha as mensagens entre o orquestrador e cada serviço (PaymentService, InventoryService, ShippingService).
- Os **serviços** apenas executam suas regras específicas de domínio e retornam o resultado.

**Benefícios dessa separação:**
- Separação clara de responsabilidades.
- O orquestrador foca **só** no fluxo de negócio.
- O ESB cuida **só** da comunicação/integração.
- Cada serviço foca **só** na execução da sua regra de negócio.

### 3.6 SAGA Pattern (consistência em sistemas distribuídos)

#### O problema

Em sistemas distribuídos (vários serviços, cada um com seu próprio banco de dados), **não é possível usar controle de transação ACID tradicional** (Atomicidade, Consistência, Isolamento, Durabilidade) — porque **não existe um banco de dados único** que englobe todas as operações.

**Pergunta central:** como garantir consistência entre transações realizadas por vários serviços diferentes, se não dá para fazer um "BEGIN/COMMIT/ROLLBACK" tradicional que abranja todos eles?

#### A solução: SAGA

> O **SAGA Pattern** é uma **sequência de transações locais**, onde **cada transação tem uma ação compensatória** para rollback em caso de falha.

Funcionamento:

1. O processo é dividido em uma sequência de **transações locais** (Serviço 1 → Serviço 2 → Serviço 3 → ...), cada uma executada e confirmada **dentro do seu próprio serviço/banco**.
2. Se **todas as etapas tiverem sucesso** → processo concluído normalmente (*Process Completed*).
3. Se **alguma etapa falhar** → são disparadas as **ações compensatórias** das etapas já concluídas, "desfazendo" o que já havia sido feito (*Rollback Triggered*) — não é um rollback de banco de dados, é uma **operação inversa de negócio** (ex: se "reservar estoque" falhar depois do pagamento, dispara-se "cancelar pagamento" como compensação).

**Por isso o SAGA aparece como exemplo de fluxo crítico que pede orquestração**: para coordenar corretamente a ordem das transações e das compensações, geralmente é mais simples ter um componente central (orquestrador) controlando esse fluxo do que depender só de coreografia.

### 3.7 Evolução do SOA: rumo a eventos

A evolução natural do SOA (especialmente após a "quebra" do ESB) caminha para arquiteturas **orientadas a eventos**:

- **Event Producers** (produtores de eventos) publicam eventos em **Event Channels** (canais de eventos).
- **Event Consumers** (consumidores de eventos) recebem e reagem a esses eventos.

Essa estrutura é a base tecnológica que sustenta a **coreografia** (seção 3.5) e está por trás de ferramentas como Kafka, RabbitMQ, Azure Service Bus e Amazon SQS, citadas como os "novos ESBs" na prática.

### 3.8 Vantagens da SOA

- **Reuso de serviços** (o mesmo serviço atende vários sistemas/processos).
- **Baixo acoplamento** entre as partes.
- **Interoperabilidade** entre sistemas diferentes (linguagens, plataformas).
- **Centralização de capacidades** (regras de negócio reutilizáveis ficam concentradas em serviços específicos).
- **Alinhamento com o negócio** (cada serviço tende a representar uma capacidade de negócio real).

### 3.9 Desvantagens da SOA

- **Complexidade arquitetural** maior do que um sistema monolítico simples.
- **Dependência de infraestrutura** (ex: ESB) — que pode virar gargalo se evoluir para um "God ESB".
- **Overhead de comunicação** (mais chamadas de rede, serialização/desserialização etc.).
- **Governança pesada**: é preciso cuidar de **versionamento**, **contratos** e **políticas** entre todos os serviços.
- **Dificuldade de testes** (testar um fluxo de negócio pode envolver vários serviços e integrações).

### 3.10 Trade-offs da SOA

1. **Interoperabilidade ↔ Performance**
   Ganhar interoperabilidade (serviços heterogêneos conversando via contratos padronizados) tem um custo de **performance** — cada chamada entre serviços envolve rede, serialização, possivelmente um intermediário (ESB/gateway).

2. **+ Centralização ↔ + Gargalo (ponto único de falha)**
   Centralizar capacidades em serviços específicos (ex: um serviço de autenticação usado por todos) facilita reuso e governança, mas esse serviço centralizado pode se tornar **gargalo** ou **ponto único de falha** se não for projetado para alta disponibilidade.

### 3.11 Exemplos de utilização da SOA

| Domínio | Como o SOA é usado | Característica marcante |
|---|---|---|
| **Sistemas bancários** | Integra conta, cartão, empréstimos e pagamentos | Alto reuso e interoperabilidade; forte necessidade de governança e segurança |
| **Sistemas hospitalares** | Integra prontuário, laboratório e faturamento | Evita duplicidade de dados; integração complexa com sistemas legados |
| **Sistemas governamentais** (ex: GOV.BR) | Integra autenticação, assinatura digital e dados públicos | Alta interoperabilidade entre órgãos; alta complexidade e dependência de múltiplos sistemas |
| **ERP corporativo** | Integra módulos como financeiro, RH e estoque | Reuso de regras de negócio; pode gerar acoplamento indireto entre áreas |
| **Logística e transporte** | Integra pedidos, rastreamento e transportadoras | Visão unificada do processo; dependência de integrações externas |

> **Regra geral:** SOA é mais indicado em **ambientes grandes, heterogêneos** e com **forte necessidade de integração** — exatamente os cenários acima.

---

## 4. Cliente-Servidor vs SOA: comparação direta

| | **Cliente x Servidor** | **Orientado a Serviços (SOA)** |
|---|---|---|
| **Modelo** | Tradicional, com centralização da lógica e dos dados no servidor | Sistema composto por serviços independentes que se comunicam para entregar valor |
| **Comunicação** | Requisição / resposta direta entre cliente e servidor | Via serviços (APIs / mensagens), normalmente passando por um ESB/barramento |
| **Lógica de negócio** | Centralizada | Distribuída entre serviços independentes e reutilizáveis |
| **Facilidade inicial** | Mais simples de desenvolver inicialmente | Mais complexo de projetar e governar |
| **Escalabilidade** | Limitada (servidor pode ser gargalo) | Maior flexibilidade e escalabilidade (cada serviço escala de forma independente) |
| **Integração entre sistemas/organizações** | Não é o foco | É o ponto forte do SOA |

**Em resumo:** Cliente-Servidor centraliza a lógica em um (ou poucos) servidor(es); SOA distribui essa lógica em serviços independentes que se comunicam por interfaces padronizadas. SOA é mais indicado em ambientes complexos, com múltiplos sistemas legados e forte necessidade de integração (bancos, hospitais, governo).

---

## 5. Conclusão

- **Cliente-Servidor** é o estilo fundamental de comunicação distribuída: cliente pede, servidor responde. É **simples** e está na base de quase tudo, mas centraliza riscos (SPOF, gargalo) que precisam ser mitigados com réplicas, balanceamento etc.
- **SOA** organiza o sistema como um conjunto de **serviços interoperáveis e fracamente acoplados**, promovendo **reuso, padronização e alinhamento com o negócio**, para **resolver problemas de integração em larga escala** — ao custo de maior complexidade arquitetural, necessidade de governança rigorosa e possíveis impactos de desempenho.
- O **ESB**, que era o coração do SOA tradicional, sofreu com o problema do **"God ESB"** quando acumulava responsabilidades demais; na prática atual, suas funções foram divididas entre **API Gateways** (borda) e **ferramentas de mensageria/eventos** (Kafka, RabbitMQ, SQS, Azure Service Bus).
- A coordenação entre serviços pode ser feita por **orquestração** (controle centralizado, mais fácil de entender) ou **coreografia** (controle distribuído via eventos, mais autônomo porém mais difícil de rastrear) — e o **SAGA Pattern** usa transações locais + ações compensatórias para garantir consistência sem depender de ACID tradicional.
- Os princípios do SOA continuam **influenciando arquiteturas modernas**, como microsserviços e arquiteturas orientadas a eventos.

---

## 6. O que mais cai em prova

- Definir **Cliente-Servidor** e **SOA** com as palavras-chave certas (segundo Bass, Clements e Kazman): "clientes solicitam, servidores fornecem" / "serviços interoperáveis, fracamente acoplados, interfaces bem definidas".
- Diferenciar as **variações em camadas**: 2-tier (cliente → BD direto), 3-tier (cliente → servidor de aplicação → BD), N-tier (múltiplas camadas de serviços).
- Saber os **trade-offs** de cada estilo: cliente-servidor (simplicidade x disponibilidade; centralização x escalabilidade) e SOA (interoperabilidade x performance; centralização x gargalo/ponto único de falha).
- Entender o papel do **ESB**: o que ele faz (roteamento, transformação, mediação de protocolos, segurança), o que **não** é (não é API, não é back-end de domínio), e o problema do **"God ESB"**.
- Saber em que o ESB **se transformou na prática**: API Gateway (borda) + mensageria/eventos como Kafka/RabbitMQ/SQS (comunicação entre serviços).
- Diferenciar **orquestração** (controle centralizado, explícito, mais fácil de entender) de **coreografia** (controle distribuído, implícito, mais difícil de rastrear) — e saber que ambas podem existir em SOA, microsserviços ou arquiteturas orientadas a eventos.
- Compreender o **SAGA Pattern**: por que ACID não funciona em sistemas distribuídos (não há banco único) e como as **ações compensatórias** garantem consistência eventual.
- Saber identificar **exemplos de aplicação do SOA**: sistemas bancários, hospitalares, governamentais, ERPs e logística — todos com forte necessidade de integração entre sistemas heterogêneos.
