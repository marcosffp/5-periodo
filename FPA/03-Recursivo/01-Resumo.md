# Análise de Algoritmos Recursivos — Resumo Didático e Profundo

---

## 1. Ideia Principal

Quando temos um algoritmo recursivo, precisamos responder a pergunta: **quantas operações ele realiza?** Para isso, representamos o custo de execução através de uma **equação de recorrência** — uma equação matemática que descreve o comportamento do algoritmo em termos de si mesma, exatamente como o algoritmo se chama recursivamente.

O objetivo final é **resolver essa equação** e encontrar uma fórmula fechada (sem recursividade), que nos diz a complexidade do algoritmo em notação O(n).

---

## 2. Conceitos Importantes

**Recursividade** é quando uma função se chama a si mesma. Para que não caia em loop infinito, toda recursão precisa de dois componentes:

- **Caso base:** a condição que para a recursão (o piso do problema).
- **Caso recursivo:** a chamada que reduz o problema até atingir a base.

**Equação de Recorrência** é a tradução matemática do custo do algoritmo. Ela também tem dois pedaços:

- O custo da **base**: S(0) = algum valor constante.
- O custo do **caso recursivo**: S(n) = custo local + S(problema menor).

---

## 3. Método Geral para Resolver

O processo tem etapas bem definidas:

1. **Identificar base e recursividade** no código.
2. **Definir n** (o parâmetro que controla o tamanho do problema).
3. **Identificar as operações relevantes** (multiplicações, comparações, etc.).
4. **Montar a equação de recorrência** com base e caso recursivo.
5. **Resolver por substituição iterativa** até encontrar um padrão.
6. **Generalizar com uma Fórmula Geral (F.G)** em termos de i.
7. **Aplicar a condição da base** para descobrir o valor de i.
8. **Substituir** e obter a fórmula fechada.

---

## 4. Explicação Detalhada do Exemplo (Passo a Passo)

### O código analisado:

```
int rec1(int n) {
    if (n == 0)        // linha 2
        return 1;      // linha 3
    else
        return n * rec1(n-1);  // linha 5
}
```

Este é o **fatorial** de n. Vamos analisar seu custo.

---

### Passo 1 — Identificar base e recursividade

Olhando o código, vemos claramente:

- **Base:** quando `n == 0`, retorna 1. O algoritmo para aqui.
- **Recursivo:** quando `n > 0`, faz `n * rec1(n-1)`. O problema se reduz de n para n-1 a cada chamada.

---

### Passo 2 — Definir n e a operação relevante

O parâmetro `n` é a medida do tamanho do problema — ele diminui de 1 em 1 a cada chamada.

A operação relevante é a **multiplicação** `n * rec1(n-1)`. Cada chamada faz exatamente 1 multiplicação.

---

### Passo 3 — Montar a equação de recorrência

Chamamos de S(n) o número de operações para entrada n.

**Na base** (n = 0): o algoritmo só faz a comparação e retorna. Nenhuma multiplicação. Portanto:

> S(0) = 1

**No caso recursivo** (n > 0): faz 1 multiplicação, mais o custo da chamada recursiva S(n-1). Portanto:

> S(n) = 1 + S(n-1)

Equação completa:

$$\begin{cases} S(0) = 1 \\ S(n) = 1 + S(n-1) \end{cases}$$

---

### Passo 4 — Resolver por substituição iterativa

Agora vamos **expandir** a equação, substituindo S(n-1), depois S(n-2), etc. A ideia é enxergar um padrão.

**Partindo de S(n):**

> S(n) = 1 + S(n-1)

Mas S(n-1) também segue a mesma regra: S(n-1) = 1 + S(n-2). Substituindo:

> S(n) = 1 + [1 + S(n-2)] = **S(n-2) + 2**

Agora S(n-2) = 1 + S(n-3). Substituindo:

> S(n) = [1 + S(n-3)] + 2 = **S(n-3) + 3**

Perceba o padrão emergindo. A cada substituição, o argumento de S diminui em 1 e a constante aumenta em 1. Isso nos dá:

| Passo | Expressão |
|-------|-----------|
| Inicial | S(n-1) + 1 |
| 1ª substituição | S(n-2) + 2 |
| 2ª substituição | S(n-3) + 3 |
| i-ésima substituição | **S(n-i) + i** |

---

### Passo 5 — Escrever a Fórmula Geral (F.G)

> **F.G = S(n - i) + i**

Esta fórmula descreve o estado da expansão após i substituições. O i é uma variável livre — ainda não tem valor fixo.

---

### Passo 6 — Aplicar a condição da base

Queremos que a expansão pare quando chegamos à **base**, ou seja, quando o argumento de S for 0:

> S(n - i) = S(0) → n - i = 0 → **i = n**

Sabemos agora que a expansão para quando i = n.

---

### Passo 7 — Substituir i = n na F.G

> S(n) = S(n - n) + n = S(0) + n

Como S(0) = 1:

> **S(n) = 1 + n**

---

### Conclusão

O algoritmo `rec1` realiza **n + 1 operações**. Em notação assintótica:

> **S(n) = O(n)** — complexidade linear.

Faz sentido intuitivamente: para calcular o fatorial de n, o algoritmo se chama n vezes, fazendo 1 multiplicação a cada vez.

---

## 5. Como Aplicar o Raciocínio em Outros Exercícios

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

- Base: S(1) = 1
- Recursivo: S(n) = 1 + S(n/2) — aqui o problema é **dividido ao meio**!

Expandindo:

> S(n) = 1 + S(n/2)
> = 1 + 1 + S(n/4) = S(n/4) + 2
> = S(n/8) + 3
> = **S(n/2^i) + i** ← F.G

Na base: n/2^i = 1 → 2^i = n → **i = log₂(n)**

Substituindo: S(n) = S(1) + log₂(n) = 1 + log₂(n)

> **S(n) = O(log n)**

E é exatamente **assim que surge o log₂(n)** do mergesort, quicksort e heapsort — sempre que um algoritmo divide o problema ao meio a cada chamada, a profundidade da recursão é logarítmica.

---

## 6. Dicas para Resolver sem Decorar

**Dica 1 — O padrão depende de como n decresce:**
- Se n diminui de 1 em 1 → a expansão gera S(n-i) + algo → i vai até n → resultado geralmente O(n).
- Se n é dividido por 2 → a expansão gera S(n/2^i) + algo → i vai até log₂(n) → resultado geralmente O(log n).
- Se ambos acontecem junto (divide e opera) → geralmente O(n log n).

**Dica 2 — A Fórmula Geral é sempre o padrão com i:**
Não tente adivinhar. Expanda 3 vezes, observe o que muda, escreva com i. Se a cada passo o argumento perde 1 e a constante ganha 1, a F.G tem essa forma simétrica.

**Dica 3 — A condição da base te dá o valor de i:**
Sempre iguale o argumento de S(?) ao valor da base para descobrir quando parar.

**Dica 4 — Verifique seu resultado com n pequeno:**
Se encontrou S(n) = n + 1, teste com n = 3: a função deveria fazer 4 operações. Conte no código — se bater, está certo.

**Dica 5 — Identifique quantas chamadas recursivas existem:**
Uma chamada recursiva → árvore linear → O(n) ou O(log n).
Duas chamadas recursivas → árvore binária → geralmente O(2^n) ou O(n log n).