# Técnicas para Análise de Algoritmos — Guia Didático Completo

---

## 1. Ideia Principal

Analisar um algoritmo significa descobrir **quanto trabalho ele realiza** em função do tamanho da entrada. Em vez de medir em segundos (o que depende do hardware), usamos uma **função matemática f(n)** que conta operações relevantes. O objetivo final é classificar esse custo usando a **notação O** (Big-O), que descreve o comportamento do algoritmo conforme n cresce.

---

## 2. Conceitos Fundamentais

### O que é "complexidade de tempo"?
É a quantidade de operações que um algoritmo executa para processar uma entrada de tamanho n. Não interessa o valor exato — interessa **como esse número cresce**.

### Por que usar matemática discreta?
Porque contar operações exige manipular somas, produtos, fatoriais e resolver equações de recorrência — ferramentas da matemática discreta.

### A base: Aho, Hopcroft e Ullman (1983)
Eles estabeleceram princípios práticos que usam as **propriedades da notação O**. Não existe uma técnica universal — cada algoritmo é estudado como um caso particular, mas os mesmos princípios se aplicam sempre.

---

## 3. Os Três Princípios Básicos

### Princípio 1 — Operações Simples são O(1)
Leitura, escrita, atribuição, operações aritméticas e lógicas custam **tempo constante**. Não importa se n é 10 ou 1.000.000 — essas operações sempre custam o mesmo.

> **Exceção importante:** se uma operação dentro de uma atribuição chama uma função, o custo deixa de ser O(1) e herda o custo dessa função.

### Princípio 2 — Custo de uma Sequência
Se você tem uma sequência de operações, o custo total é:
- a **soma** dos custos individuais, ou
- dominado pelo **maior custo** entre eles (pela regra da notação O).

**Por que isso importa?** Porque O(n) + O(1) = O(n). O termo menor desaparece.

### Princípio 3 — Custo de Laços (loops)
> **Fórmula:** custo do corpo do laço × número de repetições

O ponto crítico é **calcular corretamente o número de repetições**. Esse é o erro mais comum.

---

## 4. Método Geral Para Analisar Qualquer Algoritmo

Siga sempre esta sequência de passos:

```
Passo 0 → Não entre em pânico.
Passo 1 → Entenda o que o algoritmo faz.
Passo 2 → Identifique as operações relevantes.
Passo 3 → Avalie sequências e laços (do mais interno para o externo).
Passo 4 → Verifique se há variação de casos (melhor, pior, médio).
```

---

## 5. Análise Detalhada dos Exemplos

---

### Exemplo 1 — `alg1`: Cálculo do Fatorial

```java
int alg1(int n) {
    int res = 1;                    // linha 2
    for(int i = n; i > 1; i--) {   // linha 3
        res = res * i;              // linha 4
    }
    return res;                     // linha 6
}
```

**Passo 1 — O que faz?**
Multiplica res por i enquanto i vai de n até 2. Isso é o cálculo de n! (fatorial de n).

**Passo 2 — Operações relevantes:**
A linha 4 executa uma atribuição e uma multiplicação. Pelo Princípio 1, isso custa **2 operações = custo 2**.

**Passo 3 — Quantas vezes o laço executa?**
O laço começa em `i = n` e termina quando `i = 1` (condição `i > 1`). Então:
- i percorre os valores: n, n-1, n-2, ..., 2
- Isso são exatamente **n - 1 iterações**

> **Por quê n-1 e não n?** Porque o laço para quando i chega em 1, sem executar o corpo mais uma vez. Teste com n=4: i assume 4, 3, 2 → 3 iterações = n-1 = 4-1 = 3. ✓

**Passo 4 — Calculando f(n):**

```
f(n) = (custo do corpo) × (número de repetições)
f(n) = 2 × (n - 1)
f(n) = 2n - 2,  para n > 1
```

**Classificação em Big-O:** O(n) — descartamos constantes e termos menores.

**Há variação de casos?** Não. O laço sempre executa n-1 vezes, independentemente dos valores. É um algoritmo com comportamento único.

---

### Exemplo 2 — `alg2`: Busca Linear (Linear Search)

```java
public int alg2(int[] arr, int x) {
    for(int i = 0; i < arr.length; i++) {   // linha 2
        if(arr[i] == x) {                    // linha 3
            return i;                        // linha 4
        }
    }
    return -1;                               // linha 7
}
```

**Passo 1 — O que faz?**
Procura o elemento x no array. Retorna o índice se encontrar, -1 se não encontrar.

**Passo 2 — Operações relevantes:**
A comparação `arr[i] == x` (linha 3) é a operação dominante. Os `return` custam O(1) cada.

**Passo 3 — O laço pode parar cedo!**
Aqui existe um `return` dentro do laço. Isso significa que o número de iterações **depende da entrada** — e por isso temos variação de casos.

**Passo 4 — Análise por casos:**

> Chamamos de n o tamanho do array (`arr.length`).

**Pior caso:** x não está no array, ou está no último elemento.
- O laço executa n vezes (comparação em cada iteração) + linha 7 executa uma vez
- f(n) = n × 1 + 1 = **n + 1** → O(n)

**Melhor caso:** x é o primeiro elemento (i = 0).
- O laço executa 1 vez (comparação) + return na linha 4
- f(n) = 1 × (1 + 1) = **2** → O(1)

**Caso médio:** média aritmética entre os extremos:
```
f(n) = (pior + melhor) / 2 = (n + 1 + 2) / 2 = (n + 3) / 2 → O(n)
```

> **Insight importante:** para análise assintótica, o que importa é o **pior caso**, pois dá a garantia máxima de tempo. Por isso dizemos que a busca linear é O(n).

---

### Exemplo 3 — `alg3`: Contagem com Dois Laços Aninhados

```java
void alg3(int[] array) {
    int quant = 0;                                    // linha 2
    int max = -1;                                     // linha 3
    for(int i = 0; i < array.length; i++) {           // linha 4
        for(int j = 0; j < array.length; j++) {       // linha 5
            if(array[i] < array[j])                   // linha 6
                quant++;                              // linha 7
        }
        if(quant > max)                               // linha 9
            max = quant;                              // linha 10
        quant = 0;                                    // linha 11
    }
}
```

**Passo 1 — O que faz?**
Para cada elemento i, conta quantos elementos do array são maiores que ele (linha 6-7). Depois guarda o máximo desses contadores. Ao final, max guarda o maior número de elementos que superam algum elemento do array.

**Passo 2 — Operações relevantes:**
- Linha 6: comparação → custo 1
- Linha 7: incremento → custo 2 (condicional: só executa se a linha 6 for verdadeira)
- Linha 9: comparação → custo 1
- Linha 10: atribuição → custo 1 (condicional)
- Linha 11: atribuição → custo 1

**Passo 3 — Análise de dentro para fora (regra fundamental para laços aninhados)**

> **Por que de dentro para fora?** Porque o custo do laço externo depende do custo do que está dentro dele.

**Camada mais interna (laço j + linhas 6-7):**
- O laço j executa **n vezes** (de 0 a n-1)
- Dentro dele, linha 6 sempre executa: custo 1
- Linha 7 é condicional — mas no **pior caso**, executa também: custo 1+2 = 3 (máximo com linha 6 incluída).
- Custo do bloco interno (laço j): n × 3 = **3n**

**Por que a linha 7 tem variação?**
A comparação `array[i] < array[j]` será **falsa quando i == j** (nenhum elemento é estritamente menor que ele mesmo). Então linha 7 executa no máximo **n-1 vezes** por iteração do laço i.

**Camada seguinte (linhas 9-11, dentro do laço i):**
- Linha 9 sempre executa: custo 1
- Linha 10 é condicional (depende da entrada): custo 0 ou 1
- Custo do bloco pós-laço-j: (0 ou 1) + 1 = **2 ou 1** → aproximamos por **1 no mínimo**

**Camada mais externa (laço i):**
- O laço i executa **n vezes**

```
f(n) = n × (3n + 2) = 3n² + 2n
```

**Mas a análise mais completa do slide mostra:**
Considerando todas as operações (comparações obrigatórias + atribuições condicionais no pior caso):

```
f(n) = 3n² + 2n → O(n²)
```

O coeficiente exato muda conforme contamos as operações, mas o **comportamento assintótico é sempre O(n²)**.

**Há variação de casos?**
Sim — mas apenas nos termos menores. As linhas 7 e 10 são condicionais:
- Linha 7: no máximo executada n-1 vezes por iteração do laço i
- Linha 10: depende de quant estar crescendo

Para o Big-O, isso não muda a classificação: O(n²) em todos os casos.

---

## 6. Como Aplicar o Raciocínio em Outros Exercícios

Dado qualquer algoritmo iterativo, siga este roteiro:

**1.** Identifique o bloco mais interno de laços e atribua seu custo.

**2.** Multiplique esse custo pelo número de repetições do laço que o envolve.

**3.** Repita subindo um nível de laço por vez.

**4.** Some os custos de blocos sequenciais.

**5.** Verifique se há desvios condicionais que criam variação de casos.

**Exemplo rápido:** se você vir um laço triplo aninhado, cada um de 0 até n:
```
f(n) = n × n × n = n³ → O(n³)
```

---

## 7. Dicas Para Resolver Sem Decorar

| Padrão encontrado | Complexidade resultante |
|---|---|
| Nenhum laço | O(1) |
| Um laço de 0 a n | O(n) |
| Laço que divide n por 2 a cada iteração | O(log n) |
| Dois laços aninhados, ambos até n | O(n²) |
| Três laços aninhados, todos até n | O(n³) |
| Laço externo até n, interno que divide por 2 | O(n log n) |

> **Regra de ouro da notação O:** ao somar ou multiplicar termos, **sempre vence o de maior crescimento**. O(n² + n) = O(n²). O(3n² + 100n + 50) = O(n²). Constantes e termos menores somem.

> **Sobre variação de casos:** só existe quando há um mecanismo que pode **encerrar o algoritmo antes do tempo** (como um `return` dentro de um laço) ou **pular operações** (como um `if` que pode deixar de executar blocos importantes). Se o laço sempre completa todas as iterações, não há variação.

> **Regra para contar iterações:** para um laço `for(i = a; i < b; i++)`, o número de iterações é sempre `b - a`. Para `for(i = n; i > 1; i--)`, é `n - 1`. Sempre teste com um valor pequeno concreto para confirmar.
