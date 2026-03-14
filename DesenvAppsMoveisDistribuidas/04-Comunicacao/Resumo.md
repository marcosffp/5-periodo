# Resumo — Comunicação em Sistemas Distribuídos: RPC, RMI, REST e gRPC

-- Link do mateiral: <https://cristianoneto.online/puc/damd/aula05>
---

## 1. O Problema Central

Em sistemas centralizados, funções se comunicam internamente pela memória. Em sistemas distribuídos, essa chamada precisa cruzar a rede, o que introduz falhas imprevisíveis, atrasos e a necessidade de serialização de dados (converter objetos em bytes para transmissão). Os paradigmas de comunicação existem para abstrair essa complexidade.

---

## 2. RPC — Remote Procedure Call (1984)

RPC permite que um processo chame uma função em outro computador como se fosse uma função local. Internamente, um componente chamado **stub** encapsula a comunicação: serializa os parâmetros (marshalling), envia pela rede, recebe a resposta e a devolve ao chamador.

O ponto crítico do RPC está nas **semânticas de falha**, que definem quantas vezes uma operação pode ser executada em caso de erro:

- **Maybe:** pode executar 0 ou 1 vez. Sem garantias.
- **At-Least-Once:** executa pelo menos uma vez. Seguro apenas para operações idempotentes (que podem repetir sem efeito colateral, como uma consulta de saldo).
- **At-Most-Once:** executa no máximo uma vez. Indicado para operações não-idempotentes.
- **Exactly-Once:** executa exatamente uma vez. Usado em transações críticas como o Pix.

Para garantir o Exactly-Once em retries, usa-se uma **chave de idempotência**: um identificador único por operação que impede duplicações no servidor.

---

## 3. RMI — Remote Method Invocation (1991)

Evolução do RPC para o paradigma orientado a objetos. Em vez de chamar funções, o cliente chama métodos em um objeto que vive em outro servidor. A diferença é que esse objeto pode manter estado entre chamadas. O risco: se o servidor reiniciar, o estado é perdido.

---

## 4. REST — Representational State Transfer (2000)

REST é um estilo arquitetural, não um protocolo. Define que recursos (dados) são acessados via URLs e manipulados com verbos HTTP: GET (leitura), POST (criação), PUT (atualização), DELETE (remoção). A característica mais importante é ser **stateless**: cada requisição carrega todas as informações necessárias, sem sessão no servidor. Isso facilita escalabilidade.

GET, PUT e DELETE são **idempotentes** (repetir não muda o resultado). POST não é.

---

## 5. gRPC (2016)

Desenvolvido pelo Google, o gRPC usa **Protocol Buffers (Protobuf)** para serialização binária (mais compacto e rápido que JSON) e **HTTP/2** como transporte (permite múltiplas requisições simultâneas na mesma conexão). O contrato da API é definido em um arquivo `.proto`, independente de linguagem.

Suporta quatro modos de comunicação: chamada simples (unary), servidor enviando múltiplas respostas (server streaming), cliente enviando múltiplos dados (client streaming), e comunicação bidirecional simultânea (usado em chats e telemetria em tempo real).

---

## 6. Comparativo: REST vs gRPC

| Critério | REST | gRPC |
|---|---|---|
| Performance | Média (JSON/HTTP 1.1) | Alta (Protobuf/HTTP/2) |
| Interoperabilidade | Muito alta (nativo na web) | Alta (requer stubs gerados) |
| Debug | Fácil (texto legível) | Médio (formato binário) |
| Uso ideal | APIs públicas, B2C | Microserviços internos |

**Regra prática:** API pública para qualquer cliente? Use REST. Comunicação interna entre microsserviços com alta demanda? Use gRPC. Dados em tempo real contínuo? gRPC com streaming bidirecional.

---

## 7. O que mais cai em prova

Saber explicar as **4 semânticas de falha do RPC** e seus casos de uso é essencial. Entender que **REST é stateless** e que **POST não é idempotente** costuma aparecer. Conhecer a diferença entre REST e gRPC em termos de **serialização, protocolo e cenário de uso** é frequente. Por fim, saber o que é uma **chave de idempotência** e para que serve resolve questões sobre transações críticas.