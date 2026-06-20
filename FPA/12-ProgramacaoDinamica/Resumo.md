# 🧠 Programação Dinâmica — Guia de Estudos

## 1. 🎯 Definição

**Programação Dinâmica (PD)** é uma técnica de projeto de algoritmos baseada em um método de solução por **tabela** (memória dos resultados). É tipicamente usada para resolver **problemas combinatórios** e se torna especialmente útil quando há **sobreposição de subproblemas** na solução recursiva.

> A ideia central: ao invés de recalcular o mesmo subproblema múltiplas vezes (como na recursão ingênua), armazena-se o resultado de cada subproblema na primeira vez que é calculado e reutiliza-se esse resultado sempre que necessário.

A PD exige que o problema tenha **subestrutura ótima** — a mesma propriedade dos algoritmos gulosos. A diferença é que a PD **não toma decisões irrevogáveis**: ela avalia todas as combinações possíveis de forma eficiente, usando resultados já calculados.

---

## 2. 📛 Por que "Programação" e "Dinâmica"?

O nome não tem relação com programação de computadores. Vem da matemática:

- **Programação** = *planejamento*; estabelecer um plano para fazer algo (como em "programação linear").
- **Dinâmica** = a solução é construída *dinamicamente*, passo a passo, à medida que os subproblemas menores são resolvidos e seus resultados são usados para resolver problemas maiores.

---

## 3. 🔑 Pré-requisitos: Sobreposição de Subproblemas

### O problema da recursão ingênua — Fibonacci

A função recursiva clássica de Fibonacci ilustra o problema que a PD resolve:

```c
int Fib(int n) {
    if (n <= 2) return 1;
    return Fib(n - 1) + Fib(n - 2);
}
```

Para `Fib(7)`, a árvore de chamadas recalcula `Fib(5)` duas vezes, `Fib(4)` três vezes, `Fib(3)` cinco vezes... O padrão cresce exponencialmente: **complexidade O(2ⁿ)**.

O problema tem três características que tornam a recursão ingênua ruim:
1. **Sobreposição de subproblemas** — os mesmos subproblemas são resolvidos repetidamente;
2. **Não há memória dos resultados** — cada chamada recomeça do zero;
3. **Complexidade exponencial** — cresce inviável rapidamente.

### Solução com PD — bottom-up

```c
int fib_PD(int n) {
    int[] results = new int[n + 1];
    results[1] = 1; results[2] = 1;
    for (int i = 3; i <= n; i++) {
        results[i] = results[i-1] + results[i-2];
    }
    return results[n];
}
```

**Execução para N=6:**

| Índice | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|--------|---|---|---|---|---|---|---|
| Valor  | 0 | 1 | 1 | 2 | 3 | 5 | **8** |

Cada valor é calculado **uma única vez** e reutilizado. Complexidade: **O(n)**.

---

## 4. ⬆️ Bottom-up vs. Top-down

A PD utiliza uma abordagem **bottom-up**: resolve os subproblemas menores primeiro (a base) e vai construindo a solução dos subproblemas maiores a partir dos resultados já armazenados, como círculos concêntricos que crescem de dentro para fora.

Já a Divisão e Conquista (D&C) usa abordagem **top-down**: parte do problema maior e o divide recursivamente, sem necessariamente reaproveitar os subproblemas já resolvidos.

| Aspecto | PD (bottom-up) | D&C (top-down) |
|---|---|---|
| **Direção** | Do menor para o maior | Do maior para o menor |
| **Reaproveitamento** | Sim (tabela de memória) | Não (recalcula subproblemas) |
| **Uso ideal** | Subproblemas sobrepostos | Subproblemas independentes |

---

## 5. 📋 Algoritmo Geral da PD

O esqueleto de toda solução com PD segue os mesmos 5 passos:

```
1. Criar uma tabela de instâncias × valores;
2. Inicializar a primeira linha e a primeira coluna (casos base);
3. Para cada linha:
     Para cada coluna:
       Comparar o resultado anterior com o resultado ao incluir a instância atual;
       Armazenar o melhor resultado;
4. O resultado final estará na última célula da tabela;
5. Reconstruir o caminho da solução (backtracking) ou consultar tabela de objetos.
```

**Anatomia da tabela:**
- **Linhas:** instâncias/itens disponíveis (0, 1, 2, ..., N)
- **Colunas:** valores possíveis do subproblema (0, 1, 2, ..., capacidade máxima)
- **Célula T[i,j]:** a melhor solução usando apenas as instâncias 1..i com capacidade/alvo j

---

## 6. 🎒 Exemplo 1: Mochila 0-1

**Problema:** maximizar o valor total de itens em uma mochila com capacidade C. Cada item ou é levado inteiro (1) ou não é levado (0).

### 6.1 A Fórmula de Recorrência

```
T[i,j] = max(T[i-1, j], T[i-1, j - Pᵢ] + Vᵢ)
```

Onde:
- **T** — a matriz de memorização (dimensões N × C)
- **i** — índice do item atual (linha da tabela)
- **j** — capacidade limite da mochila neste subproblema (coluna da tabela)
- **Pᵢ** — peso do item i
- **Vᵢ** — valor do item i

**Dois cenários para cada célula:**

| Cenário | Fórmula | Significado |
|---|---|---|
| **A — Não incluir item i** | `T[i-1, j]` | Herdamos o melhor resultado já obtido com os itens anteriores na mesma capacidade. É uma "cópia" da célula diretamente acima. |
| **B — Incluir item i** | `T[i-1, j - Pᵢ] + Vᵢ` | "Abrimos espaço" (j - Pᵢ) na linha anterior e somamos o valor do item i. |

> Escolhemos sempre o **maior** dos dois cenários.

> ⚠️ **Caso especial:** se `j < Pᵢ` (item não cabe), o Cenário B é impossível — usamos apenas `T[i-1, j]`.

### 6.2 Preenchimento passo a passo

**Dados:** 3 itens, capacidade C = 50.

| Item | Peso | Valor |
|------|------|-------|
| 1    | 10   | 60    |
| 2    | 20   | 100   |
| 3    | 30   | 120   |

**Inicialização:** linha 0 (sem itens) = tudo 0; coluna 0 (capacidade 0) = tudo 0.

| Item\Cap | 0 | 10 | 20 | 30 | 40 | 50 |
|---|---|---|---|---|---|---|
| 0 (base) | 0 | 0 | 0 | 0 | 0 | 0 |
| 1 (P=10, V=60) | 0 | 60 | 60 | 60 | 60 | 60 |
| 2 (P=20, V=100) | 0 | 60 | 100 | 160 | 160 | 160 |
| 3 (P=30, V=120) | 0 | 60 | 100 | 160 | 180 | **220** |

**Exemplo de cálculo — T[2, 30]** (Item 2, capacidade 30):
- Cenário A: não incluir item 2 → `T[1, 30]` = 60
- Cenário B: incluir item 2 → `T[1, 30-20] + 100` = `T[1, 10] + 100` = 60 + 100 = **160**
- Resultado: `max(60, 160)` = **160** ✅

**Resultado final:** T[3, 50] = **220** — valor máximo alcançável.

> Este exemplo é exatamente o caso que o algoritmo **guloso falhou** (guloso V/P encontrou 160; PD encontra 220). A PD garante o ótimo global.

### 6.3 Reconstrução da solução (Backtracking)

Para descobrir *quais itens* compõem a solução, percorre-se a tabela de volta:

1. **Ponto de partida:** célula `T[N, C]` (canto inferior direito).
2. **Avaliação:** compare `T[i, j]` com `T[i-1, j]`:
   - Se **iguais**: item i **não** foi incluído → subir: `i ← i - 1`.
   - Se **diferentes**: item i **foi** incluído → adicionar à solução, reduzir capacidade (`j ← j - Pᵢ`) e subir (`i ← i - 1`).
3. **Parar** quando `i = 0` ou `j = 0`.

**Backtracking do exemplo:**
- `T[3,50]=220` vs `T[2,50]=160` → **diferentes** → **Item 3** incluído; j = 50−30 = 20, i = 2
- `T[2,20]=100` vs `T[1,20]=60` → **diferentes** → **Item 2** incluído; j = 20−20 = 0, i = 1
- j = 0 → parar

**Solução: Itens {2, 3}** — peso total 50, valor total **220** ✅

---

## 7. 🪙 Exemplo 2: Problema do Troco

**Problema:** dado um sistema monetário S com moedas pré-definidas, qual é a **menor quantidade de moedas** para retornar um troco C?

### 7.1 A Fórmula de Recorrência

```
C[I, J] = MIN(C[I-1, J], 1 + C[I, J - Vᵢ])
```

Onde:
- **C** — matriz de memorização (moedas × valores de troco)
- **I** — índice da moeda atual (linha)
- **J** — valor do troco parcial a alcançar (coluna)
- **C[I,J]** — número mínimo de moedas para dar troco J usando apenas as I primeiras moedas
- **Vᵢ** — valor da moeda i

**Dois cenários:**

| Cenário | Fórmula | Significado |
|---|---|---|
| **Não usar moeda I** | `C[I-1, J]` | Herda a melhor solução com as moedas anteriores para o mesmo troco J |
| **Usar moeda I** | `1 + C[I, J - Vᵢ]` | Conta 1 moeda (a atual) + o mínimo para fechar o troco restante (J - Vᵢ) |

> ⚠️ **Inicialização especial:** linha 0 (sem moedas) = **∞** para todos os J > 0 (impossível dar troco sem moedas); coluna 0 = **0** (troco zero não precisa de moedas). A PD usa ∞ aqui para que o MIN sempre ignore casos impossíveis.

> ⚠️ **Caso impossível:** se `J - Vᵢ < 0` (moeda é maior que o troco atual), a inclusão é impossível → usa-se `C[I-1, J]`.

### 7.2 Preenchimento passo a passo

**Dados:** S = {1, 2, 6, 8}, C = 12

**Linha da moeda 1 (V=1):** a única moeda disponível é "1", então para dar troco J basta usar J moedas de valor 1:

| Moeda\Troco | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 0 (base) | 0 | ∞ | ∞ | ∞ | ∞ | ∞ | ∞ | ∞ | ∞ | ∞ | ∞ | ∞ | ∞ |
| 1 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 |
| 2 | 0 | 1 | 1 | 2 | 2 | 3 | 3 | 4 | 4 | 5 | 5  | 6  | 6  |
| 6 | 0 | 1 | 1 | 2 | 2 | 3 | 1 | 2 | 2 | 3 | 2  | 3  | 2  |
| 8 | 0 | 1 | 1 | 2 | 2 | 3 | 1 | 2 | 1 | 2 | 2  | 3  | **2** |

**Resultado:** C[4, 12] = **2** moedas (uma de 6 + uma de 6, ou outra combinação ótima).

> **Comparação com guloso:** o algoritmo guloso clássico (sempre usar a maior moeda possível) daria 8+1+1+1+1 = 5 moedas para troco 12 com S={1,2,6,8}. A PD encontra 6+6 = **2 moedas** — resultado muito superior.

---

## 8. ⚖️ PD vs. Força Bruta vs. Guloso

| Aspecto | Força Bruta | Guloso | PD |
|---|---|---|---|
| **Estratégia** | Testa todas as combinações | Escolha local irrevogável a cada passo | Resolve subproblemas menores e combina os resultados |
| **Garantia de ótimo** | Sempre (por definição) | Apenas se houver greedy-choice property | **Sempre** (para problemas com subestrutura ótima) |
| **Complexidade** | Exponencial/fatorial | Geralmente O(n log n) | **Pseudopolinomial** (ex: O(N×C) na mochila) |
| **Memória** | Nenhuma necessária | Nenhuma | Requer a tabela inteira |
| **Revisita decisões** | N/A (testa tudo) | Nunca | Sim (avalia ambos os cenários) |
| **Quando usar** | Entradas minúsculas | Problema com greedy-choice | Problemas com sobreposição de subproblemas |

> **Conclusão:** o guloso é rápido mas pode falhar (Mochila 0-1, Troco com moedas arbitrárias). A PD é mais lenta e consome mais memória, mas **garante o ótimo** para qualquer instância do problema.

---

## 9. 🧩 Quando usar PD?

Um problema é bom candidato à PD quando possui **duas propriedades**:

1. **Subestrutura ótima:** a solução ótima do problema contém soluções ótimas dos subproblemas (essa propriedade é compartilhada com o guloso).
2. **Sobreposição de subproblemas:** os mesmos subproblemas aparecem repetidamente durante a resolução — sem memoização, seriam recalculados exponencialmente.

> Se o problema tem subestrutura ótima mas **não** tem sobreposição de subproblemas (os subproblemas são independentes), a D&C é mais adequada (ex: Merge Sort).

---

## 10. 📝 Resumo Mental

```
Programação Dinâmica funciona bem quando:
  ✅ O problema tem subestrutura ótima
  ✅ Há sobreposição de subproblemas (mesmos subproblemas recalculados)
  ✅ A solução pode ser construída incrementalmente bottom-up
  ✅ A memória extra (tabela) é viável (não explode o espaço disponível)

Vantagens sobre o Guloso:
  ✅ Garante o ótimo global sempre
  ✅ Avalia incluir E não incluir cada item (sem decisões irrevogáveis)
  ✅ Funciona para Mochila 0-1 e sistemas de moedas arbitrários

Custos da PD:
  ⚠️ Memória O(N×C) para a tabela
  ⚠️ Tempo O(N×C) — pseudopolinomial (depende do valor de C, não só de N)
  ⚠️ Projetar a recorrência correta exige cuidado
```

---

## 11. 🎯 Resumo Final

| Aspecto | Programação Dinâmica |
|---|---|
| **Ideia central** | Guardar resultados de subproblemas em uma tabela e reutilizá-los |
| **Pré-requisito** | Subestrutura ótima + Sobreposição de subproblemas |
| **Abordagem** | Bottom-up: resolver do menor para o maior |
| **Estrutura de dados** | Tabela/Matriz de dimensões instâncias × capacidade |
| **Fórmula geral** | Comparar "não incluir" (herdar linha anterior) vs. "incluir" (ir para coluna deslocada + ganho) |
| **Reconstrução** | Backtracking na tabela a partir do canto inferior direito |
| **Complexidade** | O(N×C) tempo e memória |
| **Problema da Mochila 0-1** | `max(T[i-1,j], T[i-1, j-Pᵢ] + Vᵢ)` |
| **Problema do Troco** | `min(C[I-1,J], 1 + C[I, J-Vᵢ])` — inicializa com ∞ |
| **Diferença para Guloso** | Não toma decisão irrevogável — garante o ótimo mesmo onde o guloso falha |
