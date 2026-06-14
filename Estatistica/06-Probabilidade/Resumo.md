# Resumo — Cálculo de Probabilidades

## 0. Visão Geral: para que serve tudo isso?

Esse capítulo é a **base** de tudo que vem depois (inclusive do capítulo de Variáveis Aleatórias). A ideia central é:

> Dar um significado **numérico** à chance de algo acontecer, sempre entre 0 (impossível) e 1 (certeza), e fornecer **regras** para calcular essa chance em situações cada vez mais complexas.

O capítulo segue uma linha lógica:

1. **Vocabulário básico**: Experimento, Espaço Amostral, Evento.
2. **Relações entre eventos**: Complemento, Mutuamente Excludentes, Coletivamente Exaustivos (a "gramática" que usamos para combinar eventos).
3. **Definição formal de Probabilidade** (axiomas) e os **Teoremas** que derivam diretamente dela.
4. **Espaços Equiprováveis**: o caso mais simples e mais cobrado — "casos favoráveis / casos totais".
5. **Probabilidade Condicional**: o que muda quando já sabemos que algo aconteceu.
6. **Independência**: o caso especial em que "saber B" não muda nada sobre A.
7. **Teorema de Bayes**: "inverter" a condicional — descobrir a causa mais provável dado um efeito observado.

Guarde essa "espinha dorsal": ela vai te ajudar a não se perder no meio das fórmulas. Esse capítulo é o que dá sentido a expressões como `P(x)`, `f(x,y)` etc. do capítulo de Variáveis Aleatórias.

---

## 1. Conceitos Fundamentais

### 1.1. Experimento Aleatório (E)

> É qualquer processo de coleta de dados cujo resultado **varia**, mesmo repetido sob as mesmas condições.

Características de um Experimento Aleatório:

- Pode ser **repetido indefinidamente**, sob as mesmas condições;
- **Não é possível prever** o resultado exato *a priori* (antes de realizar);
- Quando repetido muitas vezes, a **frequência relativa** `f = s/n` (onde `s` = número de sucessos e `n` = número de repetições) se estabiliza.

### 1.2. Espaço Amostral (S)

> É o **conjunto de todos os resultados possíveis** de um experimento.

- **Espaço Amostral Discreto**: número finito (ou contável) de resultados.
- **Espaço Amostral Contínuo**: resultados formam um intervalo de números reais.

#### Exemplo 6.1 — lançar um dado

```
S = {1, 2, 3, 4, 5, 6}
```

#### Exemplo 6.2 — lançar dois dados simultaneamente

```
        ⎧ (1,1) (1,2) (1,3) (1,4) (1,5) (1,6) ⎫
        ⎪ (2,1) (2,2) (2,3) (2,4) (2,5) (2,6) ⎪
        ⎪ (3,1) (3,2) (3,3) (3,4) (3,5) (3,6) ⎪
   S =  ⎨ (4,1) (4,2) (4,3) (4,4) (4,5) (4,6) ⎬     → 36 resultados
        ⎪ (5,1) (5,2) (5,3) (5,4) (5,5) (5,6) ⎪
        ⎪ (6,1) (6,2) (6,3) (6,4) (6,5) (6,6) ⎪
        ⎩                                     ⎭
```

### 1.3. Evento

> Um **Evento** é qualquer **subconjunto** do Espaço Amostral. É representado por letras maiúsculas (exceto "E" e "S", que já têm outro significado).

Existem **três Eventos especiais**:

```
Evento Certo      ⇔  A = S          → sempre ocorre
Evento Impossível ⇔  A = ∅          → nunca ocorre
Evento Simples    ⇔  evento com um único resultado de S
```

#### Exemplo — eventos a partir do dado (Exemplo 6.1)

```
A = { números pares }         = {2, 4, 6}
B = { números maiores que 4 } = {5, 6}
C = { números múltiplos de 3 }= {3, 6}
```

#### Exemplo — eventos a partir de dois dados (Exemplo 6.2)

```
A = {(x,y) / x+y = 6}   = {(1,5),(2,4),(3,3),(4,2),(5,1)}
B = {(x,y) / x-y > 2}   = {(4,1),(5,1),(5,2),(6,1),(6,2),(6,3)}
```

### Operações entre eventos (vêm da teoria de conjuntos)

Como Espaço Amostral e Eventos são **conjuntos**, valem as operações usuais:

```
A ∪ B  → evento que ocorre se A OU B (ou ambos) ocorrem
A ∩ B  → evento que ocorre se A E B ocorrem simultaneamente
Ā      → evento que ocorre se A NÃO ocorrer (complemento de A)
```

> **Dica:** sempre que ler "ou" pense em **união (∪)**; sempre que ler "e" / "simultaneamente" pense em **intersecção (∩)**; sempre que ler "não" pense em **complemento (Ā)**.

---

## 2. Eventos Complementares, Mutuamente Excludentes e Coletivamente Exaustivos

Essas três relações descrevem **como os eventos se "encaixam"** dentro do Espaço Amostral.

### 2.1. Complemento (Ā ou Aᶜ)

> Reúne **todos os resultados de S que não pertencem a A**.

```
A ∩ Ā = ∅      (A e seu complemento nunca ocorrem juntos)
A ∪ Ā = S      (A e seu complemento, juntos, formam o S inteiro)
```

#### Exemplo 6.4

```
S = {1,2,3,4,5,6,7,8,9,10,11,12}
A = {2,5,7,8}        →  Ā = {1,3,4,6,9,10,11,12}
B = {2,3,5,8,10,12}  →  B̄ = {1,4,6,7,9,11}
```

### 2.2. Mutuamente Excludentes

> Dois (ou mais) eventos são **Mutuamente Excludentes** se a ocorrência de um **exclui** a possibilidade de ocorrência do outro — ou seja, **não pode existir intersecção** entre eles.

```
A e B são Mutuamente Excludentes  ⇔  A ∩ B = ∅
```

#### Exemplo 6.5

```
S = {1,2,3,4,5,6,7,8,9,10,11,12}
A = {1,3,5,8,11}    B = {2,6,9,12}    C = {2,4,7,8,10,12}

A e B SÃO Mutuamente Excludentes,     pois A ∩ B = ∅
A e C NÃO são Mutuamente Excludentes, pois A ∩ C = {8}
B e C NÃO são Mutuamente Excludentes, pois B ∩ C = {2,12}
```

### 2.3. Coletivamente Exaustivos

> Dois (ou mais) eventos são **Coletivamente Exaustivos** se **pelo menos um deles** tem que ocorrer em cada realização do experimento — ou seja, a **união de todos eles é igual ao Espaço Amostral**.

```
A, B, C, ... são Coletivamente Exaustivos  ⇔  A ∪ B ∪ C ∪ ... = S
```

#### Exemplo 6.6

```
S = {1,2,3,4,5,6,7,8,9,10,11,12}
A = {3,4,6,8}   B = {1,7,8,11,12}   C = {1,2,5,6,11,12}   D = {2,4,6,9,10,11}

A ∪ B ∪ C ∪ D = S   →  A, B, C e D são Coletivamente Exaustivos
```

> **Explicação simples:** "Mutuamente Excludentes" responde "esses eventos podem acontecer **juntos**?" (não, se forem M.E.). "Coletivamente Exaustivos" responde "esses eventos **cobrem tudo**?" (sim, se forem C.E.). Os dois conceitos são **independentes** — um evento e seu complemento, por exemplo, são ao mesmo tempo Mutuamente Excludentes **e** Coletivamente Exaustivos.

---

## 3. Definição de Probabilidade e Axiomas

### Definição

> A Probabilidade de um evento A, denotada `P(A)`, é uma **função** definida sobre o Espaço Amostral S que associa a cada evento um número real, satisfazendo os seguintes **axiomas**:

```
a.1)  0 ≤ P(A) ≤ 1                                    → toda probabilidade está entre 0 e 1
a.2)  P(S) = 1                                        → o evento certo tem probabilidade 1
a.3)  Se A e B são Mutuamente Excludentes, então
      P(A ∪ B) = P(A) + P(B)                          → probabilidades de eventos M.E. se somam
```

### Explicação simples

Esses três axiomas são as "regras do jogo" — **tudo o que vem depois (teoremas, fórmulas de espaço equiprovável, condicional, Bayes etc.) é consequência apenas desses três fatos**. Vale a pena memorizá-los bem, porque as demonstrações dos teoremas seguintes sempre voltam a eles.

---

## 4. Principais Teoremas

Esses teoremas **derivam diretamente dos axiomas** da seção 3 — e caem bastante em prova tanto como fórmula quanto como demonstração.

### T.1) A probabilidade do conjunto vazio é ZERO

```
P(∅) = 0
```

### T.2) Probabilidade do Complemento

```
P(Ā) = 1 - P(A)
```

**Por quê?** Como `S = A ∪ Ā` e `A ∩ Ā = ∅` (são Mutuamente Excludentes), pelo axioma a.3:
`P(S) = P(A) + P(Ā)`. Como `P(S) = 1` (axioma a.2), temos `1 = P(A) + P(Ā)`, logo `P(Ā) = 1 - P(A)`.

> **Macete:** sempre que o problema pedir **"pelo menos um"**, pense no complemento: `P(pelo menos um) = 1 - P(nenhum)`. É quase sempre mais fácil calcular "nenhum" e depois subtrair de 1.

### T.3) Se A ⊂ B, então P(B) ≥ P(A)

> Se A está **contido** em B (A é um "subconjunto" de B), então a probabilidade de B é **maior ou igual** à de A.

**Por quê?** Podemos escrever `B = A ∪ (Ā ∩ B)`, e essas duas partes são Mutuamente Excludentes. Logo `P(B) = P(A) + P(Ā ∩ B)`. Como `P(Ā ∩ B) ≥ 0` (axioma a.1), conclui-se que `P(B) - P(A) ≥ 0`, ou seja, `P(B) ≥ P(A)`.

### T.4) Regra da Soma (eventos quaisquer)

```
P(A ∪ B) = P(A) + P(B) - P(A ∩ B)
```

### Explicação simples

Essa é a fórmula **mais usada** do capítulo. Pense num diagrama de Venn: se você simplesmente somar `P(A) + P(B)`, a região onde A e B se **sobrepõem** (`A ∩ B`) é **contada duas vezes**. Por isso é preciso **subtrair** `P(A ∩ B)` uma vez, para "corrigir" essa dupla contagem.

```
   ┌─────────────┐
   │   A    ┌────┼────┐
   │     ╔══╪════╪══╗ │     P(A∪B) = P(A) + P(B) - P(A∩B)
   │     ║ A∩B   ║ B │      (a área hachurada A∩B foi
   │     ╚══╪════╪══╝ │      somada duas vezes, então
   └────────┘    └────┘      tiramos uma vez de volta)
```

> **Dica:** se A e B são Mutuamente Excludentes, `A ∩ B = ∅`, então `P(A ∩ B) = 0` e a fórmula "cai" de volta no axioma a.3: `P(A ∪ B) = P(A) + P(B)`. O Axioma a.3 é, portanto, um **caso particular** do Teorema T.4.

---

## 5. Probabilidades Finitas dos Espaços Amostrais Finitos

> Seja `S = {a₁, a₂, a₃, ..., aₙ}` um Espaço Amostral Finito. A cada evento simples `{aᵢ}` associamos um número `pᵢ`, satisfazendo:

```
c.1)  pᵢ > 0,  para i = 1, 2, ..., n        → nenhuma probabilidade individual é zero ou negativa

c.2)  Σ pᵢ = p₁ + p₂ + ... + pₙ = 1          → a soma de todas vale 1 (100%)
```

A probabilidade `P(A)` de qualquer evento composto A é a **soma das probabilidades dos eventos simples** que formam A.

#### Exemplo 6.7 — montando um sistema de equações

Três atiradores A, B e C competem. O atirador A tem **3x** mais chance de acertar que B; B tem **2x** mais chance que C. Quais as probabilidades de vitória de cada um?

**Passo 1 — traduzir as relações em equações:**

```
P(A) + P(B) + P(C) = 1        (c.2 — a soma de tudo é 1)
P(B) = 2 × P(C)                (B tem 2x mais chance que C)
P(A) = 3 × P(B) = 3 × 2×P(C) = 6 × P(C)
```

**Passo 2 — substituir tudo em função de P(C):**

```
6×P(C) + 2×P(C) + P(C) = 1
9 × P(C) = 1
```

**Resultado:**

```
P(C) = 1/9     P(A) = 6/9     P(B) = 2/9
```

> **Macete:** quando o problema dá relações do tipo "X tem n vezes mais chance que Y", escreva tudo em função da variável "menor" da cadeia (aqui, C) e use `c.2` (a soma = 1) para montar uma única equação.

---

## 6. Espaços Amostrais Finitos Equiprováveis

> Um Espaço Amostral é **equiprovável (ou uniforme)** quando **todos os resultados têm a mesma probabilidade**. Se S tem `n` resultados possíveis, cada um tem probabilidade `1/n`.

Se um evento composto A contém `r` resultados, então:

```
P(A) = r × (1/n) = r/n
```

ou, na notação mais usada:

```
            NCF(A)     Número de Casos Favoráveis ao evento A
P(A) = ------------- = -----------------------------------------
            NTC          Número Total de Casos
```

> **Dica:** esse é o tipo de probabilidade mais cobrado em prova com **dados, baralhos e urnas**. O segredo é **contar bem** — geralmente usando Análise Combinatória (Arranjos, Combinações, Permutações) para encontrar NCF e NTC.

#### Exemplo 6.8 — baralho de 52 cartas

```
A = { a carta é de ouros }    →  P(A) = NCF(A)/NTC = 13/52 = 1/4
B = { a carta é uma figura }  →  P(B) = NCF(B)/NTC = 12/52 = 3/13
```

(13 cartas de cada naipe; 12 figuras = J, Q, K de cada um dos 4 naipes)

#### Exemplo 6.9 — combinações (sem reposição, sem ordem)

Num lote de 12 peças, 4 são defeituosas. Duas peças são retiradas aleatoriamente. Calcule:

**a) Probabilidade de as duas peças serem defeituosas**

Como **a ordem não importa** e **não há reposição** → usar **Combinação**:

```
NCF(A) = C(4,2) = 4!/[(4-2)! × 2!] = 6      (pares possíveis entre as 4 defeituosas)
NTC    = C(12,2) = 12!/[(12-2)! × 2!] = 66  (pares possíveis entre as 12 peças)

P(A) = 6/66 = 1/11
```

**b) Probabilidade de as duas peças serem boas** (8 peças boas no total)

```
NCF(B) = C(8,2) = 28
P(B) = 28/66 = 14/33
```

**c) Probabilidade de pelo menos uma peça ser defeituosa**

> Aqui aparece de novo o **macete do complemento** (T.2)!

**1ª maneira (contagem direta):** "pelo menos uma defeituosa" = (1 boa e 1 defeituosa) OU (2 defeituosas)

```
NCF(C) = (4×8) + C(4,2) = 32 + 6 = 38
P(C) = 38/66 = 19/33
```

**2ª maneira (usando o complemento — mais rápida):** o complemento de "pelo menos uma defeituosa" é "nenhuma defeituosa" = "ambas boas" = evento B, já calculado no item (b)!

```
P(C) = 1 - P(B̄) = 1 - P(B) = 1 - 14/33 = 19/33
```

> **Lição:** sempre que o evento pedido for o **"oposto"** de um evento que você já calculou (ou que é mais fácil de calcular), use `P(evento) = 1 - P(complemento)`.

---

## 7. Probabilidade Condicional e Teorema do Produto

### 7.1. Ideia central

> A **Probabilidade Condicional** `P(A|B)` é a probabilidade de A ocorrer **sabendo que B já ocorreu**. "Saber que B ocorreu" **restringe** o Espaço Amostral — passamos a considerar apenas os resultados onde B é verdade.

#### Exemplo motivador (Seção 6.8)

Lançando dois dados (36 resultados possíveis, cada um com probabilidade 1/36): qual a probabilidade da soma ser 8, **dado que** o primeiro dado deu 3?

- **Sem informação:** soma = 8 tem 5 resultados favoráveis `{(2,6),(3,5),(4,4),(5,3),(6,2)}` → P = 5/36.
- **Sabendo que o 1º dado é 3:** o Espaço Amostral "encolhe" para apenas 6 resultados `{(3,1),(3,2),(3,3),(3,4),(3,5),(3,6)}`, dos quais só `(3,5)` dá soma 8 → P = **1/6**.

> **Explicação simples:** pense em `P(A|B)` como "eu já sei que estou dentro de B; dentro desse novo universo (menor), qual a chance de A?". O B vira o **novo Espaço Amostral**.

### 7.2. Fórmula

```
            P(A ∩ B)
P(A|B) = -----------          (com P(B) > 0)
             P(B)
```

Em espaços equiprováveis, equivalente a:

```
            NCF(A ∩ B)
P(A|B) = ----------------
            NCF(B)
```

(porque o `NTC` do numerador e do denominador se cancelam)

#### Exemplo 6.10 — calculando P(A), P(B), P(A|B) e P(B|A)

```
A = {(x,y) / x+y = 10}     B = {(x,y) / x > y}
```

```
P(A) = NCF(A)/NTC = 3/36  = 1/12
P(B) = NCF(B)/NTC = 15/36 = 5/12

P(A|B) = NCF(A∩B)/NCF(B) = 1/15
P(B|A) = NCF(A∩B)/NCF(A) = 1/3
```

> Note que `P(A|B) ≠ P(B|A)` em geral — são perguntas **diferentes** ("dado que B, qual a chance de A?" vs. "dado que A, qual a chance de B?").

### 7.3. Teorema do Produto

> Isolando `P(A∩B)` na fórmula da Condicional, obtemos a probabilidade de **dois eventos ocorrerem simultaneamente**:

```
P(A ∩ B) = P(A) × P(B|A)
      ou
P(A ∩ B) = P(B) × P(A|B)
```

#### Exemplo 6.11 — verificando com os valores do Exemplo 6.10

```
P(A∩B) = P(A) × P(B|A) = (1/12) × (1/3) = 1/36
P(A∩B) = P(B) × P(A|B) = (5/12) × (1/15) = 1/36     ✔ (os dois caminhos batem)
```

---

## 8. Independência Estatística

### Definição

> Dois eventos A e B são **independentes** se a probabilidade de um **não muda** quando se sabe que o outro ocorreu:

```
P(A) = P(A|B)        e, simetricamente,        P(B) = P(B|A)
```

Combinando com o Teorema do Produto (seção 7.3), se A e B são independentes:

```
P(A ∩ B) = P(A) × P(B)
```

### Explicação simples

"Saber que B aconteceu" **não dá nenhuma informação nova** sobre A. Isso costuma acontecer em **retiradas com reposição** (a peça retirada volta para o conjunto, então o conjunto não muda para a próxima retirada).

> **Atenção — não confundir:**
> - **Mutuamente Excludentes** (seção 2.2): `A ∩ B = ∅` → eventos que **não podem ocorrer juntos**.
> - **Independentes** (seção 8): `P(A∩B) = P(A)×P(B)` → eventos cuja ocorrência **não se influencia**.
>
> São conceitos **diferentes**! Dois eventos com probabilidade positiva não podem ser ao mesmo tempo Mutuamente Excludentes e Independentes (a menos que um deles tenha probabilidade zero).

#### Exemplo 6.12 — retirada COM reposição

Caixa com 10 peças, 4 defeituosas (6 boas). Duas peças são retiradas, **uma após a outra, com reposição**. Qual a probabilidade de ambas serem boas?

Como há reposição, a 1ª retirada **não altera** o espaço amostral da 2ª → os eventos `A = {1ª peça boa}` e `B = {2ª peça boa}` são **independentes**:

```
P(A∩B) = P(A) × P(B) = (6/10) × (6/10) = 36/100 = 0,36
```

> **Dica de prova:** "com reposição" → quase sempre **independência** (multiplica direto as probabilidades). "Sem reposição" → as probabilidades dependem do resultado anterior (precisa de **Condicional**).

---

## 9. Teorema de Bayes

### Ideia central

> Usado quando conhecemos as probabilidades de várias **"causas"** `A₁, A₂, ..., Aₙ` (mutuamente excludentes e coletivamente exaustivas) e também `P(B|Aᵢ)` (a chance de um "efeito" B acontecer, dada cada causa). O Teorema de Bayes **"inverte"** essa informação: dado que o efeito B **já ocorreu**, qual a probabilidade de ter sido causado por `Aᵢ`?

### Construção da fórmula (vale entender, não só memorizar)

```
①  P(Aᵢ|B) = P(Aᵢ ∩ B) / P(B)                    (definição de condicional)

②  P(Aᵢ ∩ B) = P(Aᵢ) × P(B|Aᵢ)                   (teorema do produto)

③  P(B) = P(A₁)×P(B|A₁) + P(A₂)×P(B|A₂) + ... + P(Aₙ)×P(B|Aₙ)
    → "decompõe" B em pedaços, um para cada causa possível Aᵢ
```

Substituindo ② e ③ em ①:

```
                    P(Aᵢ) × P(B|Aᵢ)
P(Aᵢ|B) = ─────────────────────────────────────────────
            P(A₁)×P(B|A₁) + P(A₂)×P(B|A₂) + ... + P(Aₙ)×P(B|Aₙ)
```

### Explicação simples

```
DENOMINADOR = P(B)  → é a probabilidade TOTAL do efeito B ocorrer,
               considerando TODOS os "caminhos" (causas) possíveis.

NUMERADOR   = P(Aᵢ ∩ B)  → é o "pedaço" de B que vem ESPECIFICAMENTE
               da causa Aᵢ que nos interessa.

P(Aᵢ|B) = "qual fração de B veio de Aᵢ?"
```

> **Macete para montar o problema:** 1) Liste as "causas" `Aᵢ` (urnas, máquinas, populações...) e suas probabilidades `P(Aᵢ)`. 2) Para cada causa, ache `P(B|Aᵢ)` (chance do efeito observado, dado aquela causa). 3) Monte o denominador somando `P(Aᵢ)×P(B|Aᵢ)` para todas as causas. 4) O numerador é apenas o termo da causa que te interessa.

#### Exemplo 6.13 — as três urnas

```
        Urna 1     Urna 2     Urna 3
  Pr      3          4          2
  Br      1          3          3
  Vr      5          2          3
        ─────      ─────      ─────
          9          9          8
```

Escolhe-se uma urna ao acaso (cada uma com `P(Uᵢ) = 1/3`) e dela é extraída uma bola, que sai **Branca (Br)**. Qual a probabilidade de essa bola ter vindo da Urna 2?

**Passo 1 — probabilidades de causa:**

```
P(U₁) = P(U₂) = P(U₃) = 1/3
```

**Passo 2 — probabilidade do efeito (Br) dado cada causa:**

```
P(Br|U₁) = 1/9     P(Br|U₂) = 3/9     P(Br|U₃) = 3/8
```

**Passo 3 — aplicar Bayes:**

```
                        P(U₂) × P(Br|U₂)
P(U₂|Br) = ─────────────────────────────────────────────────────────
            P(U₁)×P(Br|U₁) + P(U₂)×P(Br|U₂) + P(U₃)×P(Br|U₃)

                  (1/3)×(3/9)
P(U₂|Br) = ─────────────────────────────────────────────
            (1/3)×(1/9) + (1/3)×(3/9) + (1/3)×(3/8)

P(U₂|Br) = 24/59
```

> Note que `1/3` aparece multiplicando todos os termos (numerador e denominador) — em problemas onde as causas são equiprováveis, esse fator às vezes pode ser **cancelado**, simplificando a conta.

---

## 10. Tabela-Resumo de Fórmulas

| Conceito | Fórmula |
|---|---|
| Complemento | `P(Ā) = 1 - P(A)` |
| União (caso geral) | `P(A∪B) = P(A) + P(B) - P(A∩B)` |
| União (Mutuamente Excludentes) | `P(A∪B) = P(A) + P(B)` |
| Espaço Equiprovável | `P(A) = NCF(A) / NTC` |
| Probabilidade Condicional | `P(A\|B) = P(A∩B) / P(B)` |
| Teorema do Produto | `P(A∩B) = P(A)×P(B\|A) = P(B)×P(A\|B)` |
| Independência | `P(A∩B) = P(A) × P(B)`, ou `P(A\|B) = P(A)` |
| Teorema de Bayes | `P(Aᵢ\|B) = [P(Aᵢ)·P(B\|Aᵢ)] / Σⱼ[P(Aⱼ)·P(B\|Aⱼ)]` |

**Conceitos-chave para memorizar:**

```
A ∩ Ā = ∅            A ∪ Ā = S              (Complemento)
A ∩ B = ∅            → Mutuamente Excludentes
A ∪ B ∪ C ∪ ... = S  → Coletivamente Exaustivos
P(∅) = 0             0 ≤ P(A) ≤ 1           P(S) = 1
A ⊂ B  ⇒  P(B) ≥ P(A)
```

---

## 11. O que mais cai em prova

- **Identificar a relação entre eventos**: complementares, mutuamente excludentes, coletivamente exaustivos (ou nenhuma das três) — calculando `A∩B`, `A∪B`, `Ā`.
- **Aplicar T.2 (complemento) para "pelo menos um"**: quase sempre é mais fácil calcular `1 - P(nenhum)`.
- **Aplicar T.4 (regra da soma)** com a subtração de `P(A∩B)` para não contar a intersecção duas vezes.
- **Espaço Equiprovável com Combinatória**: montar `NCF(A)/NTC` usando Combinações (`C(n,r)`), Arranjos ou Permutações — muito comum com baralhos, dados, urnas e lotes de peças com/sem defeito.
- **Distinguir "com reposição" (independência, multiplica direto) de "sem reposição" (probabilidade condicional, o espaço muda a cada retirada)**.
- **Calcular `P(A|B)` e `P(B|A)`** e perceber que são, em geral, diferentes.
- **Teorema do Produto**: `P(A∩B) = P(A)×P(B|A)`, útil para "quebrar" eventos sequenciais (1ª peça, depois 2ª peça etc.).
- **Verificar Independência**: testar se `P(A∩B) = P(A)×P(B)` ou se `P(A|B) = P(A)`.
- **Teorema de Bayes**: problemas com "urnas", "máquinas/linhas de produção", "diagnósticos médicos" — sempre identificar as causas `Aᵢ` (com `P(Aᵢ)` e `P(B|Aᵢ)`) e montar o denominador como soma de todos os caminhos possíveis para o efeito B.
- **Problemas combinando vários conceitos**: por exemplo, retiradas sucessivas sem reposição (condicional + produto), seguidas de uma pergunta do tipo Bayes ("dado que a 3ª bola foi preta, qual a chance de a 1ª ter sido branca?").
