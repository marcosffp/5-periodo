# Notação $O$, Comportamento Assintótico e Classes de Comportamento

---

## 1. Ideia Principal

Quando analisamos algoritmos, queremos saber: **à medida que a entrada cresce, o algoritmo fica quanto mais lento?** Para responder isso de forma matemática e comparável, usamos a notação $O$ — uma linguagem precisa para descrever o crescimento do custo de um algoritmo.

A ideia central é simples: para entradas grandes, o que importa não é o valor exato do custo, mas **como ele cresce**. Um algoritmo que faz $3n^2 + 10n$ operações e um que faz $n^2$ operações crescem da mesma forma — ambos são $O(n^2)$.

---

## 2. Conceitos Importantes

---

### O que é $f(n)$ e $g(n)$?

O material usa duas funções com papéis diferentes, e é importante não confundi-las:

- $g(n)$ é a **função de custo do algoritmo** — obtida analisando o código.
- $f(n)$ é a **função candidata** — aquela que queremos testar se domina $g(n)$.

Quando escrevemos $g(n) = O(f(n))$, estamos dizendo que $f(n)$ é um **teto** para o crescimento de $g(n)$.

---

### O que é comportamento assintótico?

É o comportamento de $g(n)$ quando $n$ **cresce muito**. Para valores pequenos de $n$, qualquer algoritmo é rápido — mesmo os ruins custam pouco. O que diferencia algoritmos é o que acontece para $n$ grande.

Por isso analisamos o comportamento assintótico: queremos saber a **tendência de crescimento**, ignorando constantes e termos menores.

---

### Definição formal da Notação $O$

$$g(n) = O(f(n)) \iff \exists\, c > 0 \text{ e } m \mid 0 \leq g(n) \leq c \cdot f(n), \;\forall n \geq m$$

Em português: $g(n) = O(f(n))$ se existem uma constante $c$ e um ponto de corte $m$ tal que, **a partir de $m$**, $c \cdot f(n)$ é sempre maior ou igual a $g(n)$.

Três elementos para entender:

- $c$ é uma **constante multiplicadora** — ela "estica" $f(n)$ o quanto for necessário.
- $m$ (ou $n_0$) é o **ponto de corte** — a partir daqui $c \cdot f(n)$ passa a dominar $g(n)$ para sempre.
- A condição precisa valer **para todo $n \geq m$**, não apenas para alguns valores.

---

## 3. Método Geral para Provar $g(n) = O(f(n))$

O objetivo é encontrar valores concretos de $c$ e $m$ que satisfaçam a definição. O processo é:

1. Escrever a desigualdade $g(n) \leq c \cdot f(n)$.
2. Dividir ambos os lados por $f(n)$ para isolar $c$.
3. Analisar o lado esquerdo quando $n \to \infty$.
4. Escolher um $c$ que satisfaça a desigualdade e um $m$ a partir do qual ela vale.

---

## 4. Explicação Detalhada dos Exemplos

---

### Exemplo 1 — $g(n) = 3n^2 + 10n$ e $f(n) = n^2$

**Pergunta:** $g(n) = O(n^2)$?

Queremos encontrar $c$ e $m$ tais que:

$$3n^2 + 10n \leq c \cdot n^2, \quad \forall n \geq m$$

**Passo 1 — dividir tudo por $n^2$:**

$$\frac{3n^2 + 10n}{n^2} \leq c$$

$$3 + \frac{10}{n} \leq c$$

**Passo 2 — analisar o comportamento quando $n$ cresce:**

O termo $\frac{10}{n}$ decresce conforme $n$ aumenta. Quando $n \to \infty$, ele tende a $0$, mas nunca chega a ser $0$ de verdade. Portanto o lado esquerdo tende a $3$ vindo de cima.

**Passo 3 — escolher $c$ e $m$:**

Temos duas estratégias:

- **Estratégia 1 (limite mínimo de $c$):** como o lado esquerdo tende a $3$, qualquer $c > 3$ funciona — mas precisaremos encontrar um $m$ maior para isso valer. Por exemplo, com $c = 4$, a desigualdade $3 + \frac{10}{n} \leq 4$ vale quando $\frac{10}{n} \leq 1$, ou seja, quando $n \geq 10$. Portanto $c = 4$ e $m = 10$ funcionam.

- **Estratégia 2 (prova mais rápida):** se escolhermos $m = 1$, então $\frac{10}{n} \leq 10$ para todo $n \geq 1$, então $3 + \frac{10}{n} \leq 13$. Basta tomar $c = 13$ e $m = 1$.

Verificando com $c = 4$ e $m = 16$ (como no slide):

| $n$ | $4 \cdot n^2$ | $3n^2 + 10n$ |
|---|---|---|
| 2 | 16 | 32 |
| 4 | 64 | 88 |
| 8 | 256 | 272 |
| 16 | 1024 | 928 ✓ |
| 32 | 4096 | 3392 ✓ |
| 64 | 16384 | 12928 ✓ |

A partir de $n = 16$, $4n^2$ passa a ser sempre maior que $g(n)$. Portanto:

$$g(n) = O(n^2) \quad \text{com } c = 4 \text{ e } m = 16$$

**Por que o $m$ existe?** Porque para valores pequenos de $n$, o termo $10n$ ainda tem peso relevante — ele "atrapalha" a dominância de $n^2$. Mas conforme $n$ cresce, $n^2$ cresce muito mais rápido que $n$, então eventualmente $c \cdot n^2$ ultrapassa $g(n)$ e nunca mais perde.

---

### Exemplo 2 — análise do `algZ` (Selection Sort)

```
void algZ(int[] array) {
    int tam = array.length;
    for (int pos = 0; pos < tam-1; pos++) {       // laço externo
        int menor = pos;
        for (int j = pos+1; j < tam; j++) {        // laço interno
            if (array[j] < array[menor])           // O(1)
                menor = j;                         // O(1)
        }
        int aux = array[menor];                    // O(1)
        array[menor] = array[pos];                 // O(1)
        array[pos] = aux;                          // O(1)
    }
}
```

**Passo 1 — identificar o custo das operações internas:**

As linhas 6 e 7 (a comparação e a atribuição) são cada uma $O(1)$. Juntas: $O(1) + O(1) = O(1)$.

**Passo 2 — custo do laço interno (linha 5):**

O laço interno executa o bloco $O(1)$ repetidamente. Mas quantas vezes? Depende do valor de `pos`:
- quando `pos = 0`: roda $n-1$ vezes
- quando `pos = 1`: roda $n-2$ vezes
- quando `pos = 2`: roda $n-3$ vezes
- $\vdots$

Então o laço interno executa $O(n-1) = O(n)$ vezes na primeira iteração do externo, menos nas seguintes. O custo do bloco interno + laço = $O(n) \cdot O(1) = O(n)$.

**Passo 3 — custo das linhas 9-11:**

São três operações constantes: $O(1) + O(1) + O(1) = O(1)$.

**Passo 4 — custo do corpo do laço externo:**

$O(n) + O(1) = O(n)$.

**Passo 5 — custo do laço externo (linha 3):**

O laço externo executa $n-1 = O(n)$ vezes, e a cada iteração o corpo custa $O(n)$. Pelo produto:

$$O(n) \cdot O(n) = O(n^2)$$

**Conclusão:**

$$\text{algZ} = O(n^2)$$

Faz sentido intuitivamente: é um laço dentro de um laço, cada um percorrendo o vetor — custo quadrático.

---

## 5. Propriedades da Notação $O$

Estas propriedades permitem calcular a complexidade de algoritmos combinados sem refazer a análise do zero:

| Propriedade | Significado |
|---|---|
| $f(n) = O(f(n))$ | qualquer função é $O$ de si mesma |
| $c \cdot O(f(n)) = O(f(n))$ | constante multiplicadora não muda a classe |
| $O(f(n)) + O(f(n)) = O(f(n))$ | somar duas partes iguais não muda a classe |
| $O(f(n)) + O(g(n)) = O(\max(f(n), g(n)))$ | a parte maior domina a soma |
| $O(f(n)) \cdot O(g(n)) = O(f(n) \cdot g(n))$ | laço dentro de laço multiplica |

A mais importante na prática é a da soma: **quando trechos de um algoritmo são executados em sequência, a complexidade total é a do trecho mais caro**.

Exemplo do slide: trechos de $O(n)$, $O(n^2)$ e $O(n)$ executados em sequência:

$$O(\max(n, n^2, n)) = O(n^2)$$

---

### Uma observação importante sobre o $O$

$O(n^2)$ é um **limite superior** — não necessariamente o mais justo. Se um algoritmo é $O(n^2)$, ele também é $O(n^3)$, $O(n^4)$, $O(2^n)$... porque todas essas funções também dominam $g(n)$.

Por isso buscamos sempre o **limite assintoticamente mais justo** — a menor $f(n)$ que ainda domina $g(n)$. No caso do `algZ`, $O(n^2)$ é o limite justo, não apenas $O(n^3)$.

---

## 6. Como Aplicar o Raciocínio em Outros Exercícios

O processo para provar $g(n) = O(f(n))$ é sempre o mesmo:

**Passo 1:** escreva $g(n) \leq c \cdot f(n)$.

**Passo 2:** divida ambos os lados por $f(n)$ para isolar $c$.

**Passo 3:** analise o resultado — ele diminui com $n$? Tende a uma constante? Cresce?

- Se **tende a uma constante $k$**: escolha $c = k + 1$ (ou qualquer valor acima) e encontre o $m$ correspondente. A prova funciona.
- Se **cresce indefinidamente**: $f(n)$ não domina $g(n)$, então $g(n) \neq O(f(n))$.

**Para análise de código**, o raciocínio é de dentro para fora:

1. Identifique a operação mais interna e seu custo ($O(1)$ na maioria dos casos).
2. Determine quantas vezes o laço mais interno executa essa operação.
3. Suba um nível e determine quantas vezes o laço externo executa o laço interno.
4. Multiplique os custos dos laços aninhados; some os custos dos trechos sequenciais.
5. Aplique a propriedade do $\max$ para a soma e a multiplicação para os laços.

---

## 7. Dicas para Resolver sem Precisar Decorar

**Dica 1 — a constante $c$ pode ser qualquer valor que funcione.** Não existe um $c$ único correto. Se você escolher $m = 1$ para simplificar, basta calcular o valor máximo que o lado esquerdo pode assumir com $n = 1$ e usar esse valor como $c$.

**Dica 2 — termos menores somem.** Em $3n^2 + 10n$, o $10n$ cresce mais devagar que $n^2$. Para $n$ grande, ele é irrelevante. A classe $O$ sempre será determinada pelo termo de maior grau.

**Dica 3 — laços aninhados multiplicam, trechos sequenciais somam (e a soma pega o maior).** Dois laços um dentro do outro viram $O(n) \cdot O(n) = O(n^2)$. Dois trechos um depois do outro viram $O(\max(n, n^2)) = O(n^2)$.

**Dica 4 — $O(n-1) = O(n)$.** Constantes aditivas e multiplicativas dentro do argumento não mudam a classe. $O(3n) = O(n)$, $O(n-1) = O(n)$, $O(2n^2) = O(n^2)$.

**Dica 5 — reconheça os padrões clássicos:**

| Estrutura do código | Complexidade |
|---|---|
| Operação única, sem laço | $O(1)$ |
| Um laço de $n$ iterações | $O(n)$ |
| Dois laços aninhados de $n$ | $O(n^2)$ |
| Laço que divide por 2 a cada iteração | $O(\log n)$ |
| Laço de $n$ com laço interno que divide por 2 | $O(n \log n)$ |