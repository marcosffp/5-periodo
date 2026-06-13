# Divisão e Conquista — Guia de Estudos

## 1. Por que existe essa técnica?

Algoritmos ingênuos sobre entradas grandes podem ter custo proibitivo. Um algoritmo O(n²) aplicado a n = 1.000.000 executa 10¹² operações. Se esse mesmo problema for partido em 10 pedaços de 100.000 elementos, cada pedaço custa 10¹⁰ operações, e 10 pedaços somam 10¹¹ — **uma ordem de grandeza a menos**. Dividir antes de resolver pode, matematicamente, reduzir o total de trabalho.

---

## 2. Definição

Técnica de projeto de algoritmos que aumenta a eficiência quebrando o problema em partes **antes** de começar a resolvê-lo. O padrão tem três etapas:

| Etapa | O que faz |
|---|---|
| **Dividir** | Decompõe a instância em sub-instâncias menores |
| **Conquistar** | Resolve cada sub-instância (geralmente de forma recursiva) |
| **Combinar** | Une as soluções parciais em uma solução global |

---

## 3. Algoritmo Geral (pseudocódigo)

```
Função DivisaoEConquista(x):
    se x é pequeno o suficiente:
        retornar Resolve_Diretamente(x)        // caso base
    senao:
        decompor x em x1, x2, ..., xn          // DIVIDIR
        para cada xi:
            yi = DivisaoEConquista(xi)          // CONQUISTAR
        combinar y1, y2, ..., yn em Y           // COMBINAR
        retornar Y
```

---

## 4. Equação de Recorrência

Todo algoritmo recursivo de D&C gera uma recorrência da forma:

```
T(n) = a · T(n/b) + f(n)
```

- **a** → número de subproblemas gerados
- **b** → fator de redução do tamanho (cada subproblema tem tamanho n/b)
- **f(n)** → custo da divisão + custo da combinação

---

## 5. Quando Usar?

Quatro condições devem ser satisfeitas:

1. Deve ser **possível decompor** a instância em sub-instâncias.
2. As sub-instâncias devem ser **balanceadas** (tamanhos similares).
3. **Não deve haver sobreposição** entre sub-instâncias.
4. A **combinação dos resultados deve ser eficiente**.

---

## 6. Vantagens e Desvantagens

**Vantagens**
- Resolve problemas difíceis de forma elegante
- Tende a complexidade **logarítmica**
- Menor número de acessos à memória
- Altamente **paralelizável**

**Desvantagens**
- Overhead de recursão (pilha de chamadas)
- Dificuldade em identificar o caso base correto
- Ineficiente quando há **repetição de subproblemas** (ver Fibonacci)

---

## 7. Exemplos Clássicos

### 7.1 Max/Min em vetor — MAXMIN4

**Problema:** dado vetor A[0..n-1], encontrar o maior e o menor elemento.

**Abordagem ingênua:** melhor caso O(n−1), pior caso O(2n−2).

**Com D&C:**
- Caso base: subvetor com 1 ou 2 elementos → 1 comparação resolve os dois valores.
- Divisão: calcular o índice do meio → `Meio = ⌊(Linf + Lsup) / 2⌋`
- Recursão nas duas metades.
- Combinação: 2 comparações (Max1 vs Max2 e Min1 vs Min2).

```
Recorrência:
  T(1) = 1
  T(n) = 2·T(n/2) + 2
```

A ideia-chave: ao chegar em 2 elementos, **uma única comparação** já entrega max e min simultaneamente — mais eficiente do que duas passagens separadas.

---

### 7.2 Mergesort

- **Divisão:** partir o vetor ao meio recursivamente até sobrar elementos individuais.
- **Conquista:** elemento individual já está "ordenado".
- **Combinação:** **intercalação** de dois subvetores vizinhos já ordenados.

```
Recorrência: T(n) = 2·T(n/2) + O(n)  →  O(n log n)
```

O custo está todo na **combinação** (o merge).

---

### 7.3 Quicksort

- **Divisão:** escolher um pivô e particionar o vetor (elementos menores à esquerda, maiores à direita).
- **Conquista:** recursão nos dois lados do pivô.
- **Combinação:** **não há** — a partição já posiciona o pivô corretamente, então "combinar" é gratuito.

```
Médio: T(n) = 2·T(n/2) + O(n)  →  O(n log n)
Pior caso (pivô sempre extremo): O(n²)
```

O pior caso ilustra o problema de **desbalanceamento** — se o pivô sempre for o menor ou maior elemento, um lado recebe n−1 elementos e o outro 0.

---

### 7.4 Subsequência de Soma Máxima

**Problema:** dado array A, encontrar a subsequência contígua cuja soma é máxima.

**Por que D&C simples não é trivial?** A solução ótima pode estar:
1. Inteiramente na **metade esquerda**
2. Inteiramente na **metade direita**
3. **Cruzando o meio** — parte na esquerda, parte na direita

A combinação precisa calcular as três possibilidades e retornar a maior.

**Cálculo da interseção (`Maior_Soma_Cruzando_Meio`):**
- Expandir para a esquerda a partir do meio, acumulando a maior soma possível.
- Expandir para a direita a partir do meio+1, acumulando a maior soma possível.
- Retornar a soma das duas melhores expansões.

```
Estrutura final:
  maior_esq  = recursão(esquerda)
  maior_dir  = recursão(direita)
  cruzamento = Maior_Soma_Cruzando_Meio(...)
  retornar max(maior_esq, maior_dir, cruzamento)

Recorrência: T(n) = 2·T(n/2) + O(n)  →  O(n log n)
```

---

## 8. Armadilhas Importantes

### 8.1 Desbalanceamento

Sub-instâncias com tamanhos muito diferentes destroem o ganho logarítmico. O pior caso do quicksort é o exemplo canônico — uma divisão 1:(n−1) vira O(n²).

### 8.2 Sobreposição de Subproblemas

Quando as sub-instâncias se sobrepõem, o mesmo cálculo é refeito várias vezes. **Fibonacci recursivo** é o exemplo clássico:

```java
int Fib(int n) {
    if (n <= 2) return 1;
    return Fib(n-1) + Fib(n-2);
}
```

Para `Fib(7)`, o valor `Fib(5)` é calculado **duas vezes**, `Fib(4)` três vezes, e assim por diante. A árvore de recursão cresce exponencialmente → O(2ⁿ).

Isso viola a condição 3 ("não deve haver sub-instâncias sobrepostas") e indica que **Programação Dinâmica** seria a técnica correta, não D&C.

---

## 9. Decisões de Projeto

| Decisão | Exemplos |
|---|---|
| **O que é "pequeno o suficiente"?** | MAXMIN4: ≤ 2 elementos. Mergesort/Quicksort: 1 elemento. Na prática, às vezes usa-se um threshold maior (ex: 32 elementos) para evitar overhead de recursão. |
| **Como dividir?** | Sempre que possível, ao meio — garante balanceamento. |
| **Como combinar?** | Mergesort: intercalação O(n). Quicksort: trivial O(1). Max/Min: 2 comparações O(1). Soma máxima: varredura O(n). |

---

## 10. Outros Algoritmos que Usam D&C

- Busca Binária
- Mediana das Medianas
- Algoritmo de Karatsuba (multiplicação de números grandes)
- Menor distância entre par de pontos
- Algoritmo de Strassen (multiplicação de matrizes)
- Contador de inversões em conjuntos numéricos

---

## 11. Resumo Mental

```
D&C funciona bem quando:
  ✅ Divisão balanceada é possível
  ✅ Subproblemas são independentes (sem sobreposição)
  ✅ Combinação é barata (O(1) a O(n))

D&C não funciona bem quando:
  ❌ Divisão é desbalanceada (pior caso quicksort)
  ❌ Subproblemas se repetem (Fibonacci → use DP)
  ❌ Combinação é cara demais (O(n²) ou mais)
```