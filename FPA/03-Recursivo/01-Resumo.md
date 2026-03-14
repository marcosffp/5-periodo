### Análise de Algoritmos Recursivos

---

#### 1. Ideia Principal

Quando temos um algoritmo recursivo, precisamos responder: **quantas operações ele realiza?** Para isso, representamos o custo de execução através de uma **equação de recorrência** — uma equação matemática que descreve o comportamento do algoritmo em termos de si mesma, exatamente como o algoritmo se chama recursivamente.

O objetivo final é **resolver essa equação** e encontrar uma fórmula fechada (sem recursividade), que nos diz a complexidade em notação $O(n)$.

---

#### 2. Conceitos Importantes

**Recursividade** é quando uma função se chama a si mesma. Para que não caia em loop infinito, toda recursão precisa de dois componentes:

- **Caso base:** a condição que para a recursão.
- **Caso recursivo:** a chamada que reduz o problema até atingir a base.

**Equação de Recorrência** é a tradução matemática do custo do algoritmo. Ela também tem dois pedaços:

- O custo da **base:** $S(0) = \text{algum valor constante}$.
- O custo do **caso recursivo:** $S(n) = \text{custo local} + S(\text{problema menor})$.

---

#### 3. Método Geral para Resolver

1. Identificar base e recursividade no código.
2. Definir $n$ — o parâmetro que controla o tamanho do problema.
3. Identificar a operação relevante (multiplicação, comparação, etc.).
4. Montar a equação de recorrência com base e caso recursivo.
5. Resolver por **substituição iterativa** até enxergar um padrão.
6. Generalizar com uma **Fórmula Geral (F.G)** em termos de $i$.
7. Aplicar a **condição da base** para descobrir o valor de $i$.
8. Substituir e obter a fórmula fechada.

---

#### 4. Explicação Detalhada do Exemplo (Passo a Passo)

##### O código analisado

```
int rec1(int n) {
    if (n == 0)
        return 1;
    else
        return n * rec1(n-1);
}
```

Este é o **fatorial** de $n$. Vamos analisar seu custo contando o número de multiplicações realizadas.

---

##### Passo 1 — Identificar base e recursividade

Olhando o código:

- **Base:** quando `n == 0`, retorna 1 imediatamente. Nenhuma multiplicação é feita.
- **Recursivo:** quando `n > 0`, executa `n * rec1(n-1)`. O problema diminui de $n$ para $n-1$ a cada chamada.

---

##### Passo 2 — Definir $n$ e a operação relevante

O parâmetro $n$ é a medida do tamanho do problema — ele diminui de 1 em 1 a cada chamada recursiva.

A operação relevante é a **multiplicação** `n * rec1(n-1)`. Cada chamada realiza exatamente **1 multiplicação**.

---

##### Passo 3 — Montar a equação de recorrência

Chamamos de $S(n)$ o número de multiplicações para entrada $n$.

**Na base** ($n = 0$): nenhuma multiplicação é feita, apenas retorna 1:

$$S(0) = 1$$

**No caso recursivo** ($n > 0$): faz 1 multiplicação, mais o custo da chamada com $n-1$:

$$S(n) = 1 + S(n-1)$$

Equação completa:

$$\begin{cases} S(0) = 1 \\ S(n) = 1 + S(n-1) \end{cases}$$

---

##### Passo 4 — Resolver por substituição iterativa

A ideia central aqui é simples: **a equação fala de si mesma**, então podemos abri-la repetidamente, substituindo $S(n-1)$ pela própria definição, depois $S(n-2)$, e assim por diante. A cada substituição enxergamos um pouco mais da estrutura.

Vamos com calma, passo a passo.

**Começamos com:**

$$S(n) = 1 + S(n-1)$$

$S(n-1)$ ainda é desconhecido, mas sabemos que ele segue a mesma regra. Ou seja:

$$S(n-1) = 1 + S(n-2)$$

Substituímos isso dentro de $S(n)$:

$$S(n) = 1 + \big[1 + S(n-2)\big] = 2 + S(n-2)$$

Agora $S(n-2)$ ainda é desconhecido. Pela mesma regra:

$$S(n-2) = 1 + S(n-3)$$

Substituímos:

$$S(n) = 2 + \big[1 + S(n-3)\big] = 3 + S(n-3)$$

Observe o que está acontecendo a cada passo:

| Substituição | Expressão obtida |
|---|---|
| Nenhuma (início) | $S(n-1) + 1$ |
| $1^a$ | $S(n-2) + 2$ |
| $2^a$ | $S(n-3) + 3$ |
| $3^a$ | $S(n-4) + 4$ |

O padrão é claro: após $i$ substituições, o argumento de $S$ perdeu $i$ e a constante ganhou $i$.

---

##### Passo 5 — Escrever a Fórmula Geral (F.G)

Generalizamos o padrão observado para um $i$ qualquer:

$$\text{F.G} = S(n - i) + i$$

Esta fórmula descreve onde estamos após $i$ substituições. O $i$ ainda é uma variável livre — ainda não sabemos quando parar.

---

##### Passo 6 — Aplicar a condição da base

Queremos que a expansão pare exatamente quando chegarmos à **base**, isto é, quando o argumento de $S$ for $0$:

$$n - i = 0 \implies i = n$$

Quando $i = n$, o $S(n - i)$ vira $S(0)$, que já conhecemos: $S(0) = 1$.

---

##### Passo 7 — Substituir $i = n$ na F.G

$$S(n) = S(n - n) + n = S(0) + n = 1 + n$$

---

##### Conclusão

$$S(n) = n + 1 \implies O(n)$$

Faz sentido intuitivamente: para calcular o fatorial de $n$, o algoritmo se chama $n$ vezes, fazendo 1 multiplicação a cada vez. A constante $+1$ vem da base.

**Verificação rápida:** para $n = 3$, o algoritmo faz `3 * rec1(2)` → `2 * rec1(1)` → `1 * rec1(0)` → retorna. São exatamente 3 multiplicações. A fórmula dá $S(3) = 3 + 1 = 4$, contando a base. $\checkmark$

---

#### 5. Como Aplicar o Raciocínio em Outros Exercícios

Suponha que você encontra este código:

```
int rec2(int n) {
    if (n == 1)
        return 0;
    else
        return 1 + rec2(n/2);
}
```

Seguindo o método:

- Base: $S(1) = 1$
- Recursivo: $S(n) = 1 + S(n/2)$ — aqui o problema é **dividido ao meio**.

Expandindo:

$$S(n) = 1 + S(n/2)$$

$$= 1 + 1 + S(n/4) = 2 + S(n/4)$$

$$= 3 + S(n/8)$$

$$\text{F.G} = S(n/2^i) + i$$

Na base: $n/2^i = 1 \implies 2^i = n \implies i = \log_2 n$

Substituindo:

$$S(n) = S(1) + \log_2 n = 1 + \log_2 n$$

$$S(n) = O(\log n)$$

É exatamente **assim que surge o $\log_2 n$** no Merge Sort, Quick Sort e Heap Sort — sempre que um algoritmo divide o problema ao meio a cada chamada, a profundidade da recursão é logarítmica.

---

#### 6. Dicas para Resolver sem Decorar

**Dica 1 — O padrão depende de como $n$ decresce:**

- $n$ diminui de 1 em 1 → expansão gera $S(n-i) + i$ → $i$ vai até $n$ → resultado $O(n)$.
- $n$ é dividido por 2 → expansão gera $S(n/2^i) + i$ → $i$ vai até $\log_2 n$ → resultado $O(\log n)$.
- Ambos acontecem juntos → geralmente $O(n \log n)$.

**Dica 2 — A Fórmula Geral é sempre o padrão com $i$:**

Não tente adivinhar. Expanda 3 vezes, observe o que muda, escreva com $i$.

**Dica 3 — A condição da base te dá o valor de $i$:**

Sempre iguale o argumento de $S(?)$ ao valor da base para descobrir quando parar.

**Dica 4 — Verifique seu resultado com $n$ pequeno:**

Se encontrou $S(n) = n + 1$, teste com $n = 3$: conte as operações no código e confirme.

**Dica 5 — Identifique quantas chamadas recursivas existem:**

- Uma chamada recursiva → árvore linear → $O(n)$ ou $O(\log n)$.
- Duas chamadas recursivas → árvore binária → geralmente $O(2^n)$ ou $O(n \log n)$.
