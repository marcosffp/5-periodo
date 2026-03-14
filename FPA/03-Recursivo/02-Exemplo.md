# Análise de Algoritmos Recursivos com Dois Parâmetros — Resumo Didático

---

## 1. Ideia Principal

Este conteúdo apresenta um novo desafio: **o que acontece quando um algoritmo recursivo tem dois parâmetros que controlam seu comportamento?** Aqui, a recursividade depende de `i`, mas o laço interno depende de `n` (o tamanho do vetor). São duas dimensões diferentes e precisamos saber como tratá-las na equação de recorrência. No final, há um exercício com **duas chamadas recursivas simultâneas**, o que muda completamente o raciocínio.

---

## 2. Conceitos Importantes

**Dois parâmetros, dois papéis distintos:**
- `i` é o parâmetro que **controla a recursão** — ele diminui a cada chamada.
- `n` é o **tamanho do vetor** (vet.length) — ele é constante durante toda a execução.

**Isso significa:** quando montamos a equação de recorrência, a variável livre é `i`, e `n` aparece como uma constante que se acumula a cada nível de recursão.

**Duas chamadas recursivas:** quando uma função chama a si mesma duas vezes por nível, a árvore de recursão se bifurca. Isso é característico de algoritmos de divisão e conquista como o mergesort.

---

## 3. Método Geral

O roteiro continua o mesmo dos problemas anteriores, com um detalhe adicional importante:

1. Identificar base e caso recursivo.
2. Definir qual parâmetro é a "dimensão da recursão" (aqui: `i`) e qual é constante (aqui: `n`).
3. Identificar as operações relevantes em cada chamada.
4. Montar a equação de recorrência corretamente.
5. Expandir por substituição iterativa, observando o padrão.
6. Escrever a Fórmula Geral (F.G) com a variável de expansão `k`.
7. Aplicar a condição da base para encontrar k.
8. Substituir e obter a fórmula fechada.

---

## 4. Explicação Detalhada dos Exemplos

---

### Exemplo Principal — `rec2`

```
int rec2(int[] vet, int i) {
    if (i == 0)              // base
        return vet[0];
    else {
        for (int j = 0; j < vet.length; j++) {  // laço
            vet[j] += vet[i];                    // operação: soma
        }
        return rec2(vet, i-1);  // chamada recursiva
    }
}
```

---

#### Passo 1 — Identificar base e recursividade

- **Base:** quando `i == 0`, apenas retorna `vet[0]`. Nenhuma operação relevante.
- **Recursivo:** quando `i > 0`, executa um laço `for` completo sobre o vetor, depois chama `rec2(vet, i-1)`.

---

#### Passo 2 — Identificar quem é n e quem é i

O código tem dois parâmetros: `vet` e `i`. A recursão diminui `i` de 1 em 1. O vetor não muda de tamanho — `vet.length` é constante. Chamamos esse tamanho constante de `n`.

**Por que isso importa?** Porque quando montarmos a equação, a parte constante de cada chamada será `n` (o custo do laço), não `1` como no exemplo anterior.

---

#### Passo 3 — Custo da base

Quando `i == 0`, apenas retornamos. Custo constante:

> **S(0) = 1**

---

#### Passo 4 — Custo do caso recursivo

No caso recursivo temos:
- Um laço `for` que percorre **todo o vetor** → executa `n` operações de soma.
- Uma chamada recursiva `rec2(vet, i-1)` → custa S(i-1).

Portanto:

> **S(i) = n + S(i-1)**

**Atenção ao detalhe:** a equação agora está em função de `i`, não de `n`. O `n` aqui é uma constante — representa o tamanho do vetor, que não muda. A variável que diminui é `i`.

---

#### Passo 5 — Montar a equação completa

$$\begin{cases} S(0) = 1 \\ S(i) = S(i-1) + n \end{cases}$$

---

#### Passo 6 — Expandir por substituição iterativa

Aplicamos a regra para S(i-1), depois S(i-2), etc.:

**Partindo de S(i):**

> S(i) = S(i-1) + n

S(i-1) segue a mesma regra: S(i-1) = S(i-2) + n. Substituindo:

> S(i) = [S(i-2) + n] + n = **S(i-2) + 2n**

S(i-2) = S(i-3) + n. Substituindo:

> S(i) = [S(i-3) + n] + 2n = **S(i-3) + 3n**

| Passo | Expressão |
|-------|-----------|
| 0 | S(i-1) + n |
| 1ª substituição | S(i-2) + 2n |
| 2ª substituição | S(i-3) + 3n |
| k-ésima substituição | **S(i-k) + kn** |

---

#### Passo 7 — Escrever a Fórmula Geral

> **F.G = S(i - k) + kn**

Note a diferença em relação ao exemplo anterior: onde antes acumulávamos `+k`, agora acumulamos `+kn`. Isso faz sentido — cada nível de recursão custa `n` operações, não apenas `1`.

---

#### Passo 8 — Aplicar a condição da base

Queremos que a expansão pare na base, ou seja, quando o argumento de S for 0:

> S(i - k) = S(0) → i - k = 0 → **k = i**

---

#### Passo 9 — Substituir k = i na F.G

> S(i) = S(i - i) + i·n = S(0) + i·n = 1 + i·n

**Resultado final:**

> **S(i) = 1 + i·n**

Em notação assintótica: **O(i·n)**, ou seja, **O(n²)** quando i e n são da mesma ordem.

**Intuição:** a função executa `i` chamadas recursivas, e em cada uma delas percorre o vetor inteiro de tamanho `n`. É como um loop dentro de um loop — custo quadrático.

---

### Exercício 1 — `rec3` (Dois parâmetros e DUAS chamadas recursivas)

```
int rec3(int[] vet, int a, int b) {
    if (a == b)                      // base
        return vet[a];
    else {
        int c = (a + b) / 2;
        int d = rec3(vet, a, c);     // 1ª chamada recursiva
        int e = rec3(vet, c, b);     // 2ª chamada recursiva
        return d + e;
    }
}
```

---

#### Passo 1 — Entender o que o código faz

A função recebe um vetor e dois índices `a` e `b`. Ela:
- Para quando `a == b` (índices iguais → um único elemento).
- Caso contrário, divide o intervalo [a, b] ao meio com `c = (a+b)/2`, e chama a si mesma **duas vezes**: uma para a metade esquerda e outra para a metade direita.

**Isso é um algoritmo clássico de divisão e conquista**, muito parecido com o mergesort.

---

#### Passo 2 — Definir n

Aqui `n` representa o **tamanho do intervalo** que a função está processando, ou seja, `n = b - a`. A cada chamada, o problema é dividido ao meio.

---

#### Passo 3 — Custo da base

Quando `a == b`, o intervalo tem tamanho 0 (um único elemento). Nenhuma operação relevante além do retorno:

> **S(0) = 1**

---

#### Passo 4 — Custo do caso recursivo

No caso recursivo, a operação relevante é a soma `d + e` na linha 8 — isso custa **1 operação**. E há **duas chamadas recursivas**, cada uma em um subproblema de tamanho `n/2`:

> **S(n) = 1 + S(n/2) + S(n/2) = 1 + 2·S(n/2)**

---

#### Passo 5 — Equação de recorrência completa

$$\begin{cases} S(0) = 1 \\ S(n) = 2 \cdot S(n/2) + 1 \end{cases}$$

---

#### Passo 6 — Expandir por substituição iterativa

**Partindo de S(n):**

> S(n) = 2·S(n/2) + 1

Agora S(n/2) segue a mesma regra: S(n/2) = 2·S(n/4) + 1. Substituindo:

> S(n) = 2·[2·S(n/4) + 1] + 1 = **4·S(n/4) + 2 + 1 = 4·S(n/4) + 3**

S(n/4) = 2·S(n/8) + 1. Substituindo:

> S(n) = 4·[2·S(n/8) + 1] + 3 = **8·S(n/8) + 4 + 3 = 8·S(n/8) + 7**

Vamos organizar o padrão:

| Passo | Expressão |
|-------|-----------|
| Inicial | 2·S(n/2) + 1 |
| 1ª substituição | 4·S(n/4) + 3 |
| 2ª substituição | 8·S(n/8) + 7 |
| k-ésima substituição | **2^k · S(n/2^k) + (2^k - 1)** |

**Por que (2^k - 1)?** Porque a constante cresce assim: 1, 3, 7, 15... Isso é sempre 2^k - 1. Perceba: 1 = 2¹ - 1, 3 = 2² - 1, 7 = 2³ - 1.

---

#### Passo 7 — Escrever a Fórmula Geral

> **F.G = 2^k · S(n/2^k) + (2^k - 1)**

---

#### Passo 8 — Aplicar a condição da base

Queremos que S(n/2^k) = S(0), ou seja, que n/2^k = 0 não é possível (nunca chega a zero por divisão). Na prática, paramos quando n/2^k = 1:

> n/2^k = 1 → 2^k = n → **k = log₂(n)**

---

#### Passo 9 — Substituir k = log₂(n) na F.G

> S(n) = 2^(log₂n) · S(1) + (2^(log₂n) - 1)

Como 2^(log₂n) = n e S(1) = 1:

> S(n) = n · 1 + (n - 1) = **2n - 1**

**Resultado final:**

> **S(n) = O(n)** — complexidade linear!

**Intuição importante:** embora a função se chame duas vezes por nível, ela divide o problema ao meio. A árvore de recursão tem log₂(n) níveis, e em cada nível o trabalho total é proporcional a n. Isso resulta em custo linear — o mesmo resultado de percorrer o vetor uma única vez.

---

## 5. Como Aplicar o Raciocínio em Outros Exercícios

O segredo está em reconhecer o **formato** da recorrência antes de resolver. Veja os padrões mais comuns:

| Recorrência | O que indica | Resultado |
|---|---|---|
| S(n) = S(n-1) + 1 | reduz 1 por 1, custo constante | O(n) |
| S(n) = S(n-1) + n | reduz 1 por 1, custo linear por nível | O(n²) |
| S(n) = S(n/2) + 1 | divide ao meio, custo constante | O(log n) |
| S(n) = 2·S(n/2) + 1 | divide ao meio, duas chamadas | O(n) |
| S(n) = 2·S(n/2) + n | divide ao meio, duas chamadas + trabalho linear | O(n log n) |

---

## 6. Dicas para Resolver sem Precisar Decorar

**Dica 1 — Separe quem é a "variável da recursão" e quem é "constante".**
No rec2, `i` diminui e `n` é fixo. Na equação, `n` entra como constante que se multiplica pelo número de passos.

**Dica 2 — Com duas chamadas recursivas, o coeficiente de S dobra a cada substituição.**
Se S(n) = 2·S(n/2) + c, após k substituições você terá 2^k · S(n/2^k) + algo.

**Dica 3 — Para encontrar o padrão da constante acumulada, calcule os primeiros três termos à mão.**
Se a constante acumulada for 1, 3, 7, 15... é 2^k - 1. Se for 1, 2, 3... é k. O padrão fica óbvio com três termos.

**Dica 4 — Para parar a expansão, iguale o argumento de S ao valor da base.**
Se a base é S(0): iguale o argumento a 0. Se a base é S(1): iguale a 1. Isso resolve k em termos de n.

**Dica 5 — A intuição sempre confirma a matemática.**
Se o algoritmo visita cada elemento uma vez → O(n). Se faz algo para cada par → O(n²). Se divide ao meio sem trabalho extra → O(log n). Se divide ao meio com trabalho linear → O(n log n). Use isso como verificação após o cálculo.