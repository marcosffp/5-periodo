# 🎲 Variáveis discretas: contando sucessos

Esse capítulo trata de **modelos prontos** para calcular probabilidades quando a variável aleatória X é **discreta**, ou seja, só assume valores contáveis (0, 1, 2, 3...). A ideia é: em vez de montar a distribuição de probabilidade do zero toda vez, usamos fórmulas já conhecidas que se encaixam em situações recorrentes.

---

## 🟡 Distribuição de Bernoulli (página 153)

É o modelo mais simples de todos: um experimento com **só duas saídas possíveis**.

**Condições para ser Bernoulli:**

- Todas as provas acontecem sob as mesmas condições;
- Só existem dois resultados possíveis: **Sucesso (S)** ou **Fracasso (S̅)**, e eles são excludentes → P(S) + P(S̅) = 1;
- A probabilidade de sucesso `p = P(S)` é sempre a mesma;
- As provas são independentes entre si.

**Analogia do dia a dia:** imagina jogar uma moeda **uma única vez**. Só existem dois resultados (cara ou coroa), a probabilidade não muda, e não há relação com jogadas passadas. Isso é uma Bernoulli.

Para facilitar as contas, convencionamos **0 para Fracasso** e **1 para Sucesso**:

| X | P(x) |
| --- | --- |
| 0 | q |
| 1 | p |
| **Total** | **1** |

**Esperança:** μₓ = p
**Variância:** σ²(x) = p(1 − p) = pq

Faz sentido: se p = 0,5 (tipo uma moeda honesta), a variância é máxima (0,25), porque o resultado é o mais "imprevisível" possível. Se p está perto de 0 ou de 1, a variância cai, porque o resultado já é quase certo.

---

## 🔵 Distribuição Binomial (páginas 154 a 157)

E se, em vez de jogar a moeda **uma vez**, a gente jogar **n vezes** e quiser saber quantos sucessos vão sair? Isso é a Binomial: **n repetições independentes de uma Bernoulli**, todas com a mesma probabilidade de sucesso p.

A probabilidade de conseguir exatamente x sucessos (e, portanto, n − x fracassos) é:

$$f(x) = P(X=x) = C_n^x \, p^x q^{n-x}, \quad 0 \le x \le n$$

O `C_n^x` (combinação) entra porque os x sucessos podem acontecer em **qualquer ordem** dentro das n provas — precisamos contar todas as disposições possíveis.

**Notação:** X ~ B(n; p)

### 🔺 A forma do histograma muda com p (Exemplo 8.1, página 154)

Testando n = 5 com três valores de p:

- **p = 0,5** → distribuição **simétrica** (o histograma faz um "sino" equilibrado)
- **p < 0,5** → distribuição **assimétrica à esquerda** (puxada para valores baixos de x)
- **p > 0,5** → distribuição **assimétrica à direita** (puxada para valores altos de x)

**Analogia:** se a chance de sucesso é baixa (p pequeno), é mais provável ter poucos sucessos — o gráfico "empilha" para a esquerda. Se a chance é alta, empilha para a direita.

### 📐 Esperança e Variância da Binomial

Como a Binomial nada mais é que "n Bernoullis somadas", os resultados saem quase de graça:

- **Esperança:** μₓ = np
- **Variância:** σₓ² = npq

### 🧮 Exemplo 8.2 (página 155) — Pacientes esperando na clínica

70% dos pacientes esperam pelo menos 15 minutos (p = 0,7 → q = 0,3). Para 8 pacientes:

- **Nenhum espera:** P(X=0) = C₈⁰ × 0,7⁰ × 0,3⁸ = **0,00007**
- **Dois esperam:** P(X=2) = C₈² × 0,7² × 0,3⁶ = **0,00122**
- **Seis esperam:** P(X=5)* = C₈⁵ × 0,7⁵ × 0,3³ = **0,25412**

*(no gabarito do exemplo o item "c" usa x=5 na conta, mesmo pedindo "seis pacientes" no enunciado — vale conferir com o professor qual das duas leituras é a pretendida.)*

### 🎲 Exemplo 8.3 (página 156) — Um único "2" em 3 lançamentos de dado

Sucesso = sair face 2 → p = 1/6, fracasso = qualquer outra face → q = 5/6.

$$f(1) = C_3^1 \left(\frac{1}{6}\right)^1\left(\frac{5}{6}\right)^2 = 0,3472222$$

### 📊 Exemplo 8.4 (páginas 156-157) — Usando a Tabela da Binomial Acumulada

Quando n cresce, calcular ponto a ponto é trabalhoso. Por isso existe a **Tabela da Distribuição Binomial Acumulada**, que já traz pronto o valor de P(X ≤ x) para várias combinações de n, x e p.

Para X ~ B(9; 0,25):

**a) P(X ≤ 5)** → basta olhar direto na tabela, linha n=9, x=5, coluna p=0,25:
$$P(X \le 5) = 0,990005$$

**b) P(3 ≤ X ≤ 7)** → a tabela só dá valores acumulados "≤", então é preciso combinar dois valores:
$$P(3 \le X \le 7) = P(X \le 7) - P(X \le 2) = 0,999893 - 0,600677 = 0,399216$$

**Truque geral:** sempre que a probabilidade pedida for um intervalo fechado dos dois lados, subtraia o acumulado "de baixo" (até o limite inferior − 1) do acumulado "de cima" (até o limite superior).

---

## 🗺️ Resumindo até aqui

| Distribuição | Quando usar | Esperança | Variância |
| --- | --- | --- | --- |
| **Bernoulli** | 1 prova, 2 resultados possíveis | p | pq |
| **Binomial** | n provas de Bernoulli independentes, mesma p | np | npq |

A Binomial é a "mãe" de várias outras distribuições discretas que vêm a seguir — a Poisson, por exemplo, pode ser vista como um caso-limite dela (veja o resumo seguinte).
