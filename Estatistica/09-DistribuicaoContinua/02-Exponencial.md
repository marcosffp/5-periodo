# ⏱️ Distribuição Exponencial (páginas 170 a 171)

Essa distribuição responde a uma pergunta muito comum: **"quanto tempo (ou distância) até o próximo evento acontecer?"**

Exemplos clássicos: tempo até uma peça eletrônica falhar, tempo entre a chegada de dois clientes num supermercado, tempo entre duas ligações telefônicas.

### 🔗 A relação direta com a Poisson

Lembra que na Poisson (capítulo 8) contávamos **quantos eventos** aconteciam num intervalo de tempo? A Exponencial é o "espelho" disso: em vez de contar eventos, ela mede o **espaço/tempo entre um evento e outro**.

> Se um processo de Poisson tem média de λ ocorrências num intervalo, o tempo (ou espaço) entre ocorrências consecutivas nesse mesmo intervalo é **1/λ**.

**Analogia:** se em média chegam 10 navios por dia num porto (Poisson, λ=10), o tempo médio **entre a chegada de um navio e o próximo** é de 1/10 do dia — essa é a lógica da Exponencial.

### A fórmula

$$f(x) = \begin{cases} \lambda e^{-\lambda x}, & x>0, \lambda>0 \\ 0, & \text{caso contrário} \end{cases}$$

**Esperança e variância:**

$$\mu = \frac{1}{\lambda} \qquad \sigma^2 = \frac{1}{\lambda^2}$$

Repara que aqui λ representa uma **taxa** (eventos por unidade de tempo), enquanto μ = 1/λ é o **tempo médio** entre eventos — são a mesma informação, vista de dois ângulos diferentes.

### 📐 As fórmulas prontas de probabilidade

Calcular a área sob a curva exponencial exigiria integração por partes toda vez. Por sorte, já existem as fórmulas prontas:

$$P(X>x) = e^{-\lambda x} \qquad P(X \le x) = 1 - e^{-\lambda x}$$

## 🍽️ Exemplo 9.2 (páginas 170-171) — Tempo de espera em um restaurante

O tempo médio entre o pedido e o atendimento é de 10 minutos (distribuição exponencial). Como μ = 10, temos λ = 1/10 = 0,1.

**a) Probabilidade de espera superior a 10 minutos:**

$$P(X>10) = e^{-(0,1)(10)} = e^{-1} = 0,368$$

**b) Probabilidade de espera não superior a 3 minutos:**

$$P(X<3) = 1 - e^{-(0,1)(3)} = 1 - 0,741 = 0,259$$

**Sacada importante:** repara que mesmo com média de 10 minutos, a chance de esperar **mais** que a própria média (0,368) é bem menor que 50%. Isso acontece porque a Exponencial é bem assimétrica — tem uma "cauda longa" de valores grandes, mas a maior parte da probabilidade se concentra nos valores baixos, próximos de zero.

---

## 🗺️ Resumindo

| Pergunta | Ferramenta |
| --- | --- |
| Quantos eventos ocorrem num intervalo? | Poisson (capítulo 8) |
| Quanto tempo/espaço até o próximo evento? | Exponencial |

A Exponencial é o elo entre o mundo discreto (contar eventos) e o mundo contínuo (medir tempo/distância) — os dois modelos descrevem o **mesmo fenômeno físico** sob ângulos diferentes.
