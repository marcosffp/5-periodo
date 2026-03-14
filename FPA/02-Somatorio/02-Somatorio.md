# Somatórios Úteis para Análise de Algoritmos — Guia Didático Completo

---

## 1. Ideia Principal

Quando analisamos algoritmos com laços, frequentemente precisamos calcular **a soma de uma sequência de valores**. Em vez de somar termo por termo (o que seria inviável para n grande), usamos **fórmulas fechadas** — expressões matemáticas que nos dão o resultado diretamente. Este conteúdo apresenta as cinco fórmulas mais úteis para análise de algoritmos, e mais importante: ensina **quando e como aplicar cada uma**.

---

## 2. Conceitos Fundamentais

### O que é um somatório?

Um somatório é uma forma compacta de escrever uma soma. A notação:

```
Σ(i=1 até n) i
```

significa: "some o valor de i para cada i de 1 até n", ou seja: 1 + 2 + 3 + ... + n.

### Por que precisamos de fórmulas fechadas?

Se n = 1.000.000, calcular 1+2+3+...+1.000.000 somando um a um levaria tempo. A fórmula fechada nos dá a resposta instantaneamente, e mais importante: nos permite classificar a complexidade do algoritmo em termos de Big-O.

### Como reconhecer qual fórmula usar?

Cada fórmula se aplica a um **tipo de sequência**. O segredo é identificar o padrão da sequência antes de escolher a fórmula. Veja abaixo.

---

## 3. As Cinco Fórmulas — Uma por Uma

---

### Fórmula 1 — Soma de inteiros consecutivos

```
Σ(i=1 até n) i  =  n(n+1)/2
```

**O que ela soma?** Os inteiros 1, 2, 3, 4, ..., n.

**De onde vem essa fórmula?** Esta é a famosa **fórmula de Gauss**. O raciocínio é elegante: imagine escrever a soma duas vezes, uma de frente para trás e outra de trás para frente, e somá-las coluna a coluna:

```
S  =  1   +  2   +  3   + ... + (n-1) + n
S  =  n   + (n-1)+ (n-2)+ ... +  2   + 1
──────────────────────────────────────────
2S = (n+1)+(n+1)+(n+1) + ... +(n+1)+(n+1)
     └────────────── n parcelas ───────────┘
2S = n(n+1)
S  = n(n+1)/2
```

Cada par de termos soma sempre (n+1), e há exatamente n pares, então 2S = n(n+1).

**Exemplo concreto:** Σ(i=1 até 5) i = 1+2+3+4+5 = 15. Pela fórmula: 5×6/2 = 15. ✓

**Quando aparece em algoritmos?** Sempre que um laço interno começa em `i+1` e vai até `n`, gerando a sequência n-1, n-2, ..., 1 (como no Selection Sort).

**Classificação Big-O:** n(n+1)/2 ≈ n²/2 → **O(n²)**

---

### Fórmula 2 — Soma de potências de 2

```
Σ(i=0 até k) 2^i = 2^(k+1) - 1
```

---

**O que essa soma representa**

A expressão

```
Σ(i=0 até k) 2^i
```

significa somar todas as potências de 2 começando em (2^0) até (2^k).

Ou seja:

```
2^0 + 2^1 + 2^2 + 2^3 + ... + 2^k
```

Sabendo que:

```
2^0 = 1
2^1 = 2
2^2 = 4
2^3 = 8
```

a soma fica:

```
1 + 2 + 4 + 8 + ... + 2^k
```

Cada termo é **o dobro do anterior**.

---

**Ideia da demonstração**

Queremos descobrir o valor dessa soma **sem somar termo por termo**.

Para isso damos um nome para a soma:

```
S = 1 + 2 + 4 + 8 + ... + 2^k
```

Nosso objetivo é descobrir **quanto vale S**.

---

**Criando uma segunda equação**

Multiplicamos toda a soma por 2:

```
2S = 2 + 4 + 8 + ... + 2^(k+1)
```

Agora temos duas equações:

```
S  = 1 + 2 + 4 + 8 + ... + 2^k
2S =     2 + 4 + 8 + ... + 2^k + 2^(k+1)
```

Observe que **quase todos os termos são iguais**.

---

**Por que fazemos a subtração**

Queremos eliminar os termos repetidos do meio.

A operação que cancela termos iguais é **subtração**.

Então fazemos:

```
2S − S
```

No lado esquerdo:

```
2S − S = S
```

Ou seja, conseguimos **isolar S**, que é o que queremos descobrir.

---

**Subtraindo as somas**

Agora subtraímos as duas expressões:

```
2 + 4 + 8 + ... + 2^(k+1)
-
1 + 2 + 4 + ... + 2^k
```

Os termos iguais cancelam:

```
2 − 2
4 − 4
8 − 8
...
2^k − 2^k
```

Tudo desaparece.

Sobra apenas:

```
2^(k+1) − 1
```

---

**Resultado final**

Como

```
2S − S = S
```

então:

```
S = 2^(k+1) − 1
```

Logo:

```
Σ(i=0 até k) 2^i = 2^(k+1) − 1
```

---

**Exemplo concreto**

Para (k = 3):

```
2^0 + 2^1 + 2^2 + 2^3
```

Calculando:

```
1 + 2 + 4 + 8 = 15
```

Usando a fórmula:

```
2^(k+1) − 1
2^4 − 1
16 − 1 = 15
```

Resultado igual.

**Classificação em análise de algoritmos**

A expressão:

```
2^(k+1) − 1
```

é dominada pelo termo exponencial.

Portanto:

```
O(2^k)
```

Classificação: **crescimento exponencial**.

---

### Fórmula 3 — Soma de potências de $\frac{1}{2}$

$$\sum_{i=0}^{k} \left(\frac{1}{2}\right)^i = 2 - \frac{1}{2^k}$$

---

#### O que essa soma representa

A expressão $\sum_{i=0}^{k} \left(\frac{1}{2}\right)^i$ significa somar:

$$1 + \frac{1}{2} + \frac{1}{4} + \frac{1}{8} + \dots + \frac{1}{2^k}$$

Cada termo é metade do anterior. Exemplo dos primeiros termos:

$$1,\; \frac{1}{2},\; \frac{1}{4},\; \frac{1}{8},\; \frac{1}{16},\; \dots$$

---

#### Damos um nome para a soma

Chamamos a soma de $S$:

$$S = 1 + \frac{1}{2} + \frac{1}{4} + \frac{1}{8} + \dots + \frac{1}{2^k}$$

Nosso objetivo é descobrir quanto vale $S$.

---

#### Criamos uma segunda equação

Multiplicamos toda a soma por $\frac{1}{2}$. Isso desloca todos os termos:

$$\frac{S}{2} = \frac{1}{2} + \frac{1}{4} + \frac{1}{8} + \dots + \frac{1}{2^{k+1}}$$

Agora temos duas equações:

$$S = 1 + \frac{1}{2} + \frac{1}{4} + \frac{1}{8} + \dots + \frac{1}{2^k}$$

$$\frac{S}{2} = \frac{1}{2} + \frac{1}{4} + \frac{1}{8} + \dots + \frac{1}{2^k} + \frac{1}{2^{k+1}}$$

Observe que quase todos os termos são iguais.

---

#### Por que fazemos a subtração

Queremos cancelar os termos iguais do meio. Então fazemos $S - \frac{S}{2}$.

No lado esquerdo:

$$S - \frac{S}{2} = \frac{S}{2}$$

---

#### Subtraindo as somas

Subtraímos as duas expressões:

$$\left(1 + \frac{1}{2} + \frac{1}{4} + \dots + \frac{1}{2^k}\right) - \left(\frac{1}{2} + \frac{1}{4} + \dots + \frac{1}{2^{k+1}}\right)$$

Cancelamentos:

- $\frac{1}{2} - \frac{1}{2}$
- $\frac{1}{4} - \frac{1}{4}$
- $\frac{1}{8} - \frac{1}{8}$
- $\vdots$
- $\frac{1}{2^k} - \frac{1}{2^k}$

Tudo cancela. Sobra apenas:

$$1 - \frac{1}{2^{k+1}}$$

Então:

$$\frac{S}{2} = 1 - \frac{1}{2^{k+1}}$$

Multiplicando por 2:

$$S = 2 - \frac{1}{2^k}$$

---

#### Resultado final

$$\sum_{i=0}^{k} \left(\frac{1}{2}\right)^i = 2 - \frac{1}{2^k}$$

---

#### Verificando com valores pequenos

**$k = 0$**
$$1 \quad\Rightarrow\quad \text{Fórmula: } 2 - 1 = 1 \checkmark$$

**$k = 1$**
$$1 + \frac{1}{2} = 1{,}5 \quad\Rightarrow\quad \text{Fórmula: } 2 - \frac{1}{2} = 1{,}5 \checkmark$$

**$k = 2$**
$$1 + \frac{1}{2} + \frac{1}{4} = 1{,}75 \quad\Rightarrow\quad \text{Fórmula: } 2 - \frac{1}{4} = 1{,}75 \checkmark$$

---

#### Intuição importante

Cada termo adiciona metade do anterior:

$$1 + 0{,}5 + 0{,}25 + 0{,}125 + \dots$$

A soma vai crescendo e se aproxima de 2, mas nunca passa de 2:

```
1
1.5
1.75
1.875
1.9375
...
```

Quando $k \to \infty$:

$$\frac{1}{2^k} \to 0 \quad\Rightarrow\quad S \to 2$$

---

#### Onde aparece em algoritmos

Essa soma aparece quando o trabalho diminui pela metade a cada nível. Exemplo típico: Merge Sort. O custo por nível da recursão pode ser:

```
n + n/2 + n/4 + n/8 + ...
```

Fatorando $n$:

```
n · (1 + 1/2 + 1/4 + ...)
```

Essa soma é limitada por 2, então o custo total é $\leq 2n$.

---

#### Classificação em análise de algoritmos

A soma $1 + \frac{1}{2} + \frac{1}{4} + \dots$ é limitada por uma constante (2). Portanto, em relação a $k$:

$$O(1)$$

Ela não cresce indefinidamente.

---

### Fórmula 4 — Soma de potências de uma base qualquer (série geométrica geral)

```
Σ(i=0 até k) a^i  =  (a^(k+1) - 1) / (a - 1),  para a ≠ 1
```

**O que ela soma?** 1, a, a², a³, ..., a^k — uma progressão geométrica com razão `a`.

**De onde vem?** Existe uma derivação clássica. Chame S a soma, multiplique por `a`, e subtraia:

```
S  = 1  + a  + a² + ... + a^k
aS = a  + a² + a³ + ... + a^(k+1)

aS - S = a^(k+1) - 1
S(a-1) = a^(k+1) - 1
S      = (a^(k+1) - 1) / (a-1)
```

**Verificação:** Para a=2, k=3 → (2⁴-1)/(2-1) = 15/1 = 15. Isso é igual à Fórmula 2 com k=3! ✓ A Fórmula 2 é apenas um caso especial desta, com a=2.

**Quando aparece em algoritmos?** Em análise de algoritmos recursivos onde cada chamada gera `a` subchamadas, ou em estruturas de dados como árvores de grau `a`. Também aparece quando o tamanho do problema se multiplica por uma constante a cada iteração.

**Classificação Big-O:** Depende de `a`. Se a > 1, cresce como **O(a^k)**. Se a < 1, converge para uma constante.

---

### Fórmula 5 — Soma de quadrados

```
Σ(i=1 até n) i²  =  n(n+1)(2n+1)/6
```

**O que ela soma?** Os quadrados: 1, 4, 9, 16, 25, ..., n².

**Intuição geométrica:** Imagine empilhar quadrados de lado 1×1, 2×2, 3×3, ..., n×n. A soma dos volumes desses cubos pode ser visualizada como um sólido que, com a fórmula certa, resulta nessa expressão. A derivação formal usa indução matemática ou manipulação de somas telescópicas.

**Verificação:** n=3 → 1+4+9 = 14. Fórmula: 3×4×7/6 = 84/6 = 14. ✓

**Quando aparece em algoritmos?** Em análises mais avançadas, como certos algoritmos de grafos ou quando o custo do laço interno cresce quadraticamente. É menos comum nos exemplos básicos, mas essencial quando aparecer.

**Classificação Big-O:** n(n+1)(2n+1)/6 ≈ n³/3 → **O(n³)**

---

## 4. Tabela Resumo — Como Reconhecer Cada Fórmula

| O que você vê na sequência | Tipo | Fórmula | Big-O |
|---|---|---|---|
| 1, 2, 3, ..., n | Aritmética | n(n+1)/2 | O(n²) |
| 1, 2, 4, 8, ..., 2^k | Geométrica, base 2 | 2^(k+1)-1 | O(2^k) |
| 1, 1/2, 1/4, ..., 1/2^k | Geométrica, razão 1/2 | 2 - 1/2^k | O(1) |
| 1, a, a², ..., a^k | Geométrica, base a | (a^(k+1)-1)/(a-1) | O(a^k) |
| 1, 4, 9, ..., n² | Quadrática | n(n+1)(2n+1)/6 | O(n³) |

---

## 5. Como Aplicar em Exercícios — Método Passo a Passo

Dado um algoritmo com laços, siga este roteiro:

**Passo 1:** Escreva a sequência de iterações do laço problemático para valores concretos de n pequeno (n=4 ou n=5).

**Passo 2:** Identifique o padrão — a sequência é crescente linear? Potências de 2? Quadrática?

**Passo 3:** Monte o somatório formal com os limites corretos.

**Passo 4:** Aplique a fórmula correspondente.

**Passo 5:** Simplifique algebricamente e extraia o Big-O.

**Exemplo de aplicação rápida:**
```
Um laço onde o trabalho na iteração i é 2^i, para i de 0 até k:
→ Reconheço: série geométrica base 2
→ Aplico: Σ 2^i = 2^(k+1) - 1
→ Resultado: O(2^k)
```

---

## 6. Dicas Para Nunca Esquecer

**Dica 1 — Fórmula de Gauss: pense em pares.** Se você esquecer n(n+1)/2, recrie: some o primeiro com o último (1+n), o segundo com o penúltimo (2+(n-1))... todos os pares dão (n+1), e há n/2 pares. Resultado: n(n+1)/2.

**Dica 2 — Série geométrica: multiplica e subtrai.** Se você esquecer a fórmula da série geométrica, recrie: chame a soma de S, multiplique por `a`, subtraia S de aS. Quase tudo cancela, e você isola S.

**Dica 3 — Verifique com n pequeno.** Sempre teste a fórmula com n=3 ou n=4 antes de usar em uma prova. Se a soma manual e a fórmula derem o mesmo resultado, você está no caminho certo.

**Dica 4 — O Big-O vem do termo de maior grau.** Não precisa simplificar tudo: n(n+1)/2 = n²/2 + n/2. O maior grau é n² — então é O(n²). Rápido e direto.

**Dica 5 — Limite inferior vs. superior.** Σ(i=1 até n) i começa em 1. Σ(i=0 até n-1) i começa em 0 — mas o zero não contribui, então é a mesma soma. Cuidado com os limites ao adaptar as fórmulas.
