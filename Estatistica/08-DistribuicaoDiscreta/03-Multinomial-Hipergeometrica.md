# 🎨 Distribuição Multinomial (página 164)

A Binomial só lida com **dois** resultados possíveis (sucesso/fracasso). E se o experimento tiver **vários** resultados possíveis? Por exemplo: tirar uma bola de uma caixa com bolas de 3 cores diferentes.

É exatamente para isso que serve a **Multinomial**: uma **generalização da Binomial**, onde em vez de 2 resultados temos k resultados possíveis (a₁, a₂, ..., aₖ), cada um com sua própria probabilidade (p₁, p₂, ..., pₖ), constante ao longo das provas e independente entre elas.

Se xᵢ é o número de vezes que o resultado aᵢ aparece (com x₁ + x₂ + ... + xₖ = n), a fórmula é:

$$P(x_1,x_2,...,x_k) = P_n^{x_1,x_2,...,x_k} \times p_1^{x_1} \times p_2^{x_2} \times ... \times p_k^{x_k}$$

O `P_n^{x_1,x_2,...,x_k}` é a **permutação com repetição** — a versão do "C_n^x" da Binomial, mas para mais de 2 categorias.

**Analogia:** pensa na Binomial como escolher entre "chove" ou "não chove". A Multinomial seria escolher entre "sol", "chuva" ou "nublado" — mais opções, mesma lógica de contar todas as ordens possíveis.

## 🔴🟡⚪ Exemplo 8.6 (páginas 164-165) — Bolas coloridas

Uma caixa tem 4 bolas vermelhas, 3 brancas e 3 pretas (10 no total, com reposição a cada extração). Em 5 extrações, qual a probabilidade de sair 2 vermelhas, 1 branca e 2 pretas?

Probabilidades de cada cor: pᵥ = 0,4 | p_b = 0,3 | p_p = 0,3

$$P(2,1,2) = P_5^{2,1,2} \times p_v^2 \times p_b^1 \times p_p^2 = \frac{5!}{2! \, 1! \, 2!} \times 0,4^2 \times 0,3^1 \times 0,3^2 = 0,1296$$

---

# 🎯 Distribuição Hipergeométrica (páginas 165 a 166)

Repara num detalhe importante do exemplo acima: as bolas eram **repostas** depois de cada extração. Isso mantém p constante — condição obrigatória para usar Binomial ou Multinomial.

**E se não houver reposição?** Aí a probabilidade de sucesso muda a cada extração (porque o total de itens disponíveis diminui), e a Binomial deixa de valer. É aí que entra a **Hipergeométrica**.

**Regra prática para escolher o modelo:**

- **Com reposição** (ou população "infinita") → **Binomial**
- **Sem reposição** (população finita, cada retirada muda as chances) → **Hipergeométrica**

**Analogia do dia a dia:** tirar cartas de um baralho **sem devolver** é Hipergeométrica (a cada carta tirada, a composição do baralho muda). Já jogar uma moeda várias vezes é Binomial (a moeda "não muda" a cada jogada).

Na Hipergeométrica, de uma população de N itens, dos quais k são considerados "sucesso" (e N − k são "fracasso"), extraímos uma amostra de tamanho n **sem reposição**. Queremos a probabilidade de obter exatamente x sucessos:

$$P(X=x) = \frac{C_k^x \times C_{N-k}^{n-x}}{C_N^n}$$

**Notação:** X ~ H(N, n, k)

**Esperança e variância:**

$$\mu = \frac{nk}{N} \qquad \sigma^2 = n\left(\frac{k}{N}\right)\left(1-\frac{k}{N}\right)\left(\frac{N-n}{N-1}\right)$$

Repara que a esperança é igual à da Binomial trocando p por k/N. A variância também é parecida com `npq`, só que multiplicada pelo fator `(N-n)/(N-1)` — esse fator é a chamada **correção de população finita**, que "compensa" o efeito de não haver reposição.

**Atalho útil:** quando n é bem pequeno comparado com N, tirar com ou sem reposição dá quase no mesmo — nesse caso, a Hipergeométrica pode ser aproximada pela Binomial (com p = k/N). Faz sentido: se a população é gigante, tirar poucos itens dela "não faz cócegas" no total, então a chance quase não muda entre uma extração e outra.

## 📦 Exemplo 8.7 (páginas 165-166) — Lotes de peças

Lotes de 40 peças são aceitáveis se tiverem no máximo 3 peças defeituosas. Extrai-se uma amostra de 5 peças (sem reposição) e rejeita-se o lote se aparecer ao menos 1 defeituosa. Se o lote tem exatamente 3 peças defeituosas, qual a probabilidade de a amostra ter **exatamente 1** defeituosa?

Aqui: N = 40, n = 5, k = 3 (defeituosas), x = 1

$$P(X=1) = \frac{C_3^1 \times C_{37}^4}{C_{40}^5} = \frac{3 \times 66045}{658008} = 0,301113$$

---

## 🗺️ Resumindo tudo do capítulo

| Distribuição | Nº de resultados possíveis | Reposição? | Fórmula-chave |
| --- | --- | --- | --- |
| **Bernoulli** | 2 | única prova | p |
| **Binomial** | 2 | com reposição (ou população infinita) | C_n^x p^x q^{n-x} |
| **Poisson** | eventos raros, sem "n" fixo | — | λ^x e^{-λ} / x! |
| **Multinomial** | k | com reposição | permutação × produto de pᵢ^{xᵢ} |
| **Hipergeométrica** | 2 | **sem** reposição | combinação de C_k^x, C_{N-k}^{n-x}, C_N^n |

A pergunta que resolve qual modelo usar é sempre a mesma: **quantos resultados são possíveis? A probabilidade se mantém constante entre as provas (com reposição) ou muda (sem reposição)? Dá pra contar quantas "tentativas" aconteceram, ou só faz sentido contar quantos eventos ocorreram?**
