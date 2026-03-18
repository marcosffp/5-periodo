# Notações $\Omega$ e $\Theta$ — Limites Assintóticos Inferior e Justo

---

## 1. Ideia Principal

No resumo anterior vimos a notação $O$, que descreve o **limite superior** do crescimento de um algoritmo — uma garantia de que o custo nunca passa de certo teto. Agora completamos o quadro com mais duas notações:

- $\Omega$ (Ômega) descreve o **limite inferior** — uma garantia de que o custo nunca fica abaixo de certo piso.
- $\Theta$ (Teta) descreve o **limite justo** — quando o custo está espremido entre um teto e um piso do mesmo tipo, ou seja, cresce exatamente na mesma ordem que $f(n)$.

Juntas, as três notações descrevem o comportamento assintótico de forma completa: $O$ diz o máximo que pode crescer, $\Omega$ diz o mínimo, e $\Theta$ diz quando os dois coincidem.

---

## 2. Conceitos Importantes

---

### Notação $\Omega$ — limite inferior

$$g(n) = \Omega(f(n)) \iff \exists\, c > 0 \text{ e } m \mid 0 \leq c \cdot f(n) \leq g(n), \;\forall n \geq m$$

Em português: $g(n) = \Omega(f(n))$ se existem uma constante $c$ e um ponto de corte $m$ tal que, **a partir de $m$**, $c \cdot f(n)$ é sempre menor ou igual a $g(n)$.

A diferença em relação ao $O$ está na direção da desigualdade:

| Notação | Desigualdade | Papel de $f(n)$ |
|---|---|---|
| $O$ | $g(n) \leq c \cdot f(n)$ | teto — $f(n)$ domina $g(n)$ por cima |
| $\Omega$ | $g(n) \geq c \cdot f(n)$ | piso — $f(n)$ fica por baixo de $g(n)$ |

**Intuição:** se $g(n) = \Omega(n^2)$, o algoritmo nunca será mais rápido que $n^2$. Não importa qual entrada você forneça — para $n$ suficientemente grande, o custo sempre supera algum múltiplo de $n^2$.

---

### Notação $\Theta$ — limite justo

$$g(n) = \Theta(f(n)) \iff \exists\, c_1, c_2 > 0 \text{ e } m \mid 0 \leq c_1 \cdot f(n) \leq g(n) \leq c_2 \cdot f(n), \;\forall n \geq m$$

Em português: $g(n) = \Theta(f(n))$ se existem **duas constantes** $c_1$ e $c_2$ e um ponto de corte $m$ tal que, a partir de $m$, $g(n)$ fica sempre espremida entre $c_1 \cdot f(n)$ (por baixo) e $c_2 \cdot f(n)$ (por cima).

Visualmente, $g(n)$ fica dentro de um corredor delimitado por $c_1 \cdot f(n)$ e $c_2 \cdot f(n)$ — ela não pode escapar nem por cima nem por baixo.

**Relação com $O$ e $\Omega$:** a notação $\Theta$ é equivalente a ter as duas ao mesmo tempo:

$$g(n) = \Theta(f(n)) \iff g(n) = O(f(n)) \;\text{ e }\; g(n) = \Omega(f(n))$$

---

## 3. Método Geral para Provar cada Notação

---

### Para provar $g(n) = \Omega(f(n))$

O objetivo é encontrar $c$ e $m$ tais que $g(n) \geq c \cdot f(n)$.

1. Escreva a desigualdade $g(n) \geq c \cdot f(n)$.
2. Divida ambos os lados por $f(n)$ para isolar $c$.
3. Analise o lado esquerdo quando $n$ cresce.
4. Se o lado esquerdo cresce ou tende a uma constante positiva, a prova funciona — escolha $c$ menor que esse valor.

---

### Para provar $g(n) = \Theta(f(n))$

O objetivo é encontrar $c_1$, $c_2$ e $m$ tais que $c_1 \cdot f(n) \leq g(n) \leq c_2 \cdot f(n)$.

Isso significa provar **duas coisas ao mesmo tempo**:

- A parte direita ($g(n) \leq c_2 \cdot f(n)$) — igual a provar $g(n) = O(f(n))$.
- A parte esquerda ($g(n) \geq c_1 \cdot f(n)$) — igual a provar $g(n) = \Omega(f(n))$.

---

## 4. Explicação Detalhada dos Exemplos

---

### Exemplo 1 — $\Omega$: $g(n) = n^3 + 2n^2$ e $f(n) = 5n^2 + 10$

**Pergunta:** $g(n) = \Omega(f(n))$?

Queremos mostrar que $g(n) \geq c \cdot f(n)$ para $n$ suficientemente grande. Com $c = 1$:

$$n^3 + 2n^2 \geq 1 \cdot (5n^2 + 10)$$

Olhando a tabela do slide:

| $n$ | $g(n)$ | $f(n)$ |
|---|---|---|
| 0 | 0 | 10 |
| 2 | 16 | 30 |
| 4 | 96 | 90 ✓ |
| 6 | 288 | 190 ✓ |
| 8 | 640 | 330 ✓ |

A partir de $n = 4$, $g(n) \geq f(n)$. Portanto $c = 1$ e $m = 4$.

**Por que isso acontece?** Porque $g(n)$ tem grau 3 e $f(n)$ tem grau 2. Para $n$ pequeno $f(n)$ pode ser maior (o $+10$ tem peso), mas conforme $n$ cresce, o $n^3$ domina completamente e $g(n)$ dispara acima de $f(n)$ para sempre.

**Resultado:**

$$g(n) = \Omega(5n^2 + 10) \quad \text{com } c = 1 \text{ e } m = 4$$

---

### Exemplo 2 — $\Theta$: $g(n) = 5n^2 + 10$ e $f(n) = n^2 + 2n$

**Pergunta:** $g(n) = \Theta(f(n))$?

Precisamos encontrar $c_1$, $c_2$ e $m$ tais que:

$$c_1 \cdot (n^2 + 2n) \leq 5n^2 + 10 \leq c_2 \cdot (n^2 + 2n)$$

O slide usa $c_1 = 1$ e $c_2 = 5$. Vamos verificar:

| $n$ | $1 \cdot f(n)$ | $g(n)$ | $5 \cdot f(n)$ |
|---|---|---|---|
| 0 | 0 | 10 | 0 |
| 2 | 8 | 30 | 40 |
| 4 | 24 | 90 | 120 |
| 6 | 48 | 190 | 240 |
| 8 | 80 | 330 | 400 |

A partir de $n = 2$, a condição $1 \cdot f(n) \leq g(n) \leq 5 \cdot f(n)$ vale em todas as linhas. $g(n)$ fica sempre dentro do corredor.

Para $n = 0$: $g(0) = 10$ e $5 \cdot f(0) = 0$ — a condição falha. Por isso $m = 2$, não $m = 0$.

**Por que a condição vale a partir de $n = 2$?** Os dois têm grau dominante $n^2$. Para $n$ pequeno os termos menores ($2n$ em $f$ e $+10$ em $g$) distorcem a relação. Para $n$ grande, o comportamento é ditado por $n^2$ nos dois, então $g(n)$ e $f(n)$ crescem proporcionalmente — é por isso que existem constantes que os enquadram.

**Resultado:**

$$g(n) = \Theta(n^2 + 2n) \quad \text{com } c_1 = 1,\; c_2 = 5 \text{ e } m = 2$$

---

### Exercício — prove $n^2 + 10 = \Omega(n^2)$

Queremos mostrar que $n^2 + 10 \geq c \cdot n^2$.

**Passo 1 — dividir por $n^2$:**

$$\frac{n^2 + 10}{n^2} \geq c$$

$$1 + \frac{10}{n^2} \geq c$$

**Passo 2 — analisar:** o lado esquerdo é sempre maior que $1$ para qualquer $n > 0$, pois $\frac{10}{n^2} > 0$.

**Passo 3 — escolher $c$ e $m$:** basta tomar $c = 1$ e $m = 1$. Para todo $n \geq 1$:

$$n^2 + 10 \geq 1 \cdot n^2 \checkmark$$

**Resultado:** $n^2 + 10 = \Omega(n^2)$ com $c = 1$ e $m = 1$.

---

### Exercício — prove $3n^3 + 2n^2 = \Omega(n^3)$

Queremos mostrar que $3n^3 + 2n^2 \geq c \cdot n^3$.

**Passo 1 — dividir por $n^3$:**

$$3 + \frac{2}{n} \geq c$$

**Passo 2 — analisar:** o lado esquerdo é sempre maior que $3$ para qualquer $n > 0$, pois $\frac{2}{n} > 0$.

**Passo 3 — escolher $c$ e $m$:** basta tomar $c = 3$ e $m = 1$. Para todo $n \geq 1$:

$$3n^3 + 2n^2 \geq 3 \cdot n^3 \checkmark$$

**Resultado:** $3n^3 + 2n^2 = \Omega(n^3)$ com $c = 3$ e $m = 1$.

---

## 5. Como Aplicar o Raciocínio em Outros Exercícios

O processo para cada notação é simétrico:

**Para provar $O$:** divida $g(n)$ por $f(n)$ e verifique se o resultado **tende a uma constante ou diminui** quando $n \to \infty$. Se sim, essa constante (ou um valor acima dela) serve como $c$.

**Para provar $\Omega$:** divida $g(n)$ por $f(n)$ e verifique se o resultado **tende a uma constante positiva ou cresce** quando $n \to \infty$. Se sim, qualquer valor abaixo desse limite serve como $c$.

**Para provar $\Theta$:** faça os dois acima. Se ambos funcionam com o mesmo $f(n)$, então $g(n) = \Theta(f(n))$.

**Quando $\Theta$ não é possível:** se $g(n)$ tem grau maior que $f(n)$, como no exemplo $g(n) = n^3 + 2n^2$ e $f(n) = 5n^2 + 10$, podemos provar $\Omega$ mas não $O$ — porque $g(n)$ cresce mais rápido e nunca fica abaixo de $f(n)$, mas também nunca fica acima de um múltiplo dela pelo lado superior.

---

## 6. Comparando as Três Notações

| Notação | O que prova | Desigualdade | Papel de $f(n)$ |
|---|---|---|---|
| $O$ | limite superior | $g(n) \leq c \cdot f(n)$ | teto do crescimento |
| $\Omega$ | limite inferior | $g(n) \geq c \cdot f(n)$ | piso do crescimento |
| $\Theta$ | limite justo | $c_1 \cdot f(n) \leq g(n) \leq c_2 \cdot f(n)$ | corredor do crescimento |

---

## 7. Dicas para Resolver sem Precisar Decorar

**Dica 1 — lembre da direção da desigualdade pelo nome.** $\Omega$ é o limite **inferior** — o piso. A desigualdade aponta para cima: $g(n) \geq c \cdot f(n)$. $O$ é o limite **superior** — o teto. A desigualdade aponta para baixo: $g(n) \leq c \cdot f(n)$.

**Dica 2 — para $\Omega$, escolha $c$ menor que o grau dominante.** Se $g(n) = 3n^3 + \dots$ e $f(n) = n^3$, ao dividir você obtém $3 + \text{algo positivo}$. O lado esquerdo nunca fica abaixo de $3$, então $c = 3$ funciona.

**Dica 3 — $\Theta$ só existe quando os dois têm o mesmo grau dominante.** Se $g(n) = 5n^2 + 10$ e $f(n) = n^2 + 2n$, ambos têm grau $n^2$. Para $n$ grande eles crescem proporcionalmente, então existem $c_1$ e $c_2$ que os enquadram. Se os graus fossem diferentes, $\Theta$ seria impossível.

**Dica 4 — $\Theta$ é $O$ e $\Omega$ ao mesmo tempo.** Se você já provou $O$ e $\Omega$ para o mesmo $f(n)$, você automaticamente tem $\Theta$. Basta juntar as constantes: o $c$ do $O$ vira $c_2$, e o $c$ do $\Omega$ vira $c_1$.

**Dica 5 — o $m$ pode ser qualquer valor que faça a condição funcionar.** Não existe um $m$ único correto. Se precisar de $m$ pequeno para simplificar, use $m = 1$ e ajuste $c$ para compensar.