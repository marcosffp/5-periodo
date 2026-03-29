# Relógios Lógicos — Lamport · Vetoriais · Causalidade em SD
**PUC Minas — ICEI — Aplicações Móveis e Distribuídas — Aula 06**
**Professor:** Cristiano de Macedo Neto

*Aula de reforço — Tópico 05 · Coordenação e Sincronização*

*"The concept of time is fundamental to our way of thinking and we tend to take it for granted. But the nature of time in a distributed system is far more subtle than it might seem."*
— Leslie Lamport · *Time, Clocks, and the Ordering of Events in a Distributed System* · CACM v.21, n.7, 1978

**🎯 O que você vai aprender:**
- Por que relógios físicos falham em sistemas distribuídos
- Como o algoritmo de Lamport funciona passo a passo
- Por que relógios vetoriais são mais poderosos — e quando usar cada um
- Onde Amazon Dynamo, Git e Riak usam esses conceitos hoje em produção

---

## Por que Relógios Físicos Falham em SD?

### Situação do dia a dia — grupo de WhatsApp
Alice (São Paulo, UTC-3) envia: *"Vamos marcar na sexta?"* às 14h00. Bob (Lisboa, UTC+1) responde: *"Confirmado!"* às 14h01 horário de Lisboa — que equivale a 13h01 UTC. O servidor registra: **resposta de Bob com timestamp MENOR que a pergunta de Alice**. Uma resposta antes da pergunta. O relógio físico mentiu.

### ⚠️ Clock Skew — dessincronia entre máquinas
Mesmo com NTP (Network Time Protocol), podem existir diferenças de **dezenas a centenas de ms** entre os relógios de nós diferentes. Um evento "anterior" pode ter timestamp maior que um evento "posterior" em outro nó.

### ⚠️ Clock Drift — relógios derivam com o tempo
Cristais de quartzo oscilam em frequências ligeiramente diferentes. Após horas, a diferença pode ser de **segundos**. Correções do NTP geram saltos abruptos no tempo — ainda mais confusos para o sistema.

### 💡 A solução de Lamport: jogar fora o relógio de parede
Em vez de tentar sincronizar relógios físicos (impossível com precisão absoluta), Lamport propôs: **use apenas a ordem dos eventos, não o tempo absoluto**. Para coordenação distribuída, só precisamos saber *o que veio antes do quê* — não a hora exata.

---

## A Relação "Happens-Before" (→)

Lamport definiu uma relação que captura **causalidade potencial** entre eventos, sem precisar de relógio físico. Lê-se: *"a pode ter influenciado b"*.

**1. Ordem dentro do mesmo processo:** Se **a** e **b** são eventos no *mesmo processo* e **a** ocorre antes de **b**, então **a → b**.

**2. Envio e recebimento de mensagem:** Se **a** é o *envio* de uma mensagem e **b** é o *recebimento* dessa mesma mensagem, então **a → b**. Uma mensagem não pode chegar antes de ser enviada.

**3. Transitividade:** Se **a → b** e **b → c**, então **a → c**.

### 🟣 Eventos Concorrentes (a ∥ b)
Se **nem a → b nem b → a**, os eventos são **concorrentes**. Não há relação causal — aconteceram independentemente. A relação → define uma **ordem parcial** — existem pares de eventos simplesmente incomparáveis.

---

## Algoritmo do Relógio de Lamport — As 3 Regras

Cada processo mantém um **contador inteiro C** (inicializado em 0). Objetivo: garantir que se **a → b** então **C(a) < C(b)**.

**R1 — Evento interno (local):**
`C := C + 1`
Antes de qualquer evento interno, o processo incrementa seu relógio. Garante que eventos locais sejam ordenados.

**R2 — Envio de mensagem:**
`C := C + 1 → enviar(mensagem, timestamp = C)`
Incrementa o relógio, depois envia a mensagem com o **timestamp atual embutido**.

**R3 — Recebimento de mensagem:**
`C := max(C_local, C_mensagem) + 1`
Pega o **maior** entre o relógio local e o timestamp da mensagem, depois incrementa. Garante que o recebimento seja posterior a qualquer evento anterior à mensagem.

**Analogia — o assistente de reuniões:** Cada processo numera suas ações sequencialmente. Ao receber um e-mail com numeração maior, você "atualiza" sua numeração para continuar dali — garantindo que sua resposta seja sempre mais recente que a pergunta.

---

## A Grande Limitação dos Relógios de Lamport

O algoritmo garante: **se a → b, então C(a) < C(b)**. Mas a recíproca **não é válida**: C(a) < C(b) *não* implica a → b.

### O problema crítico
Dois eventos **concorrentes** (sem relação causal) podem ter timestamps diferentes. Ao ver C(a)=3 e C(b)=7, você *não pode afirmar* que a causou b. Pode ter sido coincidência de numeração.

- ✅ **O que Lamport garante:** se soubermos que **a → b**, então obrigatoriamente C(a) < C(b).
- ❌ **O que Lamport NÃO garante:** C(a)=3 < C(b)=7 **não implica** que a → b. **Lamport não detecta concorrência.**

### 💡 A solução: Relógios Vetoriais
Fidge (1988) e Mattern (1989), independentemente, propuseram guardar um **vetor de contadores** — um por processo. Isso torna a relação causal **bidirecional** e detecta concorrência com precisão.

---

## Relógios Vetoriais — Ideia Central

**Analogia — planilha compartilhada do grupo:** Cada membro do grupo mantém uma planilha com **uma coluna por participante**, anotando quantas mensagens cada um enviou (até onde ele sabe). Ao receber uma mensagem, atualiza a planilha com o **máximo** entre o que sabia e o que está na mensagem. Isso é exatamente um relógio vetorial.

### Estrutura do vetor
Para N processos, cada processo `Pᵢ` mantém `Vᵢ[1..N]`:
- `Vᵢ[i]` = número de eventos do próprio processo
- `Vᵢ[j]` = o que `Pᵢ` sabe sobre `Pⱼ`

*Ex: VP1 = [3, 1, 0] → "P1 fez 3 eventos, P2 fez 1 (que sei), P3 fez 0"*

### As 3 Regras
- **Evento interno:** `Vᵢ[i] := Vᵢ[i] + 1`
- **Envio de mensagem:** `Vᵢ[i]++ → enviar(m, Vᵢ)`
- **Recebimento:** `∀j: Vᵢ[j] = max(Vᵢ[j], Vm[j])` → depois: `Vᵢ[i]++`

### 💡 A propriedade fundamental — bicondicional
Com relógios vetoriais: **a → b ⟺ V(a) < V(b)**
Funciona em *ambas as direções*! Se V(a) e V(b) são incomparáveis, então **a ∥ b** (concorrentes). Lamport só garantia uma direção.

---

## Como Comparar Vetores e Detectar Concorrência

**→ a causalmente precede b (a → b):**
`V(a) < V(b) ⟺ ∀i: V(a)[i] ≤ V(b)[i] ∧ ∃j: V(a)[j] < V(b)[j]`
*Ex: V(a)=[1,0,0] e V(b)=[2,1,0]: 1≤2, 0≤1, 0≤0 e pelo menos um estrito → a → b ✅*

**∥ a e b são concorrentes (a ∥ b):**
`¬(V(a) < V(b)) ∧ ¬(V(b) < V(a))` — vetores incomparáveis
*Ex: V(a)=[2,0,0] e V(b)=[0,0,1]: a[1]=2>b[1]=0 mas a[3]=0<b[3]=1 → incomparáveis → a ∥ b 🔶*

### Comparação final: Lamport vs. Vetorial

| Critério | Relógio de Lamport | Relógio Vetorial |
|---|---|---|
| Tipo do timestamp | Número inteiro único | Vetor de N inteiros |
| a→b implica C(a)<C(b)? | ✅ Sim | ✅ Sim |
| C(a)<C(b) implica a→b? | ❌ Não | ✅ Sim (bicondicional) |
| Detecta concorrência? | ❌ Não | ✅ Sim, com precisão |
| Overhead de memória | O(1) — 1 inteiro | O(N) — cresce com nós |
| Uso típico | Ordenação total, mutex distribuído | Detecção de conflitos, replicação |

---

## Implementação em Python — Lado a Lado

**Relógio de Lamport:**
```python
class LamportClock:
    def __init__(self):
        self.time = 0

    def event(self):
        self.time += 1
        return self.time

    def receive(self, msg_ts):
        self.time = max(self.time, msg_ts) + 1
        return self.time
```

**Relógio Vetorial:**
```python
class VectorClock:
    def __init__(self, pid, n):
        self.pid = pid
        self.v = [0] * n

    def event(self):
        self.v[self.pid] += 1
        return list(self.v)

    def receive(self, msg_v):
        for i in range(len(self.v)):
            self.v[i] = max(self.v[i], msg_v[i])
        self.v[self.pid] += 1
        return list(self.v)
```

**A diferença estrutural chave:** No Lamport, `receive` faz `max(local, msg)+1` — um único inteiro. No vetorial, faz `max()` *componente a componente* e depois incrementa **apenas a posição do próprio processo**. Cada processo só escreve no seu próprio slot do vetor — nunca no slot de outro.

---

## Aplicações Reais — Esses Conceitos Estão em Produção

**🛒 Amazon Dynamo (2007):** Usa **relógios vetoriais** para rastrear versões de objetos. Escritas concorrentes geram vetores incomparáveis → sistema retorna *ambas as versões* ao cliente para reconciliação semântica. Para carrinho de compras: reconciliação = **união dos itens**. Nenhuma compra é perdida.

**🔵 Riak KV — Dotted Version Vectors:** Evoluiu de relógios vetoriais simples para **Dotted Version Vectors (DVVs)** — elimina explosão de versões concorrentes. Desenvolvido em parceria com a **Universidade do Minho, Portugal**.

**🌿 Git — DAG de causalidade:** O grafo de commits é um **DAG** que implementa happens-before: branch = processo independente; merge commit = recebimento de mensagem; conflito de merge = escritas concorrentes.

**⚠️ Cassandra — a escolha oposta:** **Não usa relógios vetoriais**. Usa *last-write-wins* com timestamp físico. Vantagem: sem overhead. Risco documentado: *clock skew* pode causar **perda silenciosa de dados** (testes Jepsen de Kyle Kingsbury).

---

## Exercício 1 — Relógio de Lamport

**Três processos (P1, P2, P3), todos com C=0:**

1. P1 faz evento interno. Qual C(P1)?
2. P1 envia mensagem para P2. Qual o timestamp enviado? Qual C(P1) após envio?
3. P2 faz evento interno (antes de receber qualquer mensagem). C(P2)=?
4. P2 recebe a mensagem de P1. Qual C(P2) após recebimento?
5. P3 faz dois eventos internos independentes. C(P3)=?
6. Os eventos de P3 têm relação a→b com os de P1? Por quê?

### ✅ Gabarito
1. **C(P1)=1.** R1: C:=0+1=1.
2. **Timestamp=2; C(P1)=2.** R2: incrementa primeiro (1+1=2), depois envia com ts=2.
3. **C(P2)=1.** Aplica R1: C:=0+1=1.
4. **C(P2)=3.** R3: max(1,2)+1=3.
5. **C(P3)=2.** Dois R1 sequenciais: 0+1=1, 1+1=2.
6. **Não há relação happens-before.** P3 nunca trocou mensagens com P1. Os eventos são **concorrentes** (P3 ∥ P1). C(P3)=1 < C(P1)=2 *não* implica causalidade — exatamente o limite de Lamport!

**Nível Intermediário:**

P1: evento(C=**1**) → envia m1 (C=**2**, ts=2)
P2: evento(C=**1**) → recebe m1(ts=2) → C=**max(1,2)+1=3** → envia m2 (C=**4**)
P3: evento(C=**1**) → evento(C=**2**) → recebe m2(ts=4) → C=**max(2,4)+1=5**

Cadeia causal: P1(1)→P1(2)→P2(3)→P2(4)→P3(5). Os dois primeiros eventos de P3 (C=1,2) são **concorrentes** com P1 e P2.

---

## Exercício 2 — Relógios Vetoriais (N=3, todos iniciam em [0,0,0])

| # | Ação | Processo | Vetor após [P1,P2,P3] |
|---|---|---|---|
| 1 | Evento interno | P1 | **[1,0,0]** |
| 2 | Evento interno | P2 | **[0,1,0]** |
| 3 | P1 envia m para P2 | P1 | **[2,0,0]** |
| 4 | P2 recebe m de P1 | P2 | **[2,2,0]** — max([0,1,0],[2,0,0])=[2,1,0] → P2[2]++ → [2,2,0] |
| 5 | P3 faz evento interno | P3 | **[0,0,1]** |
| 6 | P2 envia m' para P3 | P2 | **[2,3,0]** |
| 7 | P3 recebe m' de P2 | P3 | **[2,3,2]** — max([0,0,1],[2,3,0])=[2,3,1] → P3[3]++ → [2,3,2] |

**Q1:** V(ev2)=[0,1,0] < V(ev4)=[2,2,0] → **ev2 → ev4** ✅

**Q2:** V(ev5)=[0,0,1] vs V(ev3)=[2,0,0]. ev5[1]=0<ev3[1]=2 mas ev5[3]=1>ev3[3]=0 → **incomparáveis → concorrentes** ✅

**Q3:** Todos os eventos 1–6 são causalmente anteriores ao evento 7 (V(ev7)=[2,3,2] é maior que todos os outros).

---

## Checkpoint — Questões de Verificação

**Q1.** P1 tem C=5. Recebe mensagem de P2 com ts=8. Qual C(P1) após o recebimento?
✅ **C = 9** — max(5,8)+1 = 8+1 = 9. O +1 garante que o recebimento seja posterior ao envio.

**Q2.** V(a)=[3,1,0] e V(b)=[1,2,0]. Qual a relação entre esses eventos?
✅ **Concorrentes (a ∥ b)** — V(a)[1]=3>V(b)[1]=1 mas V(a)[2]=1<V(b)[2]=2 → vetores incomparáveis. Isso Lamport escalar não consegue detectar!

**Q3.** No Amazon Dynamo, quando dois vetores são incomparáveis ao comparar versões de um objeto, o sistema:
✅ **Retorna ambas as versões ao cliente para reconciliação** — o Dynamo retorna ambas as versões e o cliente implementa reconciliação semântica. Para carrinho de compras: união dos itens.

---

**📚 Referências (ABNT):**
- LAMPORT, L. Time, Clocks, and the Ordering of Events in a Distributed System. *Communications of the ACM*, v.21, n.7, p.558–565, 1978.
- FIDGE, C.J. Timestamps in Message-Passing Systems That Preserve the Partial Ordering. *Proc. 11th Australian Computer Science Conference*, v.10, n.1, p.56–66, 1988.
- MATTERN, F. Virtual Time and Global States of Distributed Systems. *Parallel and Distributed Algorithms*, p.215–226, 1989.
- DeCANDIA, G. et al. Dynamo: Amazon's Highly Available Key-Value Store. *ACM SOSP*, p.205–220, 2007.
- TANENBAUM, A.S.; VAN STEEN, M. *Distributed Systems*. 4.ed. Cap.6, 2023.
- KLEPPMANN, M. *Designing Data-Intensive Applications*. O'Reilly, 2017. Cap.8–9.