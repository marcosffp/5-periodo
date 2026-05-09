# Transações Distribuídas: ACID · 2PC · 3PC · Saga
**PUC Minas — DAMD — Aula 08 · 2026/1**

> *"The transaction concept allows programmers to be sloppy: they can write application programs as if the DBMS takes care of everything."* — GRAY, J.; REUTER, A. Transaction Processing: Concepts and Techniques. Morgan Kaufmann, 1993.

---

## 🎯 Objetivos da Aula

Ao final você saberá: o que são as propriedades ACID e por que são difíceis em SD; como o 2PC garante atomicidade distribuída e qual é seu calcanhar de Aquiles; como o 3PC tenta resolver o bloqueio; e quando usar o padrão Saga em microsserviços modernos.

---

## O que é uma Transação?

Uma **transação** é uma sequência de operações tratada como uma *unidade atômica de trabalho*: ou tudo acontece, ou nada acontece. O conceito surgiu em sistemas de banco de dados nos anos 1970.

### O Exemplo Clássico: Transferência Bancária

Transferir R$500 da conta A para conta B envolve 2 operações que devem ser atômicas:
- **Op 1:** DEBITAR R$500 da conta A
- **Op 2:** CREDITAR R$500 na conta B

Falha entre as operações → R$500 desaparece. Com transação: ambas acontecem ou nenhuma acontece.

### Analogia: Pedido de e-commerce

Ao finalizar uma compra, 3 sistemas são acionados: *estoque* (reservar item), *pagamento* (cobrar cartão) e *logística* (gerar etiqueta). Se o pagamento falhar após o estoque ser reservado, o estoque deve ser liberado automaticamente. Isso é coordenação transacional distribuída.

### Transação em Banco Local

```sql
BEGIN TRANSACTION;
UPDATE contas SET saldo = saldo - 500 WHERE id = 'A';
UPDATE contas SET saldo = saldo + 500 WHERE id = 'B';
COMMIT; -- ou ROLLBACK
```

### E em Sistemas Distribuídos?

A conta A está no Banco do Brasil (São Paulo) e a conta B está no Bradesco (Recife). O COMMIT local não é mais suficiente — precisamos que *ambos os bancos concordem* sobre o resultado. Isso exige um protocolo de coordenação.

---

## Propriedades ACID
*(HAERDER, T.; REUTER, A. ACM Computing Surveys, v.15, n.4, p.287–317, dez. 1983)*

O acrônimo ACID foi cunhado por Theo Härder e Andreas Reuter em 1983, formalizando propriedades que Jim Gray havia descrito em 1978.

**A — Atomicidade:** A transação é indivisível: ou todas as operações são executadas com sucesso, ou nenhuma é. Mecanismo: Write-Ahead Log (WAL) + ROLLBACK automático em caso de falha.

**C — Consistência:** A transação leva o banco de um estado válido a outro estado válido. Todas as restrições de integridade devem ser respeitadas.

**I — Isolamento:** Transações concorrentes não interferem entre si. Níveis: Read Uncommitted → Read Committed → Repeatable Read → Serializable.

**D — Durabilidade:** Uma vez commitada, a transação persiste mesmo em caso de falha de hardware, energia ou SO. Mecanismo: WAL forçado em disco antes do COMMIT ser confirmado.

**ACID em SD:** Em um único servidor, o SGBD garante ACID internamente. Em SD, a Atomicidade e o Isolamento são os mais difíceis: precisamos que N servidores independentes concordem sobre um commit, mesmo que alguns falhem.

---

## O Problema do Commit Distribuído

### Cenário Problemático: Sem Protocolo

- Coordenador envia "COMMIT" para S1, S2 e S3
- S1 e S2: committam ✅
- S3: **crash antes de receber!** ❌
- Resultado: S1 e S2 têm dados novos, S3 tem dados antigos → **inconsistência permanente!**

### O Teorema FLP

Fischer, Lynch e Paterson (1985) provaram o **Teorema FLP**: em um sistema assíncrono com pelo menos 1 processo falho, *não existe algoritmo determinístico* que sempre termine decidindo consenso. O 2PC e o 3PC trabalham com suposições práticas para contornar esse limite teórico.

### Requisitos do Commit Atômico Distribuído

- **AC1 — Acordo:** todos os processos decidem o mesmo valor (commit ou abort)
- **AC2 — Validade:** se algum vota "abort", o resultado é abort
- **AC3 — Não-trivialidade:** se todos estão ok e sem falhas, o resultado é commit
- **AC4 — Terminação:** todo processo correto eventualmente decide

### Analogia: A Cerimônia de Casamento

O oficial pergunta "Você aceita?" ao noivo, depois à noiva. Só há casamento se ambos dizem "Sim". Se um diz "Não" ou desmaia (falha), o casamento é abortado. Mas e se o oficial desmaiar *depois* de ouvir o "Sim" do noivo, *antes* de perguntar à noiva? O noivo fica preso — não sabe se deve se considerar casado ou não. Esse é exatamente o problema de bloqueio do 2PC.

---

## Two-Phase Commit (2PC)
*(GRAY, J. Notes on Data Base Operating Systems. LNCS, v.60, Springer, 1978)*

Proposto por Jim Gray em 1978, o 2PC é o protocolo padrão para commit atômico distribuído. É a base do padrão XA usado em bancos de dados relacionais distribuídos até hoje.

### FASE 1 — Preparação (Voting Phase)

- Coordenador envia `PREPARE` para todos os participantes
- Cada participante executa a transação localmente e grava em WAL
- Se conseguiu preparar: responde `VOTE-COMMIT`
- Se não conseguiu: responde `VOTE-ABORT`
- Participante em estado PREPARED não pode mais abortar unilateralmente

### FASE 2 — Decisão (Commit Phase)

- Se **todos** votaram COMMIT → envia `GLOBAL-COMMIT`
- Se **qualquer um** votou ABORT → envia `GLOBAL-ABORT`
- Participantes executam o commit/abort e respondem `ACK`
- Total: **4n mensagens** (n = participantes)

### Cenários de Falha

**Cenário 1 — Sucesso:** Coord → PREPARE → todos votam COMMIT → GLOBAL-COMMIT → todos committam ✅

**Cenário 2 — Participante vota Abort:** P3 vota ABORT → Coord envia GLOBAL-ABORT → P1 e P2 desfazem com WAL ✅

**Cenário 3 — Coordenador falha após Fase 1:** P1, P2, P3 estão em estado PREPARED → Coordenador crasha → ficam **bloqueados** com locks → não podem commitar nem abortar sozinhos → aguardam recuperação do coordenador 😱 — **Este é o bloqueio (blocking) do 2PC!**

### O Bloqueio: O Calcanhar de Aquiles do 2PC

Quando o coordenador falha *depois* de receber todos os VOTE-COMMITs mas *antes* de enviar o GLOBAL-COMMIT, os participantes ficam em estado PREPARED indefinidamente, segurando todos os locks de banco de dados. Gray e Reuter (1993) chamaram isso de *"the fundamental limitation of two-phase commit."*

---

## Three-Phase Commit (3PC): Resolvendo o Bloqueio
*(SKEEN, D. Nonblocking Commit Protocols. ACM SIGMOD, p.133–142, 1981)*

Proposto por Dale Skeen em 1981, o 3PC adiciona uma **fase intermediária "PRE-COMMIT"** que elimina o estado de incerteza que causa o bloqueio do 2PC — desde que no máximo 1 processo falhe por vez.

**FASE 1 — Voting:** Igual ao 2PC: Coordenador envia PREPARE, participantes votam.

**FASE 2 — PRE-COMMIT (NOVO!):** Coordenador envia PRE-COMMIT antes do commit final. Participantes confirmam.

**FASE 3 — COMMIT:** Coordenador envia COMMIT. Participantes em PRE-COMMIT podem decidir sozinhos.

### Por que o PRE-COMMIT resolve o bloqueio?

No 2PC, um participante em PREPARED não sabe se o coordenador decidiu COMMIT ou ABORT antes de crashar. No 3PC, ao receber PRE-COMMIT, o participante sabe que *todos votaram COMMIT*. Se o coordenador crashar agora, qualquer participante pode assumir a liderança e concluir o COMMIT.

**Custo:** 6n mensagens vs 4n do 2PC.

### Limitação do 3PC

O 3PC resolve o bloqueio *com falhas por crash*, mas é vulnerável a **particionamento de rede** (network partition): dois grupos de participantes podem tomar decisões diferentes (split brain). Por isso, o Raft/Paxos são preferidos em produção.

---

## Exercício: Diagnóstico de Falha em 2PC

### Enunciado

Uma transação distribuída T1 envolve 4 participantes (P1, P2, P3, P4) e 1 coordenador (C). Log antes do crash:

| Evento | Timestamp |
|---|---|
| START T1 | 10:00:01 |
| PREPARE enviado para P1, P2, P3, P4 | 10:00:05 |
| Recebido VOTE-COMMIT de P1 | 10:00:06 |
| Recebido VOTE-COMMIT de P2 | 10:00:07 |
| Recebido VOTE-COMMIT de P3 | 10:00:08 |
| **CRASH DO COORDENADOR** | 10:00:09 |

**Perguntas:** a) Em que estado estão P1, P2, P3 e P4 após o crash? b) O que acontece com os locks? c) O que o coordenador faz ao se recuperar? d) Se fosse 3PC, como seria diferente?

### Solução

**a) Estados após o crash:**
- P1, P2, P3: estado PREPARED — votaram COMMIT e aguardam a decisão global. Não podem commitar nem abortar unilateralmente.
- P4: estado INITIAL ou PREPARED — o coordenador não sabe.

**b) Locks:** P1, P2 e P3 mantêm **todos os locks adquiridos** durante a preparação, ficando presos indefinidamente — bloqueando outras transações.

**c) Recuperação:** O coordenador lê o log e identifica que P4 não respondeu e a decisão COMMIT não foi gravada. Deve enviar **GLOBAL-ABORT** para garantir segurança. P1, P2 e P3 desfazem via WAL.

**d) Com 3PC:** Se P1, P2, P3 já receberam PRE-COMMIT, qualquer um pode assumir a coordenação e concluir com COMMIT. Se nenhum recebeu PRE-COMMIT, podem abortar com segurança.

---

## Saga Pattern: Transações em Microsserviços
*(GARCIA-MOLINA, H.; SALEM, K. ACM SIGMOD Record, v.16, n.3, p.249–259, 1987)*

O 2PC requer que todos os participantes estejam disponíveis ao mesmo tempo — impraticável em microsserviços. O padrão **Saga** decompõe uma transação longa em sequências de *transações locais* com *transações compensatórias* para rollback.

### Saga: Pedido de E-commerce

1. Serviço de Pedido — cria pedido (estado: PENDENTE)
2. Serviço de Pagamento — reserva valor no cartão
3. Serviço de Estoque — reserva os itens
4. Serviço de Entrega — agenda coleta → pedido CONFIRMADO ✅

**Se o Estoque falhar no Passo 3:**
- C2: Compensar Pagamento — estorna a reserva
- C1: Compensar Pedido — cancela com status CANCELADO

### Dois Tipos de Saga

**Coreografia:** cada serviço escuta eventos e reage. Descentralizado, mais difícil de depurar.

**Orquestração:** um orquestrador central (ex: Temporal.io, AWS Step Functions) coordena os passos. Mais fácil de visualizar o fluxo.

### Saga não garante Isolamento!

Durante a execução, dados intermediários *ficam visíveis* para outras transações. Saga oferece *consistência eventual*, não isolamento forte. Adequado para processos de negócio que tolerem isso.

**Onde é usado:** Amazon (Order Management System), Uber Eats, Netflix, Temporal.io, AWS Step Functions, Camunda.

---

## ACID vs BASE: O Grande Trade-off
*(PRITCHETT, D. BASE: An Acid Alternative. ACM Queue, v.6, n.3, p.48–55, 2008)*

### ACID
- Consistência **imediata e forte**
- Transações bloqueantes (locks)
- 2PC para commits distribuídos
- Escalabilidade **limitada**
- **Quando usar:** Sistemas financeiros, hospitalares, reservas de assentos.

### BASE (Basically Available · Soft state · Eventually consistent)
- Disponibilidade **máxima**
- Consistência **eventual**
- Escalabilidade **horizontal**
- **Quando usar:** Redes sociais, catálogos de produtos, analytics.

| Critério | ACID (ex: PostgreSQL) | BASE (ex: Cassandra) |
|---|---|---|
| Consistência | Forte e imediata | Eventual |
| Disponibilidade | Alta (com 2PC, pode bloquear) | Máxima |
| Escalabilidade | Limitada | Horizontal ilimitada |
| Conflitos | Impossíveis (locks) | Possíveis (LWW) |
| Uso real | Bancos, ERP, Saúde | IoT, Social, Analytics |

---

## O Teorema CAP

Eric Brewer enunciou o Teorema CAP em 2000 (provado por Gilbert e Lynch em 2002): um sistema distribuído só pode garantir **2 das 3** propriedades simultaneamente durante uma partição de rede.

- **C — Consistência:** Todos os nós veem os mesmos dados ao mesmo tempo.
- **A — Disponibilidade:** Toda requisição recebe uma resposta.
- **P — Tolerância a Partição:** O sistema funciona mesmo com perda de mensagens ou isolamento de rede.

| Escolha | Sacrifica | Exemplo |
|---|---|---|
| CP (C+P) | Disponibilidade | HBase, MongoDB, Redis |
| AP (A+P) | Consistência forte | Cassandra, DynamoDB, CouchDB |
| CA (C+A) | Tolerância a partição | Bancos relacionais tradicionais |

**CA na prática é impossível em SD** — partições de rede *vão acontecer*. A escolha real é sempre entre CP e AP.

### Analogia: ATMs Bancários

**Opção CP:** O ATM recusa operações se não conseguir verificar o saldo com o servidor central.
**Opção AP:** O ATM permite saques offline com limite conservador — disponível, mas pode haver overdraft temporário. Grandes bancos escolhem AP deliberadamente para caixas eletrônicos.

---

## Comparação: 2PC × 3PC × Saga

| Critério | 2PC | 3PC | Saga |
|---|---|---|---|
| Fases | 2 | 3 | N (uma por serviço) |
| Mensagens | 4n | 6n | Variável |
| Bloqueio | Sim | Não (falhas por crash) | Não |
| Isolamento ACID | Forte | Forte | Eventual |
| Tolerância a partição | Fraca | Fraca (split brain) | Alta |
| Complexidade | Baixa | Média | Alta |
| Escalabilidade | Baixa (locks) | Baixa | Alta |
| Uso principal | BD relacionais (XA) | Teórico / niche | Microsserviços modernos |

### Mundo Real: O que é usado hoje?

- **2PC/XA:** Oracle, PostgreSQL, IBM DB2 em sistemas legados e ERP
- **Raft+2PC híbrido:** Google Spanner
- **Saga + Orquestração:** AWS Step Functions, Temporal.io, Uber Cadence
- **Sem transações:** Cassandra, DynamoDB — consistência eventual

### Por que o 3PC não dominou o mercado?

- Custo de 6n mensagens vs 4n do 2PC
- Particionamento de rede ainda causa split brain
- Consenso baseado em Raft/Paxos resolve melhor o problema geral
- Saga + eventual consistency é mais prático para microsserviços

---

## Exercício: Projetando uma Saga

### Enunciado

Sistema de reservas de uma companhia aérea com 4 serviços:

| # | Serviço | Ação |
|---|---|---|
| 1 | Reservas | Reserva o assento (LIVRE → PENDENTE) |
| 2 | Pagamento | Cobra R$800 no cartão |
| 3 | Fidelidade | Adiciona 800 milhas |
| 4 | Notificação | Envia e-mail e SMS de confirmação |

### Solução

**a) Transações compensatórias:**

| Passo Original | Compensação |
| --- | --- |
| 1 — Reserva assento (PENDENTE) | Libera assento (PENDENTE → LIVRE) |
| 2 — Cobra R$800 | Estorna R$800 (pode levar 1-5 dias úteis) |
| 3 — Adiciona 800 milhas | Remove 800 milhas |
| 4 — Envia e-mail/SMS | Ver item (b) |

**b)** O Passo 4 (Notificação) é **não compensável**. Você não pode "desenviar" um e-mail ou SMS já entregue. A compensação seria enviar outra notificação informando o cancelamento. Por isso, notificações são frequentemente colocadas *no final da saga*, após todas as ações reversíveis serem confirmadas.

**c)** A propriedade violada é o **Isolamento (I) do ACID**. Outras transações podem ver o assento em estado PENDENTE — fenômeno de *dirty read*. Em contexto de negócio, isso é geralmente aceitável: é preferível mostrar o assento como PENDENTE (evitando dupla venda) a bloquear toda a consulta de disponibilidade. O estado PENDENTE com timeout (ex: 10 minutos) é um padrão comum em e-commerce e passagens aéreas.

---

## Checkpoint — Verificação de Aprendizado

**Q1.** Um participante respondeu VOTE-COMMIT e o coordenador crashou antes de enviar GLOBAL-COMMIT. O participante pode abortar unilateralmente?

✅ **Correto: C — Não.** Ao enviar VOTE-COMMIT, o participante cedeu o poder de decisão ao coordenador. Não pode abortar sozinho — isso violaria a atomicidade. O correto é aguardar o coordenador.

**Q2.** Diferença entre Saga e 2PC?

✅ **Correto: B.** Saga decompõe a transação em passos locais com compensações — sem bloqueio global, mas estados intermediários ficam visíveis (sem isolamento). 2PC garante atomicidade forte de forma síncrona, mas pode bloquear se o coordenador falhar.

**Q3.** E-commerce usa Cassandra para catálogo e PostgreSQL para pedidos/pagamentos. Que princípio reflete?

✅ **Correto: B — Polyglot Persistence.** Usar o banco mais adequado para cada necessidade. Catálogo tolera inconsistência leve → Cassandra (AP). Pedidos e pagamentos exigem ACID → PostgreSQL (CP). Netflix, Amazon e Spotify usam dezenas de tipos diferentes de banco de dados.

---

## Síntese da Aula

### 3 Conceitos-Chave

- **ACID** = contrato de confiabilidade de transações
- **2PC** = votação em 2 fases, bloqueante se coordenador falha
- **Saga** = consistência eventual com compensações

### No Mercado Hoje

- **2PC/XA:** legado empresarial, Oracle, IBM
- **Saga/Temporal:** Amazon, Uber, Netflix
- **Raft+2PC:** Google Spanner, CockroachDB

---

## Referências

- GRAY, J. Notes on Data Base Operating Systems. LNCS, v.60, Springer, 1978.
- HAERDER, T.; REUTER, A. ACM Computing Surveys, v.15, n.4, p.287–317, 1983.
- SKEEN, D. ACM SIGMOD, p.133–142, 1981.
- GARCIA-MOLINA, H.; SALEM, K. ACM SIGMOD, v.16, n.3, p.249–259, 1987.
- GRAY, J.; REUTER, A. Transaction Processing. Morgan Kaufmann, 1993.
- BREWER, E.A. PODC Keynote, 2000.
- GILBERT, S.; LYNCH, N. ACM SIGACT News, v.33, n.2, 2002.
- PRITCHETT, D. ACM Queue, v.6, n.3, 2008.
- KLEPPMANN, M. Designing Data-Intensive Applications. O'Reilly, 2017.

---

Todo o texto do site foi extraído acima. É o conteúdo completo da Aula 08 de DAMD (Dados em Ambientes Distribuídos) da PUC Minas, cobrindo transações distribuídas: ACID, 2PC, 3PC, Saga Pattern, BASE e o Teorema CAP.