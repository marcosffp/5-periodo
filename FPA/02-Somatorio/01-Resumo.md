# Análise do Selection Sort (alg4) — O Caso do Laço com Número Variável de Iterações

---

## 1. Ideia Principal

Este conteúdo apresenta um problema novo e mais sofisticado que os anteriores: um algoritmo com **dois laços aninhados onde o laço interno não executa sempre o mesmo número de vezes**. Isso exige uma ferramenta matemática nova — o **somatório** — para calcular o custo total corretamente. Sem entender somatórios, é impossível analisar esse tipo de algoritmo com precisão.

---

## 2. Conceitos Importantes

### O que é um somatório e por que ele aparece aqui?

Nos exemplos anteriores, o laço interno sempre executava **n vezes**, então o custo era simplesmente `n × custo_do_corpo`. Porém, quando o número de iterações do laço interno **muda a cada iteração do laço externo**, precisamos **somar** todos esses valores — e isso é exatamente o que um somatório faz.

### A fórmula fundamental da soma de inteiros consecutivos

Esta é a identidade que você precisará usar:

```
Σ(i=1 até n) i  =  n(n+1)/2
```

Em palavras: a soma de todos os inteiros de 1 até n é igual a n vezes (n+1), dividido por 2.

**Por que isso funciona?** É a fórmula de Gauss. Imagine somar 1+2+3+...+n. Se você parear o primeiro com o último (1+n), o segundo com o penúltimo (2+(n-1)), e assim por diante, cada par soma (n+1). Há n/2 pares, então o total é n(n+1)/2.

### Os somatórios úteis apresentados no material

```
Σ(i=1 até n) i     =  n(n+1)/2
Σ(i=1 até n) i²    =  n(n+1)(2n+1)/6
Σ(i=0 até k) 2^k   =  2^(k+1) - 1
Σ(i=0 até k) a^i   =  (a^(k+1) - 1)/(a-1),  para a ≠ 1
Σ(i=0 até k) 1/2^i =  2 - 1/2^k
```

Você não precisa decorar essas fórmulas — precisa reconhecer **quando** cada uma se aplica.

---

## 3. O Problema Central: Laços com Iterações Variáveis

O desafio do `alg4` é que o laço interno começa em `pos+1`, não em 0. Isso significa que quando `pos=0`, o laço interno faz n-1 iterações; quando `pos=1`, faz n-2; quando `pos=2`, faz n-3; e assim por diante. O laço interno **encolhe** a cada rodada do laço externo.

Sempre que você identificar esse padrão — **laço interno com ponto de partida que depende do laço externo** — saiba que um somatório estará envolvido.

---

## 4. Análise Detalhada do alg4 — Passo a Passo

```java
void alg4(int[] array) {
    for(int pos = 0; pos < array.length-1; pos++) {   // linha 2
        int menor = pos;                               // linha 3
        for(int j = pos+1; j < array.length; j++) {   // linha 4
            if(array[j] < array[menor])               // linha 5
                menor = j;                            // linha 6
        }
        int aux = array[menor];                        // linha 8
        array[menor] = array[pos];                     // linha 9
        array[pos] = aux;                              // linha 10
    }
}
```

---

### Passo 1 — O que o algoritmo faz?

Este é o clássico **Selection Sort** (Ordenação por Seleção). A lógica é:

Para cada posição `pos` do array, encontre o **menor elemento** entre `pos` e o final do array, e troque-o com o elemento que está em `pos`.

Resultado: após cada iteração do laço externo, a posição `pos` contém o menor elemento ainda não ordenado.

**Por que entender o algoritmo antes de analisar?** Porque entender o que o código faz ajuda a verificar se sua contagem de operações faz sentido. Se você sabe que cada iteração do laço externo "coloca um elemento no lugar certo", fica claro que o laço externo executa n-1 vezes (o último elemento já estará no lugar automaticamente).

---

### Passo 2 — Identificar as operações relevantes

A operação mais relevante é a **comparação** na linha 5: `array[j] < array[menor]`. Ela é executada em toda iteração do laço interno, sem exceção. Custo: **1**.

As linhas 8, 9, 10 são as trocas, executadas uma vez por iteração do laço externo. Custo: **3** por iteração do laço externo. Veremos isso depois.

---

### Passo 3 — Analisar o laço externo (linha 2)

```java
for(int pos = 0; pos < array.length-1; pos++)
```

Repare: a condição é `pos < array.length-1`, não `pos < array.length`. O laço vai de 0 até n-2 inclusive, o que dá **n-1 iterações**.

> **Por que n-1 e não n?** Quando restarmos apenas o último elemento (pos = n-1), ele já está no lugar certo — não há com quem compará-lo. Por isso o laço para em n-2, fazendo n-1 passagens.

---

### Passo 4 — Analisar o laço interno (linha 4) — A parte crítica

```java
for(int j = pos+1; j < array.length; j++)
```

O início de j é `pos+1`. Isso é o diferencial. Veja o que acontece a cada iteração do laço externo:

| Valor de pos | j começa em | j vai até | Nº de iterações do laço j |
|---|---|---|---|
| 0 | 1 | n-1 | n-1 |
| 1 | 2 | n-1 | n-2 |
| 2 | 3 | n-1 | n-3 |
| ... | ... | ... | ... |
| n-2 | n-1 | n-1 | 1 |
| n-1 | n | — | 0 (laço externo já parou) |

O número de iterações do laço interno forma a sequência: **n-1, n-2, n-3, ..., 1, 0**.

---

### Passo 5 — Montar o somatório

O custo total das comparações (linha 5) é a soma de todas as iterações do laço interno ao longo de todas as iterações do laço externo:

```
Custo total = (n-1) + (n-2) + (n-3) + ... + 1 + 0
```

Isso é exatamente:

```
Σ(i=0 até n-1) i  =  0 + 1 + 2 + ... + (n-1)
```

---

### Passo 6 — Aplicar a fórmula do somatório

Usamos a identidade da soma de Gauss. A soma de 0 até (n-1) é equivalente à soma de 1 até (n-1) (o zero não contribui):

```
Σ(i=0 até n-1) i  =  Σ(i=1 até n-1) i
```

Aplicando a fórmula Σ(i=1 até m) i = m(m+1)/2, com m = n-1:

```
= (n-1) × (n-1+1) / 2
= (n-1) × n / 2
= (n² - n) / 2
```

---

### Passo 7 — Incluir o custo das trocas (linhas 8-10)

As três linhas de troca executam **uma vez por iteração do laço externo**, que executa n-1 vezes:

```
Custo das trocas = 3 × (n-1) = 3n - 3
```

---

### Passo 8 — Calcular f(n) total

```
f(n) = custo_comparações + custo_trocas
f(n) = (n² - n)/2  +  3(n - 1)
f(n) = n²/2 - n/2 + 3n - 3
f(n) = n²/2 + (5n/2) - 3
```

**Classificação em Big-O:** o termo dominante é n²/2, que é O(n²). Descartamos constantes e termos menores:

```
f(n) = O(n²)
```

---

### Passo 9 — Há variação de casos?

**Não.** O laço interno sempre percorre exatamente de `pos+1` até `n-1`, independentemente dos valores do array. A condição do `if` na linha 5 pode ou não executar a linha 6, mas a comparação em si (que é o que contamos) **sempre ocorre**. Portanto, Selection Sort tem o mesmo custo para qualquer entrada de tamanho n — não há melhor, pior ou caso médio distintos para comparações.

---

## 5. O Padrão Geral: Quando Usar Somatório

Todo vez que você encontrar um laço com este formato:

```java
for(int i = 0; i < n; i++) {
    for(int j = i+1; j < n; j++) {
        // operação O(1)
    }
}
```

Reconheça imediatamente: o laço interno executa n-1, n-2, ..., 1, 0 vezes. O custo será sempre:

```
Σ(i=0 até n-1) i  =  (n² - n)/2  →  O(n²)
```

Esse padrão aparece em muitos algoritmos clássicos: Selection Sort, Bubble Sort, verificação de pares únicos em arrays, etc.

---

## 6. Visualizando o Somatório de Forma Concreta

Para n=5, o laço interno executa:

```
pos=0: 4 iterações  ████
pos=1: 3 iterações  ███
pos=2: 2 iterações  ██
pos=3: 1 iteração   █
pos=4: 0 iterações  (laço externo já parou)

Total = 4+3+2+1 = 10 = (5²-5)/2 = 20/2 = 10  ✓
```

Visualmente, você está somando a área de um triângulo formado por blocos — daí o n²/2.

---

## 7. Dicas Para Não Precisar Decorar

**Dica 1 — Reconheça o triângulo.** Sempre que o laço interno começa em `i+1` ou `pos+1` em vez de 0, você está somando um triângulo de operações. O resultado sempre será proporcional a n²/2 → O(n²).

**Dica 2 — Teste com n pequeno.** Se n=4, o laço interno faz 3+2+1+0 = 6 iterações. A fórmula dá (16-4)/2 = 6. Correto. Esse teste rápido confirma se você montou o somatório certo.

**Dica 3 — A fórmula de Gauss é sua amiga.** Toda vez que aparecer uma soma de inteiros consecutivos (seja crescente ou decrescente), aplique n(n+1)/2 ajustado ao limite correto. Não tente somar manualmente.

**Dica 4 — Separe os custos.** Algumas operações estão dentro do laço interno (custo proporcional ao somatório) e outras fora dele, mas dentro do laço externo (custo proporcional a n-1). Some os dois ao final — mas lembre que o Big-O só preserva o maior.

**Dica 5 — Laço interno constante vs. variável.** Se o laço interno vai de 0 a n sempre: custo = n × n = n². Se vai de i+1 a n: custo = n²/2. Ambos são O(n²), mas a função exata é diferente.