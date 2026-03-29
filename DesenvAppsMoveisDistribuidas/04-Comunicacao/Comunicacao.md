# Comunicação entre Processos em SD — RPC · RMI · REST · gRPC
**PUC Minas — ICEI — Aplicações Móveis e Distribuídas — Aula 05**
**Professor:** Cristiano de Macedo Neto

*Paradigmas, Trade-offs e Código na Prática · Python*

*"The dream is to make the distributed system look like a non-distributed one."*
— Andrew Tanenbaum · Distributed Systems: Principles and Paradigms

**🎯 Objetivos:**
- Compreender a evolução histórica dos paradigmas de comunicação em SD
- Implementar clientes e servidores em cada paradigma usando Python
- Analisar os trade-offs de latência, acoplamento, tipagem e escalabilidade
- Escolher o paradigma correto para cada cenário arquitetural

---

## O Problema Fundamental: Como Processos se Comunicam?

Em um sistema centralizado, funções se chamam via **pilha de execução** — rápido, síncrono, sem falhas de rede. Em SD, a chamada cruza uma rede: tudo pode dar errado, a qualquer momento, de formas imprevisíveis.

### Linha do Tempo dos Paradigmas

- **1984 — RPC (Remote Procedure Call):** Birrell & Nelson (SRC/DEC). Primeira abstração sistemática: chamar função remota como se fosse local. Base de toda evolução posterior.
- **1991 — RMI (Remote Method Invocation):** Java RMI (Sun Microsystems). RPC orientado a objetos: invocar métodos de objetos remotos com transparência de localização nativa.
- **2000 — REST (Representational State Transfer):** Roy Fielding (tese de doutorado, UC Irvine). Arquitetura baseada em recursos HTTP, stateless, escalável. Dominante na era das APIs Web.
- **2016 — gRPC (Google Remote Procedure Call):** Google (open source). RPC moderno sobre HTTP/2 com Protobuf: tipagem forte, streaming bidirecional, alta performance. Padrão em microserviços.

---

## RPC — Remote Procedure Call: Conceito e Anatomia

RPC permite que um processo **chame uma função em outro processo** (possivelmente em outra máquina) como se fosse uma chamada local. A mágica está no **stub** — código gerado automaticamente que encapsula toda a comunicação.

Fluxo: **Cliente chama função()** → **Client Stub (marshalling/serializa args)** → **Rede TCP/UDP (bytes)** → **Server Stub (unmarshalling/desserializa)** → **Servidor executa função()**

### O que o XML-RPC faz automaticamente
- **Marshalling:** converte tipos Python → XML → bytes → rede → bytes → XML → tipos Python
- **Binding:** o proxy descobre os métodos disponíveis no servidor
- **Exceções:** erros do servidor são re-lançados no cliente

---

## RPC — Semânticas de Invocação e o Problema das Falhas

Em chamadas locais, uma função é executada **exatamente uma vez**. Em RPC, a rede pode falhar *antes*, *durante* ou *depois* da execução — criando ambiguidade.

| Semântica | Garantia | Uso Ideal | Risco |
|---|---|---|---|
| Maybe | 0 ou 1 execuções | Operações sem efeito (ex: leituras não críticas) | Pode não executar |
| At-Least-Once | ≥ 1 execuções | Operações **idempotentes** (ex: GET HTTP, SET chave) | Pode duplicar |
| **At-Most-Once** | 0 ou 1 execuções | Operações **não-idempotentes** (ex: débito bancário) | Pode não executar |
| **Exactly-Once** | Exatamente 1 execução | Transações críticas (ex: Pix) | Ideal — mas custoso |

### 🔑 Idempotência: a propriedade mais importante em SD
Uma operação é **idempotente** se executá-la N vezes produz o mesmo resultado que executá-la 1 vez. `SET saldo = 500` é idempotente. `saldo -= 100` não é. Projete operações remotas para serem idempotentes sempre que possível.

---

## RMI — Remote Method Invocation: Objetos Distribuídos

RMI é RPC orientado a objetos: em vez de chamar funções, você invoca **métodos de objetos que vivem em outra JVM**. Em Python, o padrão equivalente usa **Pyro5** (Python Remote Objects).

### RPC vs. RMI: A Distinção Essencial
- **RPC:** chama *funções* — paradigma procedural, sem estado entre chamadas.
- **RMI:** chama *métodos de objetos* — paradigma OO, o objeto remoto *mantém estado* entre chamadas. O cliente obtém uma *referência de objeto remoto* e a usa como se o objeto estivesse local.

### ⚠️ O Calcanhar de Aquiles do RMI: Estado no Servidor
Se o servidor reinicia, o estado do objeto remoto é perdido. Isso viola a *transparência de falha*. Por isso, sistemas modernos preferem objetos **stateless** (REST/gRPC) com estado externalizado em banco de dados ou Redis.

---

## REST — Representational State Transfer: Arquitetura

REST não é um protocolo — é um **estilo arquitetural** com 6 restrições definidas por Roy Fielding (2000):

1. **Client-Server:** Separação de responsabilidades. Interface do usuário desacoplada do armazenamento de dados.
2. **Stateless:** Cada requisição contém **toda** a informação necessária. Servidor não guarda sessão de cliente.
3. **Cacheable:** Respostas devem indicar se podem ser cacheadas. Melhora performance e escalabilidade.
4. **Uniform Interface:** **A restrição mais importante.** Recursos identificados por URI. Manipulação via verbos HTTP padronizados.
5. **Layered System:** Cliente não sabe se fala diretamente com o servidor ou com um intermediário (load balancer, CDN).
6. **Code on Demand (opcional):** Servidor pode enviar código executável ao cliente (ex: JavaScript). Raramente usado em APIs.

### Mapeamento CRUD → Verbos HTTP

| Operação | Verbo HTTP | URI Exemplo | Idempotente? | Body? |
|---|---|---|---|---|
| Listar | GET | `GET /pedidos` | ✅ Sim | Não |
| Obter um | GET | `GET /pedidos/42` | ✅ Sim | Não |
| Criar | POST | `POST /pedidos` | ❌ Não | JSON |
| Substituir | PUT | `PUT /pedidos/42` | ✅ Sim | JSON |
| Atualizar | PATCH | `PATCH /pedidos/42` | ⚠️ Depende | JSON parcial |
| Remover | DELETE | `DELETE /pedidos/42` | ✅ Sim | Não |

### REST na prática: Códigos HTTP importam
**200 OK** · **201 Created** · **204 No Content** · **400 Bad Request** · **401 Unauthorized** · **404 Not Found** · **409 Conflict** · **429 Too Many Requests** · **500 Internal Server Error**

---

## gRPC — Google Remote Procedure Call: Protobuf e HTTP/2

gRPC é RPC moderno: usa **Protocol Buffers** (Protobuf) para serialização e **HTTP/2** como transporte. O contrato é definido em um arquivo `.proto` — linguagem agnóstica, fortemente tipada.

### 4 Padrões de Comunicação (a grande vantagem do gRPC)

- **Unary RPC:** Req → Resp. Idêntico ao REST. *Ex: autenticação*
- **Server Streaming:** Req → Resp…Resp…Resp. Servidor envia múltiplos. *Ex: feed de eventos*
- **Client Streaming:** Req…Req…Req → Resp. Cliente envia múltiplos. *Ex: upload chunked*
- **Bidirecional:** Req…↔…Resp. Full duplex simultâneo. *Ex: chat, telemetria*

Todos sobre uma única conexão HTTP/2 multiplexada. REST precisaria de WebSockets ou long-polling para o mesmo efeito.

---

## Checkpoint: Escolha do Paradigma Certo

**Q1.** API pública de CEPs, consumida por múltiplas linguagens (JavaScript, Java, Swift, Kotlin). Qual paradigma?
✅ **C — REST** — interface uniforme via HTTP, agnóstica à linguagem, amplamente suportada.

**Q2.** Sistema de telemetria com leituras de sensores a cada 10ms, latência mínima, volume altíssimo. Qual paradigma?
✅ **B — gRPC com streaming bidirecional** — HTTP/2 multiplexado, Protobuf binário e baixa latência.

**Q3.** Serviço de débito bancário via RPC. Conexão cai antes de receber resposta. Como garantir que não debite duas vezes?
✅ **D — Usar chave de idempotência única por operação** — retries com a mesma chave são seguros.

---

## Trade-offs: Quando Usar Cada Paradigma

| Critério | RPC | RMI | REST | gRPC |
|---|---|---|---|---|
| Performance / Latência | Média | Média | Média (HTTP/1.1) | Alta (HTTP/2 + Protobuf) |
| Interoperabilidade | Média | Baixa (JVM) | Muito alta (HTTP universal) | Alta (stubs em 12+ langs) |
| Tipagem / Schema | Fraca/média | Média | Fraca (JSON sem schema) | Forte (Protobuf obrigatório) |
| Streaming | Não suportado | Não suportado | Limitado (SSE/WebSocket) | 4 modos nativos |
| Acoplamento | Alto | Muito alto (JVM) | Baixo (hipermídia) | Médio (.proto compartilhado) |
| Estado entre chamadas | Sem estado | Com estado | Sem estado (restrição REST) | Sem estado (por design) |
| Visibilidade / Debug | Média (XML legível) | Baixa (binário) | Alta (curl, browser, Postman) | Média (grpcurl, Postman gRPC) |
| Cenário ideal | Sistemas legados, integração simples | Ecossistemas homogêneos JVM | APIs públicas, B2C, web | Microserviços internos, IoT, ML |

### 🏛️ Regra de Ouro Arquitetural
- **API pública / B2C / terceiros?** → REST
- **Microserviços internos com alta performance?** → gRPC
- **Ecossistema JVM homogêneo?** → RMI
- **Streaming em tempo real bidirecional?** → gRPC (ou WebSocket)
- **Dúvida?** → Comece com REST e migre quando performance se tornar gargalo

---

## Síntese: Comunicação em SD

| Paradigma | Abstração Central | Serialização | Transporte | Tecnologias Modernas |
|---|---|---|---|---|
| RPC | Chamada de função remota | XML, JSON, Thrift | TCP | Apache Thrift, XML-RPC |
| RMI | Objeto remoto com estado | Java Serialization, Pickle | JRMP / TCP | Pyro5, Java RMI, CORBA (legado) |
| REST | Recurso identificado por URI | JSON, XML, HAL | HTTP/1.1, HTTP/2 | FastAPI, Django REST, Spring Boot |
| gRPC | Contrato Protobuf + streaming | Protocol Buffers (binário) | HTTP/2 (TLS) | gRPC-Python, gRPC-Go, gRPC-Java |

### Conceitos-Chave desta Aula
- **Stub/Proxy:** código gerado que encapsula comunicação
- **Marshalling:** serialização de tipos para transmissão
- **Semântica de invocação:** Maybe, At-Least-Once, At-Most-Once, Exactly-Once
- **Idempotência:** segurança no retry de operações
- **Protobuf:** schema binário tipado e versionável
- **HTTP/2:** multiplexação, streaming, header compression

### ⚠️ Armadilhas Comuns
- Tratar chamadas RPC como se fossem locais — latência e falha são diferentes
- Não implementar timeout — cliente bloqueia indefinidamente
- Fazer retry sem idempotência — duplicação de operações
- Usar REST para microserviços internos críticos — overhead desnecessário
- Usar gRPC para API pública — difícil de consumir em browsers nativamente
- Ignorar versionamento de schema — quebra compatibilidade silenciosamente

**Próximo Assunto:** Coordenação e sincronização — Relógios lógicos de Lamport e relógios vetoriais

---

**📚 Bibliografia:**
- BIRRELL, A. D.; NELSON, B. J. *Implementing Remote Procedure Calls.* ACM TOCS, 1984.
- FIELDING, R. T. *Architectural Styles and the Design of Network-based Software Architectures.* UC Irvine, 2000.
- COULOURIS, G. et al. *Distributed Systems: Concepts and Design.* 5. ed. Cap. 5.
- TANENBAUM, A. S.; VAN STEEN, M. *Distributed Systems.* 3. ed. Cap. 4.
- KLEPPMANN, M. *Designing Data-Intensive Applications.* Caps. 4 e 11.
- NEWMAN, S. *Building Microservices.* 2. ed. O'Reilly, 2021.