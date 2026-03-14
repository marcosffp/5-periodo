# Logaritmos — Do Zero à Lógica

---

## 1. O que é logaritmo?

Tudo começa com potenciação. Você já sabe que:

$$2^3 = 8$$

Isso diz: **base 2, expoente 3, resultado 8**.

Agora imagine que alguém te dá a base e o resultado, e quer saber o expoente:

> "Que expoente devo colocar na base 2 para obter 8?"

Essa pergunta é exatamente o que o logaritmo responde:

$$\log_2 8 = 3$$

As duas expressões dizem a **mesma coisa**, só de ângulos diferentes:

$$2^3 = 8 \iff \log_2 8 = 3$$

De forma geral:

$$a^x = n \iff \log_a n = x$$

**Logaritmo é o expoente.** Toda a lógica das propriedades vem disso.

---

## 2. Como ler um logaritmo

$$\log_{\underbrace{2}_{\text{base}}} \underbrace{8}_{\text{resultado}} = \underbrace{3}_{\text{expoente}}$$

A base fica embaixo, o resultado fica ao lado, e o logaritmo te diz o expoente.

---

## 3. Propriedades — todas pela lógica

---

### Propriedade 1 — $a^{\log_a n} = n$

$\log_a n$ é o expoente que colocamos em $a$ para obter $n$. Então se elevarmos $a$ a esse expoente, o resultado é $n$ — por definição:

$$a^{\text{(expoente que dá } n)} = n$$

**Exemplo:**

$$3^{\log_3 27} = 27$$

porque $\log_3 27 = 3$ e $3^3 = 27$.

**Macete:** base e logaritmo com a mesma base se cancelam, sobrando só $n$.

---

### Propriedade 2 — $\log_a a = 1$

Pergunta: que expoente coloco em $a$ para obter $a$?

$$a^1 = a$$

Resposta óbvia: expoente $1$.

$$\log_a a = 1$$

**Exemplos:** $\log_2 2 = 1$, $\log_3 3 = 1$, $\log_{10} 10 = 1$.

---

### Propriedade 3 — $\log_a 1 = 0$

Pergunta: que expoente coloco em $a$ para obter $1$?

$$a^0 = 1$$

Qualquer base elevada a $0$ é $1$.

$$\log_a 1 = 0$$

---

### Propriedade 4 — $\log_a (x \cdot y) = \log_a x + \log_a y$

A lógica vem da potenciação. Você sabe que multiplicar potências de mesma base **soma os expoentes**:

$$2^3 \cdot 2^4 = 2^{3+4} = 2^7$$

Como logaritmo é expoente, multiplicar dois números equivale a somar seus logaritmos:

$$\log_2 (8 \cdot 16) = \log_2 8 + \log_2 16 = 3 + 4 = 7$$

Verificando: $8 \cdot 16 = 128 = 2^7$ ✓

---

### Propriedade 5 — $\log_a (x / y) = \log_a x - \log_a y$

Mesma lógica, mas agora com divisão. Dividir potências de mesma base **subtrai os expoentes**:

$$\frac{2^5}{2^3} = 2^{5-3} = 2^2$$

Então:

$$\log_2 (32 / 4) = \log_2 32 - \log_2 4 = 5 - 2 = 3$$

Verificando: $32/4 = 8 = 2^3$ ✓

---

### Propriedade 6 — $\log_a (x^k) = k \cdot \log_a x$

$x^k$ é $x$ multiplicado por si mesmo $k$ vezes. Pela propriedade 4, isso vira soma de logaritmos:

$$\log(x \cdot x \cdot x \cdots) = \underbrace{\log x + \log x + \log x + \cdots}_{k \text{ vezes}} = k \cdot \log x$$

**Exemplo:**

$$\log_2 8 = \log_2 2^3 = 3 \cdot \log_2 2 = 3 \cdot 1 = 3 \checkmark$$

**Uso importante:** quando você tem $2^k = n$ e quer isolar $k$, aplica $\log_2$ dos dois lados:

$$\log_2 2^k = \log_2 n \implies k \cdot \underbrace{\log_2 2}_{=1} = \log_2 n \implies k = \log_2 n$$

---

### Propriedade 7 — Mudança de base

$$\log_a n = \frac{\log_b n}{\log_b a}$$

**A lógica:** logaritmo é expoente numa certa base. Quando você muda de base, está convertendo esse expoente para outra escala. A divisão faz essa conversão.

**Exemplo:** calcular $\log_3 9$ usando base 2:

$$\log_3 9 = \frac{\log_2 9}{\log_2 3} = \frac{3.17}{1.58} \approx 2 \checkmark$$

**Por que isso importa em algoritmos?** Porque $\log_2 n$ e $\log_3 n$ diferem só por uma constante:

$$\log_3 n = \frac{\log_2 n}{\log_2 3} \approx \frac{\log_2 n}{1.58}$$

Por isso em análise de algoritmos escrevemos apenas $O(\log n)$ — a base não importa, pois a diferença entre bases é uma constante e constantes somem na notação $O$.

---

## 4. Verificando a lógica com potenciação

Toda propriedade de logaritmo tem um espelho em potenciação. Se você esquecer a propriedade, volte para as potências:

| Potenciação | Logaritmo equivalente |
|---|---|
| $a^1 = a$ | $\log_a a = 1$ |
| $a^0 = 1$ | $\log_a 1 = 0$ |
| $a^x \cdot a^y = a^{x+y}$ | $\log(x \cdot y) = \log x + \log y$ |
| $a^x / a^y = a^{x-y}$ | $\log(x/y) = \log x - \log y$ |
| $(a^x)^k = a^{x \cdot k}$ | $\log(x^k) = k \cdot \log x$ |

---

## 5. Os casos mais comuns em algoritmos

Em análise de algoritmos, o logaritmo aparece sempre que um problema é **dividido ao meio** (ou por qualquer constante) a cada chamada recursiva. Os casos que aparecem com mais frequência são:

**Isolando $k$ quando $2^k = n$:**

$$2^k = n \implies k = \log_2 n$$

**Cancelando base com logaritmo:**

$$2^{\log_2 n} = n$$

**Somando ao expoente** ($+1$ no expoente multiplica por 2):

$$2^{\log_2 n + 1} = 2^{\log_2 n} \cdot 2^1 = n \cdot 2 = 2n$$

**Mudança de base para remover a base** (em notação $O$):

$$\log_3 n = \frac{\log_2 n}{\log_2 3} \implies O(\log_3 n) = O(\log n)$$
