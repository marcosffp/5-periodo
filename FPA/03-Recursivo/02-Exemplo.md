# Análise de Algoritmos Recursivos com Dois Parâmetros

---

#### 1. Ideia Principal

Este conteúdo apresenta um novo desafio: **o que acontece quando um algoritmo recursivo tem dois parâmetros que controlam seu comportamento?** A recursividade depende de `i`, mas o laço interno depende de `n` (o tamanho do vetor). São duas dimensões diferentes e precisamos saber como tratá-las na equação de recorrência. No final, há um exercício com **duas chamadas recursivas simultâneas**, o que muda completamente o raciocínio.

---

#### 2. Conceitos Importantes

**Dois parâmetros, dois papéis distintos:**

- `i` é o parâmetro que **controla a recursão** — ele diminui a cada chamada.
- `n` é o **tamanho do vetor** (`vet.length`) — ele é constante durante toda a execução.

Quando montamos a equação de recorrência, a variável livre é `i`, e `n` aparece como uma constante que se acumula a cada nível de recursão.

**Duas chamadas recursivas:** quando uma função chama a si mesma duas vezes por nível, a árvore de recursão se bifurca. Isso é característico de algoritmos de divisão e conquista como o Merge Sort.

---

#### 3. Método Geral

1. Identificar base e caso recursivo.
2. Definir qual parâmetro controla a recursão e qual é constante.
3. Identificar as operações relevantes em cada chamada.
4. Montar a equação de recorrência.
5. Expandir por substituição iterativa, observando o padrão.
6. Escrever a Fórmula Geral (F.G) com a variável de expansão $k$.
7. Aplicar a condição da base para encontrar $k$.
8. Substituir e obter a fórmula fechada.

---

#### 4. Explicação Detalhada dos Exemplos

---

##### Exemplo Principal — `rec2`

```
int rec2(int[] vet, int i) {
    if (i == 0)
        return vet[0];
    else {
        for (int j = 0; j < vet.length; j++) {
            vet[j] += vet[i];
        }
        return rec2(vet, i-1);
    }
}
```

---

###### Passo 1 — Identificar base e recursividade

- **Base:** quando `i == 0`, apenas retorna `vet[0]`. Nenhuma operação relevante.
- **Recursivo:** quando `i > 0`, executa um laço `for` completo sobre o vetor, depois chama `rec2(vet, i-1)`.

---

###### Passo 2 — Identificar quem é $n$ e quem é $i$

O código tem dois parâmetros: `vet` e `i`. A recursão diminui `i` de 1 em 1. O vetor não muda de tamanho — `vet.length` é constante. Chamamos esse tamanho de $n$.

Isso importa porque quando montarmos a equação, a parte constante de cada chamada será $n$ (o custo do laço), não $1$ como no exemplo anterior.

---

###### Passo 3 — Montar a equação de recorrência

**Na base** ($i = 0$): nenhuma operação, apenas retorna:

$$S(0) = 1$$

**No caso recursivo** ($i > 0$): o laço percorre o vetor inteiro → $n$ operações de soma. Depois chama recursivamente com $i-1$:

$$S(i) = n + S(i-1)$$

Equação completa:

$$\begin{cases} S(0) = 1 \\ S(i) = S(i-1) + n \end{cases}$$

A diferença crucial em relação ao exemplo anterior: onde antes cada chamada custava $+1$, agora cada chamada custa $+n$. O $n$ é constante, mas aparece somado a cada nível.

---

###### Passo 4 — Expandir por substituição iterativa

**Começamos com:**

$$S(i) = S(i-1) + n$$

$S(i-1)$ segue a mesma regra: $S(i-1) = S(i-2) + n$. Substituindo:

$$S(i) = \big[S(i-2) + n\big] + n = S(i-2) + 2n$$

$S(i-2) = S(i-3) + n$. Substituindo:

$$S(i) = \big[S(i-3) + n\big] + 2n = S(i-3) + 3n$$

O padrão após cada substituição:

| Substituição | Expressão obtida |
|---|---|
| Nenhuma | $S(i-1) + n$ |
| $1^a$ | $S(i-2) + 2n$ |
| $2^a$ | $S(i-3) + 3n$ |
| $k$-ésima | $S(i-k) + kn$ |

A lógica é a mesma dos exemplos anteriores: o argumento de $S$ perde $k$ e a constante acumula $k$ vezes. A única diferença é que cada nível custa $n$ e não $1$, então acumulamos $kn$ e não $k$.

---

###### Passo 5 — Fórmula Geral (F.G)

$$\text{F.G} = S(i - k) + kn$$

---

###### Passo 6 — Aplicar a condição da base

Queremos parar quando o argumento de $S$ for $0$:

$$i - k = 0 \implies k = i$$

---

###### Passo 7 — Substituir $k = i$ na F.G

$$S(i) = S(i - i) + i \cdot n = S(0) + in = 1 + in$$

$$\boxed{S(i) = 1 + in \implies O(n^2)}$$

quando $i$ e $n$ são da mesma ordem.

**Intuição:** a função executa $i$ chamadas recursivas e, em cada uma, percorre o vetor inteiro de tamanho $n$. É um laço dentro de um laço — custo quadrático.

---

##### Exercício 1 — `rec3` (duas chamadas recursivas)

```
int rec3(int[] vet, int a, int b) {
    if (a == b)
        return vet[a];
    else {
        int c = (a + b) / 2;     // operação 1: divisão
        int d = rec3(vet, a, c);
        int e = rec3(vet, c, b);
        return d + e;            // operação 2: soma
    }
}
```

---

###### Passo 1 — Entender o que o código faz

A função recebe um vetor e dois índices `a` e `b` que delimitam um intervalo. Ela para quando `a == b` — quando o intervalo tem apenas um elemento. Caso contrário, encontra o meio do intervalo e chama a si mesma **duas vezes**: uma para a metade esquerda e outra para a metade direita.

---

###### Passo 2 — Definir $n$

Aqui não existe um parâmetro chamado $n$ no código. Precisamos identificar **o que está encolhendo** a cada chamada recursiva.

A função opera sobre o intervalo $[a, b]$. O que diminui a cada chamada é a **quantidade de itens nesse intervalo**. Definimos então:

$$n = b - a$$

Ou seja, $n$ é o número de posições entre `a` e `b`. É isso que mede o tamanho do problema.

**Verificando com um exemplo.** Suponha a chamada inicial `rec3(vet, 0, 8)`. Então $n = 8$.

Com `c = (0 + 8) / 2 = 4`, as duas chamadas recursivas serão:

```
rec3(vet, 0, 4)  →  n = 4 - 0 = 4  =  n/2
rec3(vet, 4, 8)  →  n = 8 - 4 = 4  =  n/2
```

Cada subproblema tem exatamente metade dos itens do intervalo original. Portanto, a cada chamada o problema é dividido ao meio: tamanho $n/2$.

---

###### Passo 3 — Montar a equação de recorrência

**Na base** ($a = b$, ou seja, $n = 0$): apenas retorna `vet[a]`, nenhuma operação relevante:

$$S(0) = 1$$

**No caso recursivo**, temos duas operações locais:

- `c = (a + b) / 2` — **1 operação** (a divisão)
- `return d + e` — **1 operação** (a soma)

Total local: **2 operações**. Mais duas chamadas recursivas, cada uma sobre um intervalo de tamanho $n/2$:

$$S(n) = \underbrace{2 \cdot S(n/2)}_{\text{duas chamadas recursivas}} + \underbrace{2}_{\text{divisão + soma}}$$

Equação completa:

$$\begin{cases} S(0) = 1 \\ S(n) = 2 \cdot S(n/2) + 2 \end{cases}$$

---

###### Passo 4 — Expandir por substituição iterativa

**Começamos com:**

$$S(n) = 2 \cdot S(n/2) + 2$$

$S(n/2)$ segue a mesma regra: $S(n/2) = 2 \cdot S(n/4) + 2$. Substituindo:

$$S(n) = 2 \cdot \big[2 \cdot S(n/4) + 2\big] + 2 = 4 \cdot S(n/4) + 4 + 2 = 4 \cdot S(n/4) + 6$$

$S(n/4) = 2 \cdot S(n/8) + 2$. Substituindo:

$$S(n) = 4 \cdot \big[2 \cdot S(n/8) + 2\big] + 6 = 8 \cdot S(n/8) + 8 + 6 = 8 \cdot S(n/8) + 14$$

Organizando o padrão:

| Substituição | Expressão obtida | Coeficiente de $S$ | Constante acumulada |
|---|---|---|---|
| Nenhuma | $2 \cdot S(n/2) + 2$ | $2$ | $2$ |
| $1^a$ | $4 \cdot S(n/4) + 6$ | $4$ | $6$ |
| $2^a$ | $8 \cdot S(n/8) + 14$ | $8$ | $14$ |
| $k$-ésima | $2^k \cdot S(n/2^k) + (2^{k+1}-2)$ | $2^k$ | $2^{k+1}-2$ |

**De onde vem $2^{k+1} - 2$?** Observe a constante acumulada: $2, 6, 14\dots$ Somando 2 a cada um dá $4, 8, 16\dots$ — potências de 2. A constante é sempre $2^{k+1} - 2$.

Verificando: $k=1 \Rightarrow 2^2 - 2 = 2$ ✓, $k=2 \Rightarrow 2^3 - 2 = 6$ ✓, $k=3 \Rightarrow 2^4 - 2 = 14$ ✓

**Por que $2^{k+1} - 2$?** A cada substituição, o coeficiente $2^k$ se distribui sobre o $+2$ que vem de dentro. A soma acumulada é:

$$2 + 2\cdot2 + 4\cdot2 + \dots + 2^{k-1}\cdot2 = 2(1 + 2 + 4 + \dots + 2^{k-1}) = 2(2^k - 1) = 2^{k+1} - 2$$

---

###### Passo 5 — Fórmula Geral (F.G)

$$\text{F.G} = 2^k \cdot S(n/2^k) + (2^{k+1} - 2)$$

---

###### Passo 6 — Aplicar a condição da base

Dividindo $n$ por $2$ repetidamente nunca chegamos a zero — chegamos a $1$, o menor intervalo indivisível. Paramos quando $n/2^k = 1$:

$$\frac{n}{2^k} = 1 \implies 2^k = n \implies k = \log_2 n$$

---

###### Passo 7 — Substituir $k = \log_2 n$ na F.G

**Primeiro termo:** $2^k \cdot S(n/2^k)$

$$2^{\log_2 n} \cdot S(1) = n \cdot 1 = n$$

**Segundo termo:** $2^{k+1} - 2$

$$2^{\log_2 n + 1} - 2 = 2^{\log_2 n} \cdot 2^1 - 2 = n \cdot 2 - 2 = 2n - 2$$

**Juntando:**

$$S(n) = n + 2n - 2 = 3n - 2$$

$$\boxed{S(n) = 3n - 2 \implies O(n)}$$

**Intuição:** embora a função se chame duas vezes por nível, ela divide o intervalo ao meio a cada vez. A árvore de recursão tem $\log_2 n$ níveis e em cada nível o trabalho total somado de todos os nós é proporcional a $n$. O resultado é linear — equivalente a percorrer o vetor uma única vez.

---

#### 5. Padrões de Recorrência mais Comuns

| Recorrência | O que indica | Resultado |
|---|---|---|
| $S(n) = S(n-1) + 1$ | reduz 1 a 1, custo constante por nível | $O(n)$ |
| $S(n) = S(n-1) + n$ | reduz 1 a 1, custo linear por nível | $O(n^2)$ |
| $S(n) = S(n/2) + 1$ | divide ao meio, custo constante | $O(\log n)$ |
| $S(n) = 2 \cdot S(n/2) + 1$ | duas chamadas, divide ao meio | $O(n)$ |
| $S(n) = 2 \cdot S(n/2) + n$ | duas chamadas + trabalho linear | $O(n \log n)$ |

---

#### 6. Dicas para Resolver sem Precisar Decorar

**Dica 1 — Separe quem é a variável da recursão e quem é constante.** No `rec2`, `i` diminui e `n` é fixo. Na equação, `n` entra como constante que se multiplica pelo número de passos.

**Dica 2 — Com duas chamadas recursivas, o coeficiente de $S$ dobra a cada substituição.** Se $S(n) = 2 \cdot S(n/2) + c$, após $k$ substituições você terá $2^k \cdot S(n/2^k) + \text{algo}$.

**Dica 3 — Para encontrar o padrão da constante acumulada, calcule os três primeiros termos à mão.** Se a constante for $2, 6, 14\dots$ é $2^{k+1} - 2$. Se for $1, 3, 7\dots$ é $2^k - 1$. Se for $1, 2, 3\dots$ é $k$. Três termos bastam para enxergar o padrão.

**Dica 4 — Para parar a expansão, iguale o argumento de $S$ ao valor da base.** Se a base é $S(0)$: iguale a $0$. Se a base é $S(1)$: iguale a $1$. Isso resolve $k$ em termos de $n$.

**Dica 5 — A intuição sempre confirma a matemática.** Visita cada elemento uma vez → $O(n)$. Faz algo para cada par → $O(n^2)$. Divide ao meio sem trabalho extra → $O(\log n)$. Divide ao meio com trabalho linear → $O(n \log n)$.
