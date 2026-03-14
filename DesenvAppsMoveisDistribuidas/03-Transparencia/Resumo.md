# Transparência em Sistemas Distribuídos

-- Link do mateiral: <https://cristianoneto.online/puc/damd/aula04>

## Contexto Geral

Quando um aplicativo "simplesmente funciona" — música que não para, documento editado por várias pessoas ao mesmo tempo, transferência bancária processada sem erros — há uma engenharia complexa trabalhando nos bastidores para esconder toda essa complexidade. Esse princípio tem nome: **transparência**.

Segundo a norma **ISO/IEC 10746 (RM-ODP)**, transparência é a propriedade de um sistema distribuído de **ocultar a separação entre seus componentes** — fazendo com que máquinas diferentes, espalhadas pelo mundo, pareçam funcionar como uma única coisa.

---

## Os 7 Tipos de Transparência

### 1. Acesso
Esconde *como* o recurso é acessado (local ou remoto). O código do cliente é idêntico independentemente da origem dos dados. Padrão usado: **Repository + Strategy**.

### 2. Localização
Esconde *onde* o recurso está fisicamente. Em vez de IPs fixos no código, usa-se um **nome lógico** (ex: DNS — você digita `google.com`, não `142.250.218.78`). Padrão usado: **Service Discovery**.

### 3. Migração
Esconde que um recurso foi **movido enquanto estava inativo**. Exemplo: manutenção noturna que realoca um serviço sem que ninguém perceba. Técnica comum: armazenar estado em serviço externo como Redis.

### 4. Relocação
Esconde que um recurso se move **enquanto está em uso**. Mais difícil que migração — exige zero interrupção. Exemplo clássico: troca de antena no roaming celular sem cair a chamada.

> Diferenca-chave: **Migração** = recurso parado. **Relocação** = recurso em uso ativo.

### 5. Replicação
Esconde que existem **múltiplas cópias** do mesmo dado. Serve para acesso mais rápido e para tolerância a falhas — se uma cópia cair, as outras assumem. Técnica: pool de réplicas com roteamento automático.

### 6. Concorrência
Esconde que **vários usuários acessam o mesmo recurso ao mesmo tempo**. Exemplo perfeito: Google Docs, onde várias pessoas editam o mesmo parágrafo simultaneamente sem conflitos visíveis. Técnica: **lock distribuído** (ex: via Redis).

### 7. Falha
Esconde que um componente **falhou e foi substituído**. O objetivo é que o sistema se recupere tão rapidamente que o usuário não perceba. Técnica principal: **Circuit Breaker** (disjuntor) — monitora falhas consecutivas e redireciona o tráfego para um backup automaticamente.

---

## Quando NÃO aplicar transparência

Transparência total pode ser perigosa. Exemplo: o **PIX** exibe a mensagem "em análise" em vez de fingir que tudo ocorreu bem. Isso é intencional — em transações financeiras, confirmar uma operação que falhou seria um desastre. A honestidade sobre o estado da operação vale mais que a ilusão de perfeição.

Outro anti-pattern: tratar chamadas remotas como se fossem locais, escondendo latência e erros de rede — isso gera bugs silenciosos difíceis de rastrear.

---

## O que mais cai em prova

- **Definição de transparência**: ocultar a separação entre componentes distribuídos (ISO/IEC 10746).
- **Diferença entre Migração e Relocação**: parado x em uso.
- **Exemplos práticos por tipo**: DNS (localização), Google Docs (concorrência), roaming (relocação), Circuit Breaker (falha).
- **Transparência de falha**: Circuit Breaker desarma ao detectar falhas em sequência e redireciona para backup.
- **Limites da transparência**: nem sempre esconder é correto — confirmações obrigatórias (pagamentos) exigem feedback explícito ao usuário.