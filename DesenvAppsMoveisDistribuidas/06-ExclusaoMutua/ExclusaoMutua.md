# Exclusão Mútua Distribuída e Eleição de Líder
**PUC Minas — DAMD — Aula 07 · 2026/1**

*"A fundamental problem in distributed systems is how to achieve agreement among a set of processes that can only communicate by passing messages."*
— TANENBAUM, A.S.; VAN STEEN, M. *Distributed Systems*, 4. ed., 2023, p. 267

---

## 🎯 Objetivos da Aula
Ao final você saberá: por que mutex local não funciona em SD; como 3 famílias de algoritmos resolvem exclusão mútua distribuída; como o Bully, Ring e Raft elegem um líder; e onde cada algoritmo aparece em sistemas reais.

---

## O Problema da Seção Crítica em SD

Em sistemas centralizados, **mutex e semáforos** funcionam porque há memória compartilhada. Em SD, os processos estão em máquinas diferentes — **não existe memória compartilhada física**.

### ❌ O Cenário Problemático
Dois servidores tentam atualizar o **saldo da conta #1042** ao mesmo tempo:
- Servidor A: lê saldo = R$1.000
- Servidor B: lê saldo = R$1.000
- A: debita R$300 → grava R$700
- B: debita R$200 → grava R$800
- 💥 Resultado: R$800 (deveria ser R$500!)

### ✅ O que queremos garantir
Três propriedades formais de exclusão mútua (Tanenbaum):
- **Exclusão Mútua:** no máximo 1 processo na seção crítica ao mesmo tempo
- **Progresso (sem deadlock):** se ninguém está na CS e alguém quer entrar, ele entra
- **Espera limitada (sem starvation):** ninguém espera para sempre

### 🧩 A Solução: Mensagens
Em SD, toda coordenação passa por **troca de mensagens**. Cada algoritmo usa estratégia diferente: *permissão*, *token* ou *coordenador central*.

**Analogia — A UTI com uma única cama disponível:** Imagine 3 médicos em alas diferentes recebendo pacientes críticos ao mesmo tempo. Só há uma cama livre. Sem coordenação, dois médicos transferem pacientes simultaneamente — desastre. A exclusão mútua distribuída é o protocolo de comunicação entre as alas para decidir quem ocupa a cama.

---

## Três Famílias de Algoritmos

### 1. Baseados em Permissão
Um processo pede *permissão* a outros antes de entrar na CS. Todos os demais devem responder.
**Exemplos:** Lamport (1978), Ricart-Agrawala (1981), Maekawa (1985)

### 2. Baseados em Token
Um *token* circula pela rede. Quem tem o token tem direito exclusivo de entrar na CS.
**Exemplos:** Token Ring, Suzuki-Kasami (1985)

### 3. Coordenador Central
Um *coordenador eleito* (líder) decide quem entra na CS. Mais simples, mas ponto único de falha.
**Exemplos:** Algoritmos de eleição de líder (Bully, Ring, Raft)

| Família | Mensagens por CS | Tolerância a falhas | Escalabilidade | Overhead |
|---|---|---|---|---|
| Permissão | 2(N−1) | Baixa | Moderada | Alto |
| Token | 1 (melhor caso) | Moderada | Alta | Baixo |
| Coordenador | 3 | SPF | Baixa | Baixo |

**Por que estudar os três?** Cada família aparece em sistemas reais. ZooKeeper usa coordenador centralizado com eleição Raft-like. PostgreSQL com Patroni usa Raft. Token Ring foi base do IEEE 802.5.

---

## Ricart-Agrawala: O Algoritmo de Permissão
*(RICART, G.; AGRAWALA, A.K. Communications of the ACM, v.24, n.1, p.9–17, jan. 1981)*

Extensão e otimização do algoritmo de Lamport (1978): **elimina as mensagens de RELEASE**, reduzindo a complexidade de 3(N−1) para **2(N−1)** mensagens por acesso à CS.

### Estado de cada processo
- **RELEASED:** fora da seção crítica
- **WANTED:** solicitando entrada
- **HELD:** dentro da seção crítica

Cada processo mantém um **relógio de Lamport** para ordenar as requisições.

### Dois tipos de mensagem
- **REQUEST \<T, Pi\>:** multicast para todos
- **REPLY:** enviado ao solicitante quando autorizado

### Regra de Decisão ao Receber REQUEST
Processo Pj recebe REQUEST de Pi. Pj responde imediatamente se:
- Pj está em estado **RELEASED** (não quer a CS)
- **OU** Pj está em WANTED e o timestamp de Pi é **menor** que o de Pj (Pi tem prioridade)

Caso contrário, Pj **enfileira a requisição** e responde apenas quando sair da CS.

Pi entra na CS quando recebe REPLY de **todos** os N−1 processos.

**Analogia — A Fila do Cartório com Senha Temporal:** Cada cidadão que chega pega uma senha com horário. Para ser atendido, você precisa que *todos os outros* confirmem: "ok, sua senha é anterior à minha, pode passar". Se alguém já está no balcão, segura sua confirmação até terminar.

### Complexidade
Com N=3 processos: cada entrada na CS custa **2(N−1) = 4 mensagens**. Para N=100 nós: 198 mensagens por acesso. Pior caso **O(N)**.

---

## Token Ring: O Token que Circula

Os processos são organizados em um **anel lógico**. Um único *token* circula pelo anel. Somente o processo com o token pode entrar na CS.

### Funcionamento
- Ao receber o token: **se quer entrar** na CS → entra, executa, sai e passa o token
- Ao receber o token: **se não quer** → passa para o próximo imediatamente
- Token circula indefinidamente, independentemente de requisições

### ⚠️ Problema: token perdido
Se um processo com o token falha, o token se perde. Detectar isso requer **timeouts** e regenerar o token é complexo.

### Complexidade
- **Melhor caso:** 1 mensagem
- **Pior caso:** N−1 mensagens (espera a volta completa)

---

## Exercício: Ricart-Agrawala com 4 Processos

| Processo | Quer entrar? | Timestamp (T) |
|---|---|---|
| P1 | ✅ Sim | T = 10 |
| P2 | ❌ Não | — |
| P3 | ✅ Sim | T = 7 |
| P4 | ✅ Sim | T = 10, PID = 4 |

**a)** P3 entra primeiro. T=7 é o menor timestamp. P1 e P4 cederão imediatamente a P3.

**b)** P1 entra segundo, P4 entra terceiro. Mesmo timestamp (T=10), desempate pelo menor PID: P1 (PID=1) < P4 (PID=4).

**c)** P2 está em RELEASED → responde REPLY imediatamente a todos os três, sem enfileirar.

**Contagem de mensagens (N=4):** Cada processo usa 2(N−1) = **6 mensagens**. Para 3 entradas: 18 mensagens totais.

⚠️ **Starvation:** O algoritmo garante ausência de starvation, desde que timestamps sejam únicos e entrega garantida.

---

## Por que Precisamos Eleger um Líder?

Muitos algoritmos distribuídos requerem um **coordenador único**. Quando falha, o sistema precisa *eleger automaticamente um novo líder*.

Casos de uso:
- **Banco de Dados:** replicação master-slave (PostgreSQL + Patroni, MySQL Group Replication)
- **Orquestração:** Kubernetes/etcd (Raft), ZooKeeper, Consul
- **Microsserviços:** Kafka controller, Elasticsearch master

### Requisitos de um Algoritmo de Eleição
- **Safety:** todos os nós concordam no mesmo líder
- **Liveness:** a eleição sempre termina
- **Unicidade:** exatamente um líder eleito por vez

### 🎯 O Problema do "Split Brain"
Se dois nós acreditam que são líderes simultaneamente → estado inconsistente gravíssimo. Algoritmos corretos **garantem matematicamente** que isso não ocorre.

---

## Bully Algorithm: O Mais Forte Vence
*(GARCIA-MOLINA, H. IEEE Trans. Computers, v.C-31, n.1, p.48–59, jan. 1982)*

Proposto por Héctor Garcia-Molina em 1982 para sistemas *síncronos com falhas por crash*. O processo com o **maior ID** entre os ativos sempre ganha a eleição.

### Análise de Mensagens (N=6 processos)
Pior caso (P1 inicia): (N-1)+(N-2)+…+1 = **O(N²)**. Exemplo: P3 envia 3 + P4 envia 2 + P5 envia 1 = **6 mensagens ELECTION + 1 COORDINATOR**.

---

## Ring Election: Chang & Roberts (1979)
*(CHANG, E.; ROBERTS, R. Commun. ACM, v.22, n.5, p.281–283, mai. 1979)*

Processos formam um **anel lógico unidirecional**. Regras:
- Recebeu ID **maior** que o seu → encaminha
- Recebeu ID **menor** que o seu → *descarta*
- Recebeu o **seu próprio** ID → você é o líder! Envia ELECTED

### Complexidade
- **Pior caso:** O(N²) mensagens
- **Média:** O(N log N)
- **Mínimo:** 3N−1 mensagens

---

## Raft: Eleição de Líder Moderna
*(ONGARO, D.; OUSTERHOUT, J. USENIX ATC'14, p.305–319, 2014 — 🏆 Best Paper Award)*

Raft decompõe o problema de consenso em: **eleição de líder**, replicação de log e segurança. Base do etcd (Kubernetes), CockroachDB e Consul.

### Três estados de cada nó
- **Follower** — responde a líderes/candidatos
- **Candidate** — solicita votos, quer ser líder
- **Leader** — coordena tudo; envia heartbeats

Todo nó começa como **Follower**. Se não recebe heartbeat em um *timeout aleatório* (150–300ms), vira Candidate e inicia eleição.

### Processo de Eleição
- Candidate incrementa seu **term** (mandato)
- Vota em si mesmo, envia `RequestVote` a todos
- Quem receber maioria (> N/2) vira **Leader**
- *Split vote*: timeout expira, novo term começa

### Terms (Mandatos)
O Raft divide o tempo em **terms** numerados monotonicamente. Um nó que descobre que seu term está desatualizado **imediatamente** volta a ser Follower.

### Por que timers aleatórios?
Se todos os timers fossem iguais, todos acordariam ao mesmo tempo → split vote eterno. Com timers aleatórios (150–300ms), a chance de colisão é próxima de zero.

### Regras de Votação
- Cada nó vota em **no máximo 1** candidato por term
- Vota "sim" se: o candidato tem log tão atualizado quanto o meu **E** não votei ainda neste term
- Candidato precisa de **maioria absoluta** (> N/2 votos)
- Maioria garante que **dois candidatos nunca ganham** o mesmo term

**Analogia:** O professor (líder) saiu da sala. O aluno que primeiro notar levanta a mão (vira candidato). Quem conseguir que mais da metade dos colegas erga a mão de volta ganha. Se dois levantam ao mesmo tempo e empatam, todos contam até um tempo aleatório e tentam de novo.

### Split Vote
Se dois candidatos dividem os votos igualmente (ex: 5 servidores, 2 votos cada), **nenhum atinge maioria**. O term termina sem líder. Um novo term começa com novos timers aleatórios.

---

## Exercício: Bully em 5 Processos

| Processo | PID | Estado |
|---|---|---|
| P1 | 1 | ✅ Ativo |
| P2 | 2 | ✅ Ativo |
| P3 | 3 | ✅ Ativo (era o coordenador, **acabou de travar**) |
| P4 | 4 | ✅ Ativo |
| P5 | 5 | ✅ Ativo (**detecta a falha de P3**) |

**a)** P5 envia ELECTION para todos com PID > 5. Mas P5 tem o maior PID ativo! Não há processo com PID maior. Nenhum responde com OK (timeout). P5 envia COORDINATOR para P1, P2, P4.

**b)** P5 se torna o novo líder. Maior PID entre os ativos.

**c)** 0 mensagens ELECTION + 3 mensagens COORDINATOR = **3 mensagens no total.**
Pior caso (P1 inicia): N(N−1)/2 + 1 = **O(N²)**. Com N=5: 11 mensagens.

---

## Comparação Final

| Algoritmo | Tipo | Mensagens/acesso | Fault tolerance | Uso Real |
|---|---|---|---|---|
| Ricart-Agrawala | Permissão | 2(N−1) | Fraca: 1 falha trava tudo | Sistemas acadêmicos, referência |
| Token Ring | Token | 1 a N−1 | Média: token perdido é problema | IEEE 802.5 (legado) |
| Bully | Eleição | O(N²) pior caso | Média: reeleição automática | Sistemas síncronos |
| Ring Election | Eleição em Anel | O(N log N) média | Média: depende do anel intacto | Redes em anel |
| Raft | Consenso | O(N) por eleição | Alta: tolera N/2−1 falhas | **etcd, CockroachDB, Consul, TiDB** |

**Por que o Raft domina?**
- Projetado para ser **compreensível**
- Tolera N/2−1 nós falhando
- Adotado pelo Kubernetes (etcd), TiKV, CockroachDB
- 33/43 alunos entenderam Raft melhor que Paxos (estudo de usabilidade)

**Limitações práticas de R-A e Token Ring:**
- R-A: 1 processo falhando = sistema trava completamente
- Token Ring: detectar token perdido exige complexidade adicional

---

## Checkpoint — Questões de Consolidação

**Q1.** No Ricart-Agrawala, P3 (WANTED, T=5) recebe REQUEST de P7 (T=3). O que P3 faz?
✅ **Correto: C** — Responde REPLY imediatamente. T=3 (P7) < T=5 (P3): P7 tem prioridade.

**Q2.** Principal vantagem e desvantagem do Bully em relação ao Raft?
✅ **Correto: B** — Vantagem: simplicidade conceitual. Desvantagem: O(N²) mensagens e requer sistema síncrono.

**Q3.** Por que o Raft usa timers aleatórios?
✅ **Correto: C** — Para evitar que múltiplos nós se tornem candidatos simultaneamente, reduzindo split votes.

---

## Síntese da Aula

**3 Conceitos Essenciais:**
- Em SD, exclusão mútua via **mensagens**
- Permissão: menor timestamp = prioridade
- Eleição: maior ID ou maioria de votos

**No Mercado Hoje:**
- **Raft:** etcd, CockroachDB, Consul
- **ZooKeeper:** ZAB (similar ao Paxos)
- Redis: Raft-like na eleição de sentinels

**Próxima Aula:** Transações Distribuídas — Two-Phase Commit (2PC), Three-Phase Commit (3PC) e falhas de coordenador.

---

**📖 Referências:**
- RICART, G.; AGRAWALA, A.K. Commun. ACM, v.24, n.1, p.9–17, 1981.
- GARCIA-MOLINA, H. IEEE Trans. Computers, v.C-31, n.1, p.48–59, 1982.
- CHANG, E.; ROBERTS, R. Commun. ACM, v.22, n.5, p.281–283, 1979.
- ONGARO, D.; OUSTERHOUT, J. USENIX ATC'14, 2014.
- SUZUKI, I.; KASAMI, T. ACM TOCS, v.3, n.4, p.344–349, 1985.
- TANENBAUM, A.S.; VAN STEEN, M. *Distributed Systems*. 4. ed., 2023.
- KLEPPMANN, M. *Designing Data-Intensive Applications*. O'Reilly, 2017.