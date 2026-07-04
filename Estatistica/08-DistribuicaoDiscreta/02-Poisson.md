# 🐟 Distribuição de Poisson (páginas 160 a 163)

A Binomial funciona bem quando sabemos exatamente **quantas provas (n)** aconteceram. Mas e quando o número de "tentativas" nem faz sentido contar? Por exemplo: quantos navios chegam a um porto por dia? Quantas ligações uma central recebe por hora? Não existe um "n fixo de tentativas" — os eventos só **acontecem** ao longo de um intervalo de tempo, área ou espaço.

Para esses casos, usamos a **Distribuição de Poisson**: ela é aplicada quando o número de fracassos (ou de provas) é uma grandeza **inumerável** — só conseguimos contar quantos sucessos (eventos) ocorreram.

**Analogia do dia a dia:** pensa em ficar contando quantos raios caem numa tempestade em uma hora. Não tem sentido perguntar "de quantas tentativas de raio" isso saiu — só faz sentido contar quantos **aconteceram**.

A fórmula:

$$P(x,t) = \frac{\lambda^x}{x!}e^{-\lambda}$$

onde **λ = μₓ = VAR(x)** — ou seja, na Poisson a **média e a variância são iguais**! Isso é uma marca registrada dessa distribuição.

**Notação:** X ~ P(λ)

---

## ⛴️ Exemplo 8.5 (página 161) — Navios chegando a um porto

Chegam em média 10 navios-tanque por dia (λ = 10) a um porto que comporta 15 navios. Qual a probabilidade de, em um dia, algum navio ter que ficar esperando (ou seja, chegarem **mais de 15** navios)?

**1ª maneira — pela fórmula direto:**

$$P(X>15) = 1 - P(X \le 15) = 1 - \sum_{x=0}^{15}\frac{10^x}{x!}e^{-10} = 1 - 0,9513 = 0,0487$$

**2ª maneira — pela Tabela de Poisson Acumulada:**
Assim como na Binomial, existe uma tabela pronta com os valores de P(X ≤ x) para vários λ. Procurando λ = 10 e x = 15 na tabela, achamos P(X ≤ 15) = 0,9512, logo:

$$P(X>15) = 1 - 0,9512 = 0,0488$$

(a pequena diferença entre as duas contas é só arredondamento).

---

## 🔁 Poisson como aproximação da Binomial (página 162)

Aqui está uma sacada importante: quando a Binomial tem um **n muito grande** e uma **p muito pequena**, calcular C_n^x fica inviável na mão (imagina calcular C₂₀₀⁵ manualmente!). Nesses casos, a **Poisson aproxima muito bem a Binomial**, desde que `np` resulte em um valor moderado.

**A ideia central:** usamos λ = np, já que essa é a esperança tanto da Binomial quanto da Poisson.

**Exemplo prático:** Binomial com n = 200 e p = 0,04, queremos P(X=5).

- **Pela Binomial (fórmula "pesada"):**
$$P(X=5) = C_{200}^5 \times 0,04^5 \times 0,96^{195}$$

- **Pela Poisson (bem mais simples):** como λ = np = 200 × 0,04 = 8,
$$P(X=5) = \frac{8^5}{5!}e^{-8} = 0,191$$

**Analogia:** é tipo trocar uma conta gigante e cheia de fatoriais por uma aproximação prática que dá quase o mesmo resultado — vale a pena sempre que "muitas tentativas, cada uma com pouca chance de sucesso" (ex: erros de impressão, defeitos de fabricação, ligações erradas, acidentes).

---

## 🗺️ Resumindo

| Situação | Distribuição indicada |
| --- | --- |
| Número fixo de provas n, cada uma sucesso/fracasso | Binomial |
| Eventos raros ao longo de tempo/espaço, sem "n" definido | Poisson |
| Binomial com n grande e p pequeno (np moderado) | Poisson como aproximação |

Repare que em ambas o parâmetro λ (ou np) concentra toda a informação: na Poisson, λ sozinho já define média e variância — não precisamos separar "quantas provas" de "qual a chance", só precisamos saber a **taxa média de ocorrência**.
