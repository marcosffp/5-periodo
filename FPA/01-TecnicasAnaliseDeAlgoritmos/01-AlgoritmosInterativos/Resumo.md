# Análise de Algoritmos — Resumo Completo

---

## Os 4 Passos para Analisar Qualquer Algoritmo

Antes de qualquer conta, sempre siga essa ordem:

1. **O que o algoritmo faz?** — entenda a lógica primeiro
2. **Marque as operações relevantes** — o que domina o custo
3. **Avalie sequências e laços** — conte quantas vezes cada coisa executa
4. **Há variação de casos?** — melhor, pior, médio

> Operações simples (atribuição, comparação, aritmética) custam **O(1)** cada.
> O custo de um laço = **(operações internas) × (número de repetições)**.

---

## Algoritmos Iterativos

---

### alg1 — Fatorial

```java
int alg1(int n){
    int res = 1;              // O(1) — ignora
    for(int i=n; i>1; i--){  // quantas vezes roda?
        res = res * i;        // 1 multiplicação + 1 atribuição = 2 operações
    }
    return res;               // O(1) — ignora
}
```

**Contando o laço:**
- `i` começa em `n` e para quando `i = 2`
- Executa para: n, n-1, n-2, ..., 2 → **(n-1) repetições**

**Montando:**
```
f(n) = (n-1) × 2 = 2n - 2
```

> **f(n) = 2n − 2**, para n > 1 → **O(n)**

---

### alg2 — Busca Linear (com variação de casos)

```java
int alg2(int[] arr, int x){
    for(int i=0; i<arr.length; i++){  // até n vezes
        if(arr[i] == x){              // 1 comparação
            return i;                 // 1 operação
        }
    }
    return -1;                        // 1 operação
}
```

**Por que tem variação?** O elemento `x` pode estar em qualquer posição — ou nem estar. O loop pode parar cedo ou percorrer tudo.

| Caso | Situação | f(n) | Notação |
|---|---|---|---|
| **Melhor** | x está na 1ª posição | 2 | O(1) |
| **Pior** | x não está (loop roda n vezes) | n + 1 | O(n) |
| **Médio** | (pior + melhor) / 2 | (n + 3) / 2 | O(n) |

---

### alg3 — Dois Laços Aninhados

```java
void alg3(int[] array){
    int quant = 0;
    int max = -1;
    for(int i=0; i<array.length; i++){        // laço externo: n vezes
        for(int j=0; j<array.length; j++){    // laço interno: n vezes
            if(array[i] < array[j])           // 1 operação
                quant++;                       // 2 operações
        }
        if(quant > max)                        // 1 operação
            max = quant;                       // 1 operação
        quant = 0;
    }
}
```

**Regra: analisa de dentro para fora.**

**Laço interno (j):** roda n vezes, custo por iteração = 3 (o if + quant++) → **3n**

**Bloco após laço interno:** if + atribuição = **2**

**Laço externo (i):** roda n vezes, cada iteração custa 3n + 2

```
f(n) = n × (3n + 2) = 3n² + 2n
```

> **f(n) = 3n² + 2n** → **O(n²)**

> ⚠️ Não há variação de casos porque o laço interno **sempre** roda n vezes, independente da entrada.

---

### alg4 — Selection Sort (laço interno variável)

```java
void alg4(int[] array){
    for(int pos=0; pos<array.length-1; pos++){    // laço externo: n-1 vezes
        int menor = pos;
        for(int j=pos+1; j<array.length; j++){    // laço interno: VARIA!
            if(array[j] < array[menor])           // 1 operação
                menor = j;
        }
        int aux = array[menor];                   // trocas: O(1)
        array[menor] = array[pos];
        array[pos] = aux;
    }
}
```

**O ponto chave:** o laço interno começa em `pos+1`, então a cada rodada ele faz **uma comparação a menos**.

**Exemplo com n = 5:**

| Rodada (pos) | j começa em | j vai até | Comparações |
|---|---|---|---|
| pos = 0 | j = 1 | j = 4 | 4 |
| pos = 1 | j = 2 | j = 4 | 3 |
| pos = 2 | j = 3 | j = 4 | 2 |
| pos = 3 | j = 4 | j = 4 | 1 |
| pos = 4 | j = 5 | — | 0 |

```
Total = 4 + 3 + 2 + 1 + 0 = 10
```

Visualmente:
```
pos=0  →  [j][j][j][j]  = 4 comparações
pos=1  →  [j][j][j]     = 3 comparações
pos=2  →  [j][j]        = 2 comparações
pos=3  →  [j]           = 1 comparação
pos=4  →               = 0 comparações
```

Isso é o **somatório de 0 até n-1**:

```
∑ i (i=0 até n-1) = (n-1) × n / 2 = (n² - n) / 2
```

Verificando com n = 5:
```
(5² - 5) / 2 = 20 / 2 = 10  ✅
```

> **f(n) = (n² - n) / 2** → **O(n²)**

---

### Comparativo: alg3 vs alg4

| | alg3 | alg4 |
|---|---|---|
| Laço externo | n vezes | n-1 vezes |
| Laço interno | sempre n vezes | diminui a cada rodada |
| Total de operações | n × n = n² | 0+1+2+...+(n-1) = n²/2 |
| Complexidade | **O(n²)** | **O(n²)** |

Os dois são O(n²), mas o alg4 faz **metade** das comparações na prática.

---

### Regra de Ouro para Laços

| Estrutura | Complexidade |
|---|---|
| 1 laço simples de n repetições | O(n) |
| 2 laços aninhados, ambos de n | O(n²) |
| 2 laços aninhados, interno variável | O(n²) — usa somatório |
| Laço que divide por 2 a cada iteração | O(log n) |

---

## Somatórios Úteis — Com Intuição

---

### 1) ∑ i = n(n+1)/2

**Intuição — Truque de Gauss:**

Soma de 1 até 100. Gauss percebeu que se você parear o primeiro com o último:
```
1   + 100 = 101
2   +  99 = 101
3   +  98 = 101
...
50  +  51 = 101
```
São **50 pares**, cada um valendo **101** → 50 × 101 = **5050**

Generalizando: n/2 pares, cada um valendo (n+1):
```
n/2 × (n+1) = n(n+1)/2
```

> **Intuição: agrupa os extremos aos pares — todos têm o mesmo valor.**

---

### 2) ∑ 2^k = 2^(k+1) - 1

**Intuição — Binário:**

Cada potência de 2 "liga" um bit:
```
2⁰ = 1  →  0001
2¹ = 2  →  0010
2² = 4  →  0100
2³ = 8  →  1000
soma   →  1111  (em binário)
```
`1111` em binário = `10000 - 1` = 2⁴ - 1 = **15** ✅

> **Intuição: somar todas as potências de 2 é ligar todos os bits — e isso equivale a 2^(k+1) - 1.**

---

### 3) ∑ 1/2^i = 2 - 1/2^k

**Intuição — Régua:**

Imagina uma régua de tamanho 2. Você vai preenchendo pela metade a cada passo:
```
Passo 1: preenche 1    → metade da régua preenchida
Passo 2: preenche 1/2  → 3/4 preenchida
Passo 3: preenche 1/4  → 7/8 preenchida
Passo 4: preenche 1/8  → 15/16 preenchida
```
Você **nunca chega em 2**, mas o que falta sempre é a última fração (1/2^k).
```
Soma = 2 - (o que falta) = 2 - 1/2^k
```

> **Intuição: cada passo preenche metade do que falta. O que sobra é sempre a última fração.**

---

### 4) ∑ a^i = (a^(k+1) - 1) / (a - 1)

**Intuição — Cancelamento por subtração:**

Chama a soma de S:
```
S   = 1 + a + a² + a³ + ... + aᵏ
a·S =     a + a² + a³ + ... + aᵏ + aᵏ⁺¹
```
Subtrai (S - a·S):
```
S - a·S = 1 - aᵏ⁺¹
S(1-a)  = 1 - aᵏ⁺¹
S       = (aᵏ⁺¹ - 1) / (a - 1)
```

Verificando com a=3, k=3:
```
1 + 3 + 9 + 27 = 40
(3⁴ - 1) / (3-1) = 80/2 = 40  ✅
```

> **Intuição: multiplica por "a", quase tudo cancela na subtração. Sobra só o primeiro e o último.**

---

### 5) ∑ i² = n(n+1)(2n+1)/6

**Intuição — Empilhar quadrados:**

```
n=1 → 1² = 1
n=2 → 1² + 2² = 5
n=3 → 1² + 2² + 3² = 14
```
Se você pegar **6 dessas pirâmides** de quadrados e encaixar, elas formam um bloco de dimensões n × (n+1) × (2n+1). Dividindo por 6 dá a fórmula.

Verificando com n=3:
```
1 + 4 + 9 = 14
3 × 4 × 7 / 6 = 84/6 = 14  ✅
```

> **Intuição: a demonstração formal usa o mesmo truque de Gauss, mas para quadrados.**

---

### Resumo dos Somatórios

| Fórmula | Resultado | Intuição |
|---|---|---|
| ∑ i (i=1 até n) | n(n+1)/2 | Pares de extremos (Gauss) |
| ∑ 2^k (i=0 até k) | 2^(k+1) - 1 | Ligar todos os bits em binário |
| ∑ 1/2^i (i=0 até k) | 2 - 1/2^k | Preencher régua pela metade |
| ∑ aⁱ (i=0 até k) | (a^(k+1)-1)/(a-1) | Multiplica por a, cancela tudo |
| ∑ i² (i=1 até n) | n(n+1)(2n+1)/6 | Empilhar quadrados em 3D |

---

## Resumo Geral dos Algoritmos

| Algoritmo | O que faz | f(n) exato | Complexidade |
|---|---|---|---|
| alg1 | Fatorial | 2n - 2 | O(n) |
| alg2 — melhor | Busca linear | 2 | O(1) |
| alg2 — médio | Busca linear | (n+3)/2 | O(n) |
| alg2 — pior | Busca linear | n + 1 | O(n) |
| alg3 | Contagem c/ 2 laços fixos | 3n² + 2n | O(n²) |
| alg4 | Selection Sort | (n²-n)/2 | O(n²) |