# 🔀 Divisão e Conquista — Guia de Estudos

## 1. 📉 Por que essa técnica existe?

Algoritmos ingênuos sobre entradas grandes podem ter custo proibitivo. Veja como `f(n) = n²` cresce:

| n | f(n) = n² |
|---|---|
| 100 | 10.000 |
| 1.000 | 1.000.000 |
| 10.000 | 100.000.000 |
| 100.000 | 10.000.000.000 (10¹⁰) |
| 1.000.000 | 1.000.000.000.000 (10¹²) |
| 10.000.000 | 100.000.000.000.000 |

Um algoritmo O(n²) aplicado a n = 1.000.000 executa **10¹²** operações. Agora imagine partir esse mesmo problema em **10 pedaços de 100.000 elementos**: cada pedaço custa f(100.000) = 10¹⁰ operações, e os 10 pedaços juntos somam **10 × 10¹⁰ = 10¹¹**.

> 10¹¹ < 10¹² — **uma ordem de grandeza a menos**, só por ter dividido o problema *antes* de resolver.

Esse é o insight central: **matematicamente, a soma do custo das partes pode ser menor que o custo do todo.**

---

## 2. 📐 Definição

Técnica de projeto de algoritmos que aumenta a eficiência **quebrando o problema em partes antes de começar a resolvê-lo**. O padrão tem três etapas:

| Etapa | O que faz |
|---|---|
| **Dividir** | Decompõe a instância em sub-instâncias menores |
| **Conquistar** | Resolve cada sub-instância (geralmente de forma recursiva) |
| **Combinar** | Une as soluções parciais em uma solução global |

Algoritmos amplamente conhecidos que seguem esse padrão: **Mergesort** e **Quicksort**.

**Analogia:** é como organizar uma mudança de casa. Em vez de tentar carregar a casa inteira de uma vez (força bruta), você separa os objetos em caixas menores (**dividir**), resolve o empacotamento de cada caixa de forma independente (**conquistar**) e, no final, empilha as caixas no caminhão (**combinar**).

---

## 3. 🧩 Algoritmo Geral (pseudocódigo)

```
Função Divisao_E_Conquista(x):
    // 1. CASO BASE
    se (x é pequeno o suficiente)
        retornar Resolve_Diretamente(x)
    senao
        // 2. DIVIDIR
        decompor o problema x em n subproblemas: x1, x2, ..., xn
        // 3. CONQUISTAR (resolver recursivamente)
        para cada i de 1 até n:
            yi = Divisao_E_Conquista(xi)
        // 4. COMBINAR
        combinar os resultados y1, y2, ..., yn em uma solução Y
        retornar Y
```

---

## 4. 🔢 Equação de Recorrência

Todo algoritmo recursivo de D&C gera uma recorrência da forma:

```
T(n) = a · T(n/b) + f(n)
```

- **a** → número de subproblemas gerados
- **b** → fator de redução do tamanho (cada subproblema tem tamanho n/b)
- **f(n)** → custo da divisão + custo da combinação

---

## 5. ✅ Quando Usar?

Quatro condições devem ser satisfeitas:

1. Deve ser **possível decompor** a instância em sub-instâncias.
2. As sub-instâncias devem ser **balanceadas** (tamanhos similares).
3. **Não deve haver sobreposição** entre sub-instâncias.
4. A **combinação dos resultados deve ser eficiente**.

---

## 6. ⚖️ Vantagens e Desvantagens

**✅ Vantagens**
- Resolve problemas difíceis de forma elegante
- Tende a complexidade **logarítmica**
- Menor número de acessos à memória
- Altamente **paralelizável**

**❌ Desvantagens**
- Overhead de recursão (pilha de chamadas)
- Dificuldade em identificar o caso base correto
- Ineficiente quando há **repetição de subproblemas** (ver Fibonacci, seção 8.2)

---

## 7. 🛠️ Exemplos Clássicos (com números reais)

### 7.1 🔍 Max/Min em vetor — MAXMIN4

**Problema:** dado o vetor `A = [08, 23, 16, 42, 15, 93, 04, 77]` (n=8), encontrar o maior e o menor elemento.

**Abordagem ingênua:** melhor caso O(n−1) = 7 comparações, pior caso O(2n−2) = 14 comparações.

**Com D&C:**
- **Caso base:** subvetor com 1 ou 2 elementos. Com 2 elementos, **uma única comparação** já entrega o max e o min simultaneamente.
- **Divisão:** calcular o índice do meio → `Meio = ⌊(Linf + Lsup) / 2⌋`, dividir recursivamente em 2.
- **Combinação:** 2 comparações (Max1 vs Max2 e Min1 vs Min2).

**Passo a passo com o vetor de exemplo:**

```
Dividir até sobrar 1 ou 2 elementos:

   [08 23 16 42 15 93 04 77]
        /              \
  [08 23 16 42]     [15 93 04 77]
     /     \            /     \
 [08 23]  [16 42]   [15 93]  [04 77]
```

**Conquistar (caso base — 1 comparação por par):**

| Par | Comparação | Min | Max |
|---|---|---|---|
| [08,23] | 08 < 23 | 08 | 23 |
| [16,42] | 16 < 42 | 16 | 42 |
| [15,93] | 15 < 93 | 15 | 93 |
| [04,77] | 04 < 77 | 04 | 77 |

**Combinar — nível 1 (2 comparações por grupo: Min1 vs Min2, Max1 vs Max2):**

| Grupo | min(Min1,Min2) | max(Max1,Max2) | Resultado |
|---|---|---|---|
| [08,23,16,42] | min(08,16)=08 | max(23,42)=42 | Min=08, Max=42 |
| [15,93,04,77] | min(15,04)=04 | max(93,77)=93 | Min=04, Max=93 |

**Combinar — nível final:**

> min(08,04) = **04**, max(42,93) = **93** → **Min = 04, Max = 93** ✅ (correto: o vetor original tem 04 como menor e 93 como maior valor)

**Contagem de comparações:** 4 (base) + 2×2 (combinação nível 1) + 2 (combinação final) = **10**

```
Recorrência:
  T(1) = 1
  T(n) = 2·T(n/2) + 2   →   para n potência de 2: T(n) = 3n/2 - 2
```

Para n=8: T(8) = 3·8/2 − 2 = **10**. Compare com a força bruta: melhor caso 7, pior caso 14. O D&C fica **sempre em 10**, independentemente da ordem dos dados — um meio-termo previsível e estável.

A ideia-chave: ao chegar em 2 elementos, **uma única comparação** já entrega max e min simultaneamente — mais eficiente do que duas passagens separadas.

---

### 7.2 🔀 Mergesort

**Problema:** ordenar o vetor `A = [1, 4, 8, 3, 6, 5, 2, 7]`.

- **Divisão:** partir o vetor ao meio recursivamente até sobrar elementos individuais.
- **Conquista:** elemento individual já está "ordenado".
- **Combinação:** **intercalação** (merge) de dois subvetores vizinhos já ordenados.

**Divisão completa:**

```
[1 4 8 3 6 5 2 7]
       │
   ┌───┴───┐
[1 4 8 3] [6 5 2 7]
   │           │
 ┌─┴─┐       ┌─┴─┐
[1 4][8 3] [6 5][2 7]
  │    │     │    │
┌─┴┐ ┌─┴┐  ┌─┴┐ ┌─┴┐
[1][4][8][3][6][5][2][7]   ← caso base: 1 elemento
```

**Conquista + Combinação (merge subindo na árvore):**

| Nível | Operação | Resultado |
|---|---|---|
| 1 | merge([1],[4]) · merge([8],[3]) · merge([6],[5]) · merge([2],[7]) | [1,4] · [3,8] · [5,6] · [2,7] |
| 2 | merge([1,4],[3,8]) · merge([5,6],[2,7]) | [1,3,4,8] · [2,5,6,7] |
| 3 (final) | merge([1,3,4,8],[2,5,6,7]) | **[1,2,3,4,5,6,7,8]** ✅ |

```
Recorrência: T(n) = 2·T(n/2) + O(n)  →  O(n log n)
```

O custo está todo na **combinação** (o merge é O(n) por nível, e há log n níveis).

**Analogia:** é como juntar dois baralhos de cartas já ordenados em um único baralho ordenado — basta sempre olhar o topo dos dois montes e tirar o menor.

---

### 7.3 ⚡ Quicksort

**Problema:** ordenar o vetor `A = [23, 93, 8, 4, 15, 77, 16, 42]`, usando o **último elemento (42)** como pivô.

- **Divisão:** escolher um pivô e particionar o vetor (elementos menores à esquerda, maiores à direita).
- **Conquista:** recursão nos dois lados do pivô.
- **Combinação:** **não há** — a partição já posiciona o pivô corretamente, então "combinar" é gratuito.

**Partição (esquema de Lomuto), pivô = 42:**

| Elemento avaliado | É < pivô (42)? | Ação |
|---|---|---|
| 23 | sim | mantém posição |
| 93 | não | avança sem trocar |
| 8 | sim | troca para a "zona dos menores" |
| 4 | sim | troca para a "zona dos menores" |
| 15 | sim | troca para a "zona dos menores" |
| 77 | não | avança sem trocar |
| 16 | sim | troca para a "zona dos menores" |

**Resultado da partição:**

```
[23 8 4 15 16 | 42 | 93 77]
```

O pivô **42** ficou corretamente posicionado: tudo à esquerda (23, 8, 4, 15, 16) é menor que ele, tudo à direita (93, 77) é maior.

- A conquista é "imediata" no retorno: agora basta recursão em `[23, 8, 4, 15, 16]` e em `[93, 77]`, **independentemente** uma da outra.

```
Médio: T(n) = 2·T(n/2) + O(n)  →  O(n log n)
Pior caso (pivô sempre extremo): O(n²)
```

O pior caso ilustra o problema de **desbalanceamento** (ver seção 8.1) — se o pivô sempre for o menor ou maior elemento, um lado recebe n−1 elementos e o outro 0.

**Analogia:** o pivô é como um fiscal que separa uma fila de pessoas por altura — quem é mais baixo vai para um lado, quem é mais alto para o outro. Depois, cada grupo se reorganiza sozinho, sem precisar mais do fiscal.

---

### 7.4 📊 Subsequência de Soma Máxima

**Problema:** dado o array `A = [10, 2, -20, 5, 3, -1, -2, 8]` (índices 0 a 7), encontrar a subsequência **contígua** cuja soma é máxima.

**Por que D&C simples não é trivial?** A solução ótima pode estar:
1. Inteiramente na **metade esquerda**
2. Inteiramente na **metade direita**
3. **Cruzando o meio** — parte na esquerda, parte na direita

**Cálculo da interseção (`Maior_Soma_Cruzando_Meio`):**
- Expandir para a **esquerda** a partir do meio, acumulando a maior soma possível.
- Expandir para a **direita** a partir do meio+1, acumulando a maior soma possível.
- Retornar a soma das duas melhores expansões.

```
Função Maior_Soma_Cruzando_Meio(A, inicio, meio, fim):
  // 1. Expandindo para a esquerda
  soma_esquerda = -INFINITO
  soma_atual = 0
  para i de meio descendo até inicio:
    soma_atual = soma_atual + A[i]
    se soma_atual > soma_esquerda:
      soma_esquerda = soma_atual
  // 2. Expandindo para a direita
  soma_direita = -INFINITO
  soma_atual = 0
  para j de (meio+1) até fim:
    soma_atual = soma_atual + A[j]
    se soma_atual > soma_direita:
      soma_direita = soma_atual
  // 3. A maior intersecção é a união dos dois melhores lados
  retornar (soma_esquerda + soma_direita)
```

**Resolvendo o exemplo passo a passo:**

| Subproblema (índices) | Vetor | maior_esq | maior_dir | maior_cruzamento | Resultado |
|---|---|---|---|---|---|
| [0,1] = [10,2] | — | 10 | 2 | 10+2=12 | **12** |
| [2,3] = [-20,5] | — | -20 | 5 | -20+5=-15 | **5** |
| [0,3] = [10,2,-20,5] | combina os dois acima | 12 | 5 | esq: max(2,2+10)=12 / dir: max(-20,-20+5)=-15 → 12+(-15)=-3 | **12** |
| [4,5] = [3,-1] | — | 3 | -1 | 3+(-1)=2 | **3** |
| [6,7] = [-2,8] | — | -2 | 8 | -2+8=6 | **8** |
| [4,7] = [3,-1,-2,8] | combina os dois acima | 3 | 8 | esq: max(-1,-1+3)=2 / dir: max(-2,-2+8)=6 → 2+6=8 | **8** |
| **[0,7] (final)** | array completo | 12 | 8 | esq: max(5,5-20,5-20+2,5-20+2+10)=**5** / dir: max(3,3-1,3-1-2,3-1-2+8)=**8** → 5+8=**13** | **13** ✅ |

**Resultado final:** a maior soma é **13**, vinda do cruzamento — a subsequência `[5, 3, -1, -2, 8]` (índices 3 a 7): 5+3−1−2+8 = 13.

```
Estrutura final:
  maior_esq  = recursão(esquerda)
  maior_dir  = recursão(direita)
  cruzamento = Maior_Soma_Cruzando_Meio(...)
  retornar max(maior_esq, maior_dir, cruzamento)

Recorrência: T(n) = 2·T(n/2) + O(n)  →  O(n log n)
```

---

## 8. ⚠️ Armadilhas Importantes

### 8.1 Desbalanceamento

Sub-instâncias com tamanhos muito diferentes destroem o ganho logarítmico.

**Exemplo concreto:** Quicksort no vetor já ordenado `[1, 2, 3, 4, 5]`, escolhendo sempre o **último elemento** como pivô:

```
[1 2 3 4 5]  pivô=5 → tudo menor, partição: [1 2 3 4] | 5 | []
[1 2 3 4]    pivô=4 → tudo menor, partição: [1 2 3]   | 4 | []
[1 2 3]      pivô=3 → tudo menor, partição: [1 2]     | 3 | []
[1 2]        pivô=2 → tudo menor, partição: [1]       | 2 | []
[1]          caso base
```

Cada chamada reduz o problema em **apenas 1 elemento**, e o lado direito é sempre vazio. A recorrência deixa de ser `T(n) = 2T(n/2) + O(n)` e passa a ser:

```
T(n) = T(n-1) + O(n)  →  O(n²)
```

Esse é o pior caso canônico do quicksort — uma divisão **1:(n−1)** em vez de uma divisão balanceada **(n/2):(n/2)**.

### 8.2 Sobreposição de Subproblemas

Quando as sub-instâncias se sobrepõem, o mesmo cálculo é refeito várias vezes. **Fibonacci recursivo** é o exemplo clássico:

```java
int Fib(int n) {
    if (n <= 2) return 1;
    return Fib(n-1) + Fib(n-2);
}
```

Árvore de recursão para `Fib(7)`:

```
                              7
              ┌───────────────┴───────────────┐
              6                                5
        ┌─────┴─────┐                   ┌─────┴─────┐
        5           4                   4           3
     ┌──┴──┐     ┌──┴──┐             ┌──┴──┐     ┌──┴──┐
     4     3     3     2             3     2     2     1
```

Note que `Fib(5)` aparece **2 vezes**, `Fib(4)` aparece **3 vezes**, `Fib(3)` aparece **4 vezes** — e a árvore continua crescendo exponencialmente até a base. Resultado: **O(2ⁿ)**.

Isso viola a condição 3 da seção 5 ("não deve haver sub-instâncias sobrepostas") e indica que **Programação Dinâmica** seria a técnica correta, não D&C — guardar os resultados já calculados (memoização) elimina todo o retrabalho.

---

## 9. 🧭 Decisões de Projeto

| Decisão | Exemplos |
|---|---|
| **O que é "pequeno o suficiente"?** | MAXMIN4: ≤ 2 elementos. Mergesort/Quicksort: 1 elemento. Na prática, às vezes usa-se um threshold maior (ex: 32 elementos) para evitar overhead de recursão — com 14 milhões de elementos e log₂(n)≈24, após ~24 divisões já se chega a 1 elemento. |
| **Como dividir?** | Sempre que possível, ao meio — garante balanceamento. |
| **Como combinar?** | Mergesort: intercalação O(n). Quicksort: trivial O(1) (a partição já resolve). Max/Min: 2 comparações O(1). Soma máxima: varredura O(n). |

> 💡 "A combinação dos resultados deve ser eficiente" — se a combinação for cara (O(n²) ou mais), o ganho da divisão pode ser anulado.

---

## 10. 📚 Outros Algoritmos que Usam D&C

- Busca Binária
- Mediana das Medianas
- Algoritmo de Karatsuba (multiplicação de números grandes)
- Menor distância entre par de pontos
- Algoritmo de Strassen (multiplicação de matrizes)
- Contador de inversões em conjuntos numéricos

---

## 11. 🎯 Resumo Final

| Aspecto | Divisão e Conquista |
|---|---|
| **Estratégia** | Dividir o problema em partes menores, resolver cada parte (recursivamente) e combinar os resultados |
| **Recorrência típica** | T(n) = a·T(n/b) + f(n) |
| **Complexidade comum** | O(n log n) quando balanceado e combinação é O(n) |
| **Funciona bem quando** | Decomposição possível, sub-instâncias balanceadas, sem sobreposição, combinação eficiente |
| **Não funciona bem quando** | Divisão desbalanceada (pior caso quicksort), subproblemas se repetem (Fibonacci → use DP), combinação cara (O(n²)+) |
| **Vantagens** | Elegância, eficiência logarítmica, menos acessos à memória, paralelizável |
| **Desvantagens** | Overhead de recursão, caso base nem sempre óbvio |
| **Exemplos clássicos** | MAX/MIN, Mergesort, Quicksort, Subsequência de Soma Máxima, Busca Binária, Karatsuba, Strassen |

---

## 12. 🧠 Resumo Mental

```
D&C funciona bem quando:
  ✅ Divisão balanceada é possível
  ✅ Subproblemas são independentes (sem sobreposição)
  ✅ Combinação é barata (O(1) a O(n))

D&C não funciona bem quando:
  ❌ Divisão é desbalanceada (pior caso quicksort → O(n²))
  ❌ Subproblemas se repetem (Fibonacci → O(2ⁿ), use DP)
  ❌ Combinação é cara demais (O(n²) ou mais)
```
