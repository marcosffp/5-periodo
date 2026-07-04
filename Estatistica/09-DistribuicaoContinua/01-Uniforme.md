# 📏 Da discreta para a contínua: o que muda?

No capítulo anterior, X só podia assumir valores contáveis (0, 1, 2, 3...). Agora X pode assumir **qualquer valor dentro de um intervalo** — por exemplo, o tempo de espera de um cliente pode ser 3 minutos, 3,27 minutos, 3,2731... minutos, sem "pulos". Isso é uma **variável aleatória contínua**.

A diferença mais importante na prática: em vez de somar probabilidades ponto a ponto, agora **calculamos áreas sob uma curva** (a função densidade de probabilidade, ou f(x)). Não faz sentido perguntar "qual a probabilidade de X ser exatamente 3,27?" (a resposta seria zero!) — só faz sentido perguntar sobre **intervalos**: "qual a probabilidade de X estar entre 3 e 4?"

---

## 🟩 Distribuição Uniforme ou Contínua (página 169)

É o modelo mais simples de variável contínua: todos os valores dentro de um intervalo `[α, β]` são **igualmente prováveis**. O gráfico de f(x) é literalmente um retângulo.

$$f(x) = \begin{cases} \dfrac{1}{\beta-\alpha}, & \alpha < x < \beta \\ 0, & \text{caso contrário} \end{cases}$$

**Notação:** X ~ U(α; β)

**Analogia do dia a dia:** imagina escolher um ponto ao acaso numa régua de α até β — não tem "lugar preferido", qualquer ponto do intervalo tem a mesma chance. É por isso que a "altura" do retângulo é constante e igual a `1/(β-α)` (esse valor garante que a área total do retângulo seja 1, como toda distribuição de probabilidade exige).

**Esperança:** μₓ = (β + α) / 2 → é literalmente o ponto médio do intervalo, o que faz total sentido pela simetria.

**Variância:** VAR(x) = (β − α)² / 12

### 🔌 Exemplo 9.1 (página 169) — Eficiência de um componente eletrônico

A eficiência varia uniformemente entre 0 e 100 (α=0, β=100). Qual a probabilidade de um componente ter eficiência:

**a) entre 60 e 75?**
Como a distribuição é uniforme, a probabilidade de um intervalo é simplesmente o **tamanho do intervalo dividido pelo tamanho total**:

$$P(60 \le x \le 75) = \frac{75-60}{100} = 0,15$$

**b) acima de 90?**

$$P(x>90) = \frac{100-90}{100} = 0,10$$

**Sacada importante:** na Uniforme, a probabilidade de qualquer intervalo é só uma questão de "proporção de comprimento" — não precisa de fórmulas complicadas nem tabela. É a distribuição contínua mais intuitiva de todas.

---

## 🗺️ Fixando a ideia

| Conceito | Discreta | Contínua |
| --- | --- | --- |
| Pergunta típica | "Qual P(X = x)?" | "Qual P(a ≤ X ≤ b)?" |
| Ferramenta | Soma de probabilidades pontuais | Área sob a curva f(x) |
| P(X = valor exato) | Pode ser > 0 | É sempre 0 |

A Uniforme é só o ponto de partida. Os próximos resumos trazem a **Exponencial** (tempos entre eventos) e a **Normal** (a mais importante e usada de todas).
