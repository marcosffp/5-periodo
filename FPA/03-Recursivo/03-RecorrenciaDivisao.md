# Equações de Recorrência com Divisão — Método Formal Passo a Passo

---

## 1. Ideia Principal

Até agora, analisávamos algoritmos recursivos expandindo manualmente e identificando um padrão intuitivo. Esse método funciona, mas **fica perigoso com recorrências mais complexas** — especialmente quando o problema se divide por 3, por 4, ou tem múltiplas chamadas com coeficientes. O material apresenta um **método formal e sistemático** com cinco passos bem definidos (A, B, C, D, E) que funciona para qualquer recorrência de divisão. A novidade principal é a **substituição de variável**: trocamos n por 3^i (ou 2^i) para transformar a recorrência em um somatório que sabemos resolver.

---

## 2. Conceitos Importantes

**Equação de recorrência** é a forma matemática de descrever o custo de um algoritmo recursivo. Ela tem sempre dois pedaços:

$$T(n) = \begin{cases} \text{custo base}, & n = \text{caso base} \\ \text{custo recursivo} + \text{trabalho local}, & \text{caso contrário} \end{cases}$$

**Por que formalizar?** Porque o método intuitivo (expansão livre + identificar padrão + F.G.) funciona bem para recorrências simples, mas é lento e sujeito a erros quando a divisão não é por 2, quando há múltiplos termos ou quando o somatório resultante não é óbvio. O método formal divide o trabalho em etapas claras.

**Série geométrica** — uma ferramenta matemática que precisaremos no final. A fórmula essencial é:

$$\sum_{j=0}^{k} a^j = \frac{a^{k+1} - 1}{a - 1} \quad (a \neq 1)$$

Isso é o que nos permite fechar o somatório que surge na expansão.

---

## 3. Método Formal em Cinco Passos

| Passo | O que fazer |
|-------|------------|
| A | Expandir a recorrência alguns níveis para ver as substituições |
| B | Identificar o padrão geral (Fórmula Geral em termos de i) |
| C | Determinar o limite — quando a expansão para (encontrar i) |
| D | Substituir n = base^i para reescrever tudo em função de i |
| E | Resolver o somatório usando a fórmula da série geométrica |

---

## 4. Explicação Detalhada do Exemplo Principal

### O algoritmo analisado:

```
void pesquisa(n) {
    if (n <= 1)
        'inspecione elemento' e termine       // base
    else {
        para cada um dos n elementos 'inspecione';  // custo: n
        pesquisa(n/3);                              // recursão: divide por 3
    }
}
```

---

### Montando a equação de recorrência

**Base:** quando n ≤ 1, o algoritmo só inspeciona 1 elemento. Custo = 1.
> T(1) = 1

**Caso recursivo:** inspeciona todos os n elementos (custo n), depois chama `pesquisa(n/3)` (custo T(n/3)).
> T(n) = T(n/3) + n

**Equação completa:**

$$T(n) = \begin{cases} 1, & n = 1 \\ T\!\left(\frac{n}{3}\right) + n, & n > 1 \end{cases}$$

---

### PASSO A — Expansão da recorrência

Vamos expandir substituindo o argumento de T pelo que ele vale, progressivamente:

Partimos de T(n) = T(n/3) + n.

Agora T(n/3) também segue a mesma regra: T(n/3) = T(n/9) + n/3. Do mesmo modo, T(n/9) = T(n/27) + n/9, e assim por diante:

$$T\!\left(\frac{n}{3}\right) = T\!\left(\frac{n}{9}\right) + \frac{n}{3}$$

$$T\!\left(\frac{n}{9}\right) = T\!\left(\frac{n}{27}\right) + \frac{n}{9}$$

$$T\!\left(\frac{n}{27}\right) = T\!\left(\frac{n}{81}\right) + \frac{n}{27}$$

$$\vdots$$

$$T(1) = 1$$

**Por que fazemos isso?** Para enxergar como o argumento de T vai diminuindo (n → n/3 → n/9 → n/27...) e como o termo adicional também vai encolhendo (n → n/3 → n/9...). Essa estrutura nos revela o padrão.

---

### PASSO B — Identificar o padrão

Olhando as linhas acima, vemos que cada linha tem a forma:

$$T\!\left(\frac{n}{3^i}\right) = T\!\left(\frac{n}{3^{i+1}}\right) + \frac{n}{3^i}$$

**Interpretação:** após i substituições, o argumento é n/3^i e o termo adicionado é n/3^i. Isso é a Fórmula Geral do padrão.

---

### PASSO C — Determinar o limite (quando para?)

Paramos quando chegamos à base, ou seja, quando o argumento de T se iguala a 1:

$$\frac{n}{3^i} = 1 \implies n = 3^i \implies \mathbf{i = \log_3(n)}$$

**Por que igualamos a 1 e não a 0?** Porque a base desta recorrência é T(1) = 1, não T(0). Sempre igualamos ao valor da base da recorrência.

---

### PASSO D — Reescrever em função do novo N

Sabemos que n = 3^i. Vamos substituir isso para trabalhar apenas com potências de 3, tornando a álgebra mais limpa. Reescrevemos as linhas da expansão com n = 3^i:

$$T(3^i) = T(3^{i-1}) + 3^i$$
$$T(3^{i-1}) = T(3^{i-2}) + 3^{i-1}$$
$$T(3^{i-2}) = T(3^{i-3}) + 3^{i-2}$$
$$\vdots$$
$$T(3^{i-(i-1)}) = T(3^{i-i}) + 3^{i-(i-1)}$$
$$T(3^0) = 1$$

Agora substituímos progressivamente de baixo para cima — pegamos o valor de T de cada linha e jogamos na linha anterior, até montar T(3^i) como uma soma:

$$T(3^i) = 3^i + 3^{i-1} + 3^{i-2} + \cdots + 3^1 + 3^0 = \sum_{j=0}^{i} 3^j$$

**Por que a soma vai de j=0 até j=i?** Porque ao substituir cada linha na anterior, os termos adicionados são 3^i, 3^(i-1), ..., 3^1, e a base contribui com T(3^0) = 1 = 3^0. Todos esses termos se acumulam.

---

### PASSO E — Resolver o somatório

Agora temos um somatório de progressão geométrica com razão a = 3:

$$T(3^i) = \sum_{j=0}^{i} 3^j = \frac{3^{i+1} - 1}{3 - 1} = \frac{3^{i+1} - 1}{2}$$

Substituímos i = log₃(n), lembrando que 3^(log₃(n)) = n por definição de logaritmo:

$$T(n) = \frac{3^{\log_3(n)+1} - 1}{2} = \frac{3 \cdot 3^{\log_3(n)} - 1}{2} = \frac{3n - 1}{2}$$

**Resultado final:**

$$\boxed{T(n) = \frac{3n-1}{2} \implies \mathbf{O(n)}}$$

**Intuição:** o algoritmo inspeciona n elementos, depois n/3, depois n/9... A soma n + n/3 + n/9 + ... é uma série geométrica que converge para ≈ 3n/2, confirmando O(n).

---

## 5. Como Aplicar o Raciocínio nos Exercícios

### Exercício a)

$$T(n) = \begin{cases} 1, & n = 1 \\ T\!\left(\frac{n}{2}\right) + 1, & n \geq 2 \end{cases}$$

**Passo A — Expandir:**

$$T(n) = T(n/2) + 1$$
$$T(n/2) = T(n/4) + 1$$
$$T(n/4) = T(n/8) + 1$$

**Passo B — Padrão:**

$$T(n/2^i) = T(n/2^{i+1}) + 1$$

**Passo C — Limite:**

$$n/2^i = 1 \implies n = 2^i \implies i = \log_2(n)$$

**Passo D — Substituir n = 2^i:**

$$T(2^i) = T(2^{i-1}) + 1$$
$$T(2^{i-1}) = T(2^{i-2}) + 1$$
$$\vdots$$
$$T(2^0) = 1$$

Somando todas as linhas: T(2^i) = 1 + 1 + 1 + ... i vezes + 1 (base) = i + 1

$$T(2^i) = i + 1$$

**Passo E — Resolver:**

Como i = log₂(n):

$$T(n) = \log_2(n) + 1 \implies \mathbf{O(\log n)}$$

**Intuição:** a cada chamada não há trabalho extra (apenas +1). O algoritmo só faz log₂(n) chamadas no total.

---

### Exercício b)

$$T(n) = \begin{cases} 0, & n = 1 \\ 2T\!\left(\frac{n}{2}\right) + n, & n \geq 2 \end{cases}$$

**Passo A — Expandir:**

$$T(n) = 2T(n/2) + n$$
$$T(n/2) = 2T(n/4) + n/2$$
$$T(n/4) = 2T(n/8) + n/4$$

**Passo C — Limite:**

$$n/2^i = 1 \implies i = \log_2(n)$$

**Passo D — Substituir n = 2^i:**

$$T(2^i) = 2T(2^{i-1}) + 2^i$$
$$T(2^{i-1}) = 2T(2^{i-2}) + 2^{i-1}$$
$$\vdots$$
$$T(2^0) = 0$$

Substituindo de baixo para cima, cada linha dobra o coeficiente do T e soma um novo 2^i:

$$T(2^i) = i \cdot 2^i$$

**Por que i · 2^i?** Porque a cada nível, o trabalho é 2^i (sempre n), e há i níveis antes da base. Isso é uma soma do tipo 2^i + 2^i + ... i vezes = i · 2^i.

**Passo E — Substituir i = log₂(n):**

$$T(n) = \log_2(n) \cdot n \implies \mathbf{O(n \log n)}$$

**Intuição:** este é exatamente o padrão do mergesort — duas chamadas recursivas, cada uma com n/2 elementos, mais trabalho linear n por nível, com log₂(n) níveis.

---

## 6. Dicas para Resolver sem Precisar Decorar

**Dica 1 — Os cinco passos são uma receita universal.** A, B, C, D, E. Não importa se divide por 2, por 3 ou por 4 — o processo é sempre o mesmo.

**Dica 2 — A substituição n = base^i é o coração do método.** Se divide por 3, faça n = 3^i. Se divide por 2, faça n = 2^i. Isso transforma a recorrência em um somatório limpo.

**Dica 3 — Ao somar todas as linhas da expansão, os T intermediários cancelam.** Isso acontece porque cada T(3^(i-1)) aparece como resultado em uma linha e como argumento na linha seguinte — eles se cancelam em pares, sobra apenas T(3^i) do lado esquerdo e a soma dos termos adicionais do lado direito.

**Dica 4 — A série geométrica é a ferramenta final.** Sempre que você ver uma soma do tipo a^0 + a^1 + ... + a^k, use a fórmula (a^(k+1) - 1)/(a - 1). Depois substitua k = log_base(n) para voltar em termos de n.

**Dica 5 — Verifique a complexidade com a intuição.** Divisão por qualquer constante + trabalho constante por nível → O(log n). Divisão por b + trabalho n por nível → O(n). Duas chamadas + trabalho n por nível → O(n log n). Se seu resultado não bate com essa tabela, revise os passos.