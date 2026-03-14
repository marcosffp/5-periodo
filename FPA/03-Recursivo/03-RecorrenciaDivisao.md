# Equações de Recorrência com Divisão — Método Formal Passo a Passo

---

## 1. Ideia Principal

Nos resumos anteriores, resolvíamos recorrências expandindo manualmente, enxergando um padrão e escrevendo a Fórmula Geral (F.G) com $i$ ou $k$. Esse método funciona bem para casos simples como divisão por 2 com padrão óbvio. Mas ele começa a falhar quando a recorrência fica mais complexa — por exemplo, quando o problema se divide por 3 em vez de 2, ou quando o somatório que surge não é imediato de fechar.

O método formal apresentado aqui resolve exatamente esse problema. Ele é uma **receita de cinco passos (A, B, C, D, E)** que funciona para qualquer recorrência de divisão, independente da base. A grande novidade é a **substituição de variável**: trocamos $n$ por $3^i$ (ou $2^i$, dependendo do problema) para transformar a recorrência em um somatório que sabemos resolver com a fórmula da série geométrica.

---

## 2. Quando usar este método vs. o anterior?

Esta é a questão mais importante antes de começar.

**O método anterior** (expandir, enxergar padrão, F.G., condição da base) funciona bem quando o problema se divide por 2 e o padrão da constante acumulada é óbvio ($k$, $2^k - 1$, $kn$, etc.), com custo local simples ($+1$, $+n$, $+2$).

**O método formal (A, B, C, D, E)** é necessário quando o problema se divide por 3, 4 ou qualquer outra base — o padrão não é imediato — ou quando o somatório resultante precisa ser fechado com a fórmula da série geométrica.

Na prática: **se dividir por 2 e o padrão for claro, use o método anterior. Se dividir por qualquer outra base, ou se o padrão não for óbvio, use o método formal.**

---

## 3. Conceito Necessário: Série Geométrica

A ferramenta matemática nova que este método usa no passo final é a série geométrica:

$$\sum_{j=0}^{k} a^j = \frac{a^{k+1} - 1}{a - 1} \quad (a \neq 1)$$

Isso nos permite fechar o somatório que surge na expansão. Sem essa fórmula, ficaria impossível obter a fórmula fechada quando a base é 3 ou maior.

---

## 4. Os Cinco Passos do Método Formal

| Passo | O que fazer |
|---|---|
| A | Expandir a recorrência alguns níveis para ver as substituições |
| B | Identificar o padrão geral em termos de $i$ |
| C | Determinar o limite — quando a expansão para (encontrar $i$) |
| D | Substituir $n = \text{base}^i$ para reescrever tudo em função de $i$ |
| E | Resolver o somatório usando a fórmula da série geométrica |

---

## 5. Exemplo Principal — divisão por 3

```
void pesquisa(n) {
    if (n <= 1)
        inspecione elemento e termine        // base
    else {
        para cada um dos n elementos, inspecione;  // custo: n
        pesquisa(n/3);                             // divide por 3
    }
}
```

---

### Montando a equação de recorrência

**Base:** quando $n \leq 1$, inspeciona 1 elemento:

$$T(1) = 1$$

**Caso recursivo:** inspeciona $n$ elementos, depois chama `pesquisa(n/3)`:

$$T(n) = T(n/3) + n$$

Equação completa:

$$\begin{cases} T(1) = 1 \\ T(n) = T(n/3) + n \end{cases}$$

---

## Passo A — Expandir a recorrência

Partimos de $T(n)$ e substituímos $T(n/3)$ pela própria regra, depois $T(n/9)$, e assim por diante:

$$T(n) = T(n/3) + n$$
$$T(n/3) = T(n/9) + n/3$$
$$T(n/9) = T(n/27) + n/9$$
$$T(n/27) = T(n/81) + n/27$$
$$\vdots$$
$$T(1) = 1$$

O argumento de $T$ vai encolhendo: $n \to n/3 \to n/9 \to n/27\dots$ O termo adicional também encolhe: $n \to n/3 \to n/9\dots$

---

## Passo B — Identificar o padrão

Cada linha tem a forma:

$$T(n/3^i) = T(n/3^{i+1}) + n/3^i$$

Após $i$ substituições, o argumento é $n/3^i$ e o termo adicionado é $n/3^i$.

---

## Passo C — Determinar o limite

Paramos quando o argumento de $T$ iguala o caso base $T(1)$:

$$\frac{n}{3^i} = 1 \implies n = 3^i \implies i = \log_3 n$$

---

## Passo D — Substituir $n = 3^i$

Este é o passo central. Em vez de trabalhar com frações como $n/3^i$, substituímos $n = 3^i$ diretamente em todas as linhas. Isso transforma cada fração numa potência inteira de 3, tornando a álgebra limpa:

$$T(3^i) = T(3^{i-1}) + 3^i$$
$$T(3^{i-1}) = T(3^{i-2}) + 3^{i-1}$$
$$T(3^{i-2}) = T(3^{i-3}) + 3^{i-2}$$
$$\vdots$$
$$T(3^1) = T(3^0) + 3^1$$
$$T(3^0) = 1$$

A última linha vem diretamente do caso base: $3^0 = 1$, então $T(3^0) = T(1) = 1$.

Agora substituímos de **baixo para cima** — começamos pelo que já sabemos e subimos uma linha de cada vez. Usando $i = 3$ como exemplo concreto:

$$T(3^0) = 1$$
$$T(3^1) = T(3^0) + 3 = 1 + 3 = 4$$
$$T(3^2) = T(3^1) + 9 = 4 + 9 = 13$$
$$T(3^3) = T(3^2) + 27 = 13 + 27 = 40$$

Perceba o que sobrou no total: $27 + 9 + 3 + 1 = 3^3 + 3^2 + 3^1 + 3^0$. Os $T$ intermediários foram desaparecendo um a um conforme subíamos — cada um foi substituído pelo seu valor até não restar nenhum. No caso geral com $i$ níveis:

$$T(3^i) = 3^i + 3^{i-1} + \dots + 3^1 + 3^0 = \sum_{j=0}^{i} 3^j$$

---

## Passo E — Resolver o somatório

Temos uma soma de progressão geométrica com razão $a = 3$. Aplicamos a fórmula:

$$\sum_{j=0}^{i} 3^j = \frac{3^{i+1} - 1}{3 - 1} = \frac{3^{i+1} - 1}{2}$$

Substituímos $i = \log_3 n$, usando que $3^{\log_3 n} = n$:

$$\frac{3^{\log_3 n + 1} - 1}{2} = \frac{3 \cdot 3^{\log_3 n} - 1}{2} = \frac{3n - 1}{2}$$

$$\boxed{T(n) = \frac{3n-1}{2} \implies O(n)}$$

**Intuição:** o algoritmo inspeciona $n$ elementos, depois $n/3$, depois $n/9\dots$ A soma $n + n/3 + n/9 + \dots$ é uma série geométrica que converge para $\approx 3n/2$, confirmando $O(n)$.

---

## 6. Exercícios Resolvidos

---

## Exercício a)

$$\begin{cases} T(1) = 1 \\ T(n) = T(n/2) + 1 \end{cases}$$

Este caso poderia ser resolvido pelo método anterior, mas vamos aplicar o método formal para treinar.

**Passo A — Expandir:**

$$T(n) = T(n/2) + 1$$
$$T(n/2) = T(n/4) + 1$$
$$T(n/4) = T(n/8) + 1$$

**Passo B — Padrão:**

$$T(n/2^i) = T(n/2^{i+1}) + 1$$

**Passo C — Limite:**

$$n/2^i = 1 \implies i = \log_2 n$$

**Passo D — Substituir $n = 2^i$:**

$$T(2^i) = T(2^{i-1}) + 1$$
$$T(2^{i-1}) = T(2^{i-2}) + 1$$
$$\vdots$$
$$T(2^0) = 1$$

Substituindo de baixo para cima, cada $T$ é substituído pelo seu valor até não restar nenhum. O que sobra é uma soma de $i$ parcelas de $+1$ mais a base:

$$T(2^i) = \underbrace{1 + 1 + \dots + 1}_{i \text{ vezes}} + 1 = i + 1$$

**Passo E — Substituir $i = \log_2 n$:**

$$T(n) = \log_2 n + 1 \implies O(\log n)$$

**Intuição:** não há trabalho extra por nível ($+1$ é constante). O algoritmo só faz $\log_2 n$ chamadas no total.

---

## Exercício b)

$$\begin{cases} T(1) = 0 \\ T(n) = 2T(n/2) + n \end{cases}$$

Este é o padrão do Merge Sort — duas chamadas recursivas com trabalho linear por nível.

**Passo A — Expandir:**

$$T(n) = 2T(n/2) + n$$
$$T(n/2) = 2T(n/4) + n/2$$
$$T(n/4) = 2T(n/8) + n/4$$

**Passo C — Limite:**

$$n/2^i = 1 \implies i = \log_2 n$$

**Passo D — Substituir $n = 2^i$:**

$$T(2^i) = 2T(2^{i-1}) + 2^i$$
$$T(2^{i-1}) = 2T(2^{i-2}) + 2^{i-1}$$
$$\vdots$$
$$T(2^0) = 0$$

Substituindo de baixo para cima, o coeficiente de $T$ dobra a cada nível e o termo adicional é sempre $2^i$. O trabalho de cada nível é $2^i$ e há $i$ níveis antes da base, então:

$$T(2^i) = i \cdot 2^i$$

**Passo E — Substituir $i = \log_2 n$:**

$$2^{\log_2 n} = n \implies T(n) = \log_2 n \cdot n \implies O(n \log n)$$

**Intuição:** este é exatamente o padrão do Merge Sort — duas chamadas com $n/2$ elementos, trabalho linear $n$ por nível, e $\log_2 n$ níveis no total.

---

## 7. Dicas para Resolver sem Precisar Decorar

**Dica 1 — Os cinco passos são uma receita universal.** A, B, C, D, E. Não importa se divide por 2, por 3 ou por 4 — o processo é sempre o mesmo.

**Dica 2 — A substituição $n = \text{base}^i$ é o coração do método.** Se divide por 3, faça $n = 3^i$. Se divide por 2, faça $n = 2^i$. Isso elimina as frações e transforma a recorrência em um somatório limpo.

**Dica 3 — Substitua de baixo para cima.** Comece pela linha que já conhece ($T(1)$ ou $T(3^0)$) e suba uma linha de cada vez, substituindo o $T$ do lado direito pelo valor que acabou de calcular. Os $T$ intermediários vão desaparecendo até sobrar apenas a soma dos termos adicionais.

**Dica 4 — A série geométrica é a ferramenta final.** Sempre que aparecer uma soma do tipo $a^0 + a^1 + \dots + a^k$, use $\frac{a^{k+1}-1}{a-1}$. Depois substitua $k = \log_{\text{base}}(n)$ para voltar em termos de $n$.

**Dica 5 — Verifique com a intuição:**

| Formato da recorrência | Resultado |
|---|---|
| $T(n/b) + 1$ | $O(\log n)$ |
| $T(n/b) + n$ | $O(n)$ |
| $2T(n/2) + 1$ | $O(n)$ |
| $2T(n/2) + n$ | $O(n \log n)$ |
