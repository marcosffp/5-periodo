# Resumo — Variáveis Aleatórias

## 0. Visão Geral: para que serve tudo isso?

Esse capítulo conecta a **Probabilidade** (capítulo anterior) com a **Estatística Descritiva** (capítulos anteriores, médias, desvio padrão etc.). A ideia central é:

> Transformar resultados de um experimento (que muitas vezes não são números, como "cara/coroa") em **números**, para que a gente possa calcular médias, variâncias, gráficos, etc. sobre eles.

O capítulo segue uma linha lógica:

1. **O que é** uma Variável Aleatória (VA).
2. VA pode ser **Discreta** (valores contáveis, "aos saltos") ou **Contínua** (qualquer valor num intervalo).
3. Para cada tipo, existe uma forma de descrever as probabilidades: **Função de Probabilidade** (discreta) ou **Função Densidade** (contínua).
4. Existe também uma **Função de Repartição F(x)**, que acumula as probabilidades até um ponto.
5. Quando temos **duas variáveis ao mesmo tempo** (X e Y), surgem as distribuições **conjuntas**, **marginais** e o conceito de **independência**.
6. Por fim, assim como na estatística descritiva, calculamos **medidas de posição** (Esperança, Mediana, Moda) e **medidas de dispersão** (Desvio Padrão, Variância, Covariância, Correlação) — só que agora usando probabilidades em vez de frequências.

Guarde essa "espinha dorsal": ela vai te ajudar a não se perder no meio das fórmulas.

---

## 1. O que é uma Variável Aleatória (VA)?

### Definição formal

> Sejam **E** um experimento e **S** o Espaço Amostral associado a esse experimento. Uma função **X**, que associa a cada elemento `s ∈ S` um número real `X(s)`, é denominada **Variável Aleatória**.

### Explicação simples

Pense numa Variável Aleatória como um **"tradutor"**: ela pega cada resultado possível de um experimento (que pode ser "cara", "coroa", "vermelho", "defeituoso" etc.) e **traduz para um número**. Isso é útil porque é muito mais fácil fazer conta com números do que com palavras.

```
S (espaço amostral)  --- X --->  ℝ (números reais)

  s₁  ------------------------>  X(s₁)
  s₂  ------------------------>  X(s₂)
  s₃  ------------------------>  X(s₃)
```

### Exemplo de referência (vai aparecer várias vezes no capítulo)

Lançar duas moedas. O Espaço Amostral é:

```
S = { KK, KC, CK, CC }      (K = cara, C = coroa)
```

Seja **X = número de caras (K)** obtidas. Para cada resultado, traduzimos em número:

| Evento | KK | KC | CK | CC |
|---|---|---|---|---|
| X | 2 | 1 | 1 | 0 |

Agora podemos montar a tabela de probabilidades de X:

| Número de Caras (X) | P(x) |
|---|---|
| 0 | 1/4 |
| 1 | 1/2 |
| 2 | 1/4 |

Note que cada resultado de S tem probabilidade 1/4 (são 4 resultados igualmente prováveis), mas como **KC** e **CK** dão o mesmo valor de X (=1), as probabilidades se somam: 1/4 + 1/4 = 1/2.

---

## 2. Variável Aleatória Discreta x Contínua

Essa é a primeira "bifurcação" do capítulo — tudo o que vem depois depende de saber se a VA é discreta ou contínua.

| | **Discreta** | **Contínua** |
|---|---|---|
| Valores que pode assumir | Valores **inteiros / contáveis** (0, 1, 2, 3, ...) | **Qualquer valor real** dentro de um intervalo |
| Exemplos | Número de caras em 2 moedas, número de filhos, número de defeitos | Altura, peso, tempo, temperatura |
| Como descrever a probabilidade | Função de Probabilidade `P(x)` | Função Densidade de Probabilidade `f(x)` |
| P(X = um valor específico) | Pode ser maior que zero | É **sempre zero** (explicado na seção 4) |

> **Dica:** se você consegue "contar nos dedos" os valores possíveis (mesmo que sejam infinitos, como 0,1,2,3,...), a variável é discreta. Se os valores "preenchem" um intervalo sem buracos (ex: qualquer número entre 1,70m e 1,80m), é contínua.

---

## 3. Variáveis Aleatórias Discretas

### 3.1. Função de Probabilidade — P(x)

A probabilidade de uma Variável Aleatória X assumir um valor `x₀` é chamada de **Função de Probabilidade**, escrita como:

```
P(X = x₀)   ou simplesmente   P(x₀)
```

Ela determina a **Distribuição de Probabilidades** da VA X, que pode ser representada de **3 formas**:

- **Tabela** (lista de valores e suas probabilidades)
- **Gráfico** (geralmente em "bastões" verticais)
- **Fórmula** (uma expressão matemática)

#### Exemplo 7.1 — as três formas de representar P(x)

Usando o exemplo das 2 moedas (X = número de caras):

**Tabela:**

| X | P(x) |
|---|---|
| 0 | 1/4 |
| 1 | 1/2 |
| 2 | 1/4 |

**Gráfico:** barras verticais em x=0 (altura 1/4), x=1 (altura 1/2) e x=2 (altura 1/4).

**Fórmula:**

```
P(x) = (1/4) × C(2,x)     para x = 0, 1, 2
```

(onde C(2,x) é a combinação "2 escolhe x" — o número de formas de tirar x caras em 2 jogadas).

### Observações importantes

1. **Qualquer função de uma VA também é uma VA.** Ou seja, se `X` é uma Variável Aleatória, então `Y = aX + b` ou `Z = cX² - 3/X` também são Variáveis Aleatórias. Isso é importante porque depois vamos calcular esperança e variância de "funções de X", não só de X puro.
2. A probabilidade `P(X = x)` pode ser escrita tanto como `P(x)` quanto como `f(x)` — são a mesma coisa.

### Condições para ser uma Distribuição de Probabilidades válida

Para que `f(x)` seja de fato uma distribuição de probabilidades, ela **precisa** satisfazer:

```
f(x) ≥ 0                    (nenhuma probabilidade é negativa)

      +∞
      Σ   f(xᵢ) = 1          (a soma de todas as probabilidades é 1, ou seja, 100%)
    i=-∞
```

> **Macete:** sempre que você for verificar se uma "fórmula" dada num exercício é realmente uma distribuição válida, confira essas duas condições. É um tipo de pergunta clássico de prova.

---

### 3.2. Função de Repartição (ou Função Acumulada) — F(x)

#### Definição

> A **Função de Repartição** de uma VA X, no ponto `x = x₀`, é definida como a probabilidade de X assumir um valor **menor ou igual** a `x₀`:

```
F(x₀) = P(X ≤ x₀)

         x₀
F(x₀) =  Σ   f(x)
       x=-∞
```

### Explicação simples

Pense em `F(x)` como um **"placar acumulado"**: ele soma todas as probabilidades "até aquele ponto, da esquerda para a direita". É exatamente como a **frequência acumulada** que você já viu em estatística descritiva, só que agora usando probabilidades.

### 3.2.1. Propriedades da Função de Repartição

Essas propriedades caem bastante em prova — todas decorrem diretamente da ideia de "soma acumulada":

```
P.1)  F(-∞) = 0                                        → antes de tudo, nada foi acumulado
P.2)  F(+∞) = 1                                        → no final, acumulou-se 100%
P.3)  P(a < x ≤ b) = F(b) - F(a)         (com b > a)   → "tira" o que já estava acumulado até a
P.4)  P(a ≤ x ≤ b) = F(b) - F(a) + P(X = a)            → soma de volta o ponto a, que tinha sido tirado
P.5)  P(a < x < b) = F(b) - F(a) - P(X = b)            → tira também o ponto b
P.6)  P(a ≤ x < b) = F(b) - F(a) + P(X = a) - P(X = b) → ajusta os dois extremos
```

> **Dica para entender P.3 a P.6:** `F(b) - F(a)` sempre te dá o "intervalo (a, b]" (sem contar `a`, contando `b`). As propriedades P.4, P.5 e P.6 são só ajustes para incluir ou excluir os pontos `a` e `b`, somando ou subtraindo `P(X=a)` e `P(X=b)` conforme o caso.

#### Exemplo 7.2 — construindo F(x) passo a passo

Uma VA X assume os valores 0, 1 e 2, com probabilidades 1/3, 1/6 e 1/2, respectivamente.

| X | P(x) |
|---|---|
| 0 | 1/3 |
| 1 | 1/6 |
| 2 | 1/2 |

Para montar `F(x)`, analisamos cada **intervalo possível**: `x < 0`; `0 ≤ x < 1`; `1 ≤ x < 2`; `x ≥ 2`.

- **Para x < 0:** não existe nenhuma probabilidade acumulada ainda → `F(x) = 0`
- **Para 0 ≤ x < 1:** já passamos pelo ponto x=0, então acumulamos `P(X=0) = 1/3` → `F(x) = 1/3`
- **Para 1 ≤ x < 2:** já passamos por x=0 e x=1, então acumulamos `P(X=0) + P(X=1) = 1/3 + 1/6 = 1/2` → `F(x) = 1/2`
- **Para x ≥ 2:** já passamos por todos os valores possíveis, então a soma é 100% → `F(x) = 1`

Resultado final:

```
        ⎧ 0     , x < 0
        ⎪ 1/3   , 0 ≤ x < 1
F(x) =  ⎨ 1/2   , 1 ≤ x < 2
        ⎪ 1     , x ≥ 2
        ⎩
```

Graficamente, isso forma um **"gráfico de escadas"**: começa em 0, sobe em "saltos" exatamente nos valores 0, 1 e 2, e termina em 1.

---

## 4. Variáveis Aleatórias Contínuas

### 4.1. Função Densidade de Probabilidade — f(x)

Para uma VA **contínua**, não faz sentido falar em "P(X = um valor exato)" usando tabela — os valores possíveis são infinitos. Em vez disso, usamos a **Função Densidade de Probabilidade f(x)**, que precisa satisfazer:

```
f(x) ≥ 0   para todo x ∈ ℝ_X

∫ f(x) dx = 1     (a integral sobre todos os valores possíveis de x é igual a 1)
ℝ_X
```

### A ideia central: probabilidade = área

> No caso de Variáveis Aleatórias Contínuas, a probabilidade é dada pela **área sob a curva** de `f(x)` num intervalo dado. **Não existe probabilidade num ponto, só num intervalo.**

```
        b
P(a<x<b) = ∫ f(x) dx
        a
```

### Explicação simples (analogia)

Pense em `f(x)` como o **"perfil de uma montanha"**: a altura da montanha num ponto específico não te diz "quantas pessoas moram exatamente ali" — só a **área** de uma região da montanha (um pedaço dela) representa uma quantidade real. Da mesma forma, `f(x)` sozinho não é uma probabilidade; só quando você **integra** (calcula a área) em um intervalo é que obtém uma probabilidade.

### Observações fundamentais (muito importantes!)

**1ª) A probabilidade de X ser exatamente igual a um valor `x₀` é ZERO.**

```
        x₀
P(X=x₀) = ∫ f(x) dx = 0      (a "área" de uma linha, sem largura, é zero)
        x₀
```

Consequência direta: **para variáveis contínuas, não importa se o intervalo é aberto ou fechado nas pontas**:

```
P(a ≤ x ≤ b) = P(a ≤ x < b) = P(a < x ≤ b) = P(a < x < b)
```

> **Atenção:** essa regra é **diferente** do caso discreto! No discreto, incluir ou não um ponto específico muda o resultado (veja as propriedades P.3-P.6 da seção 3.2). No contínuo, **não muda**, porque incluir um único ponto não acrescenta área nenhuma.

**2ª) `f(x)` NÃO é uma probabilidade.** Só quando integramos `f(x)` num intervalo `(a,b)` é que obtemos uma probabilidade, igual à área delimitada pela curva, o eixo X e as retas `x=a` e `x=b`.

### 4.2. Função de Repartição de uma VA Contínua

Funciona igual ao caso discreto (é a probabilidade acumulada até `x₀`), mas em vez de **somar**, agora **integramos**:

```
          x₀
F(x₀) =   ∫  f(x) dx
        -∞
```

#### Exemplo 7.3 — verificando e construindo F(x) passo a passo

Seja:

```
        ⎧ (3/2)(1 - x²)   , 0 < x < 1
f(x) =  ⎨
        ⎩ 0               , caso contrário
```

**Passo 1 — verificar se é uma Função Densidade válida:**

- `f(x) ≥ 0`? Para `0 < x < 1`, temos `x² < 1`, logo `1 - x² > 0`, logo `f(x) > 0`. ✔
- A integral é igual a 1?

```
        +∞                0              1              +∞
P =     ∫  f(x)dx   =     ∫ 0 dx    +    ∫ (3/2)(1-x²)dx   +   ∫ 0 dx
       -∞               -∞             0               1

  = (3/2) [ x - x³/3 ]₀¹ = (3/2) × [ (1 - 1/3) - (0 - 0) ] = (3/2) × (2/3) = 1   ✔
```

Logo, `f(x)` **é** uma Função Densidade de Probabilidade válida.

**Passo 2 — construir F(x) por partes:**

- **Para x < 0:** não há área sob a curva ainda → `F(x) = 0`
- **Para 0 ≤ x < 1:**

```
        x                              x
F(x) =  ∫ f(x)dx = 0 + (3/2) ∫ (1-x²) dx = (3/2)(x - x³/3)
      -∞                              0
```

- **Para x ≥ 1:** já somamos toda a área possível (que totaliza 1) → `F(x) = 1`

Resultado final:

```
        ⎧ 0              , x < 0
        ⎪
F(x) =  ⎨ (3/2)(x - x³/3), 0 ≤ x < 1
        ⎪
        ⎩ 1              , x ≥ 1
```

Graficamente, esse `F(x)` é uma curva em **"S"**: começa em 0, sobe suavemente (sem saltos, pois é contínua) e se estabiliza em 1 a partir de x=1.

> **Diferença visual importante:** `F(x)` de uma VA **discreta** é um gráfico em **"escada"** (com saltos). `F(x)` de uma VA **contínua** é uma curva **suave**, sem saltos.

---

## 5. Variáveis Aleatórias Bidimensionais (X,Y)

Até agora trabalhamos com **uma** variável por vez. Mas muitas vezes queremos estudar **duas variáveis ao mesmo tempo** — por exemplo, peso e altura de uma pessoa, ou os resultados de dois dados lançados juntos.

### Definição

> Sejam **E** um Experimento Aleatório, **S** o Espaço Amostral associado, e `X = X(s)` e `Y = Y(s)` duas funções, cada uma associando um número real a cada resultado `s ∈ S`. O par ordenado **(X,Y)** é chamado de **Variável Aleatória Bidimensional**.

### 5.1. Caso Discreto — Distribuição Conjunta

A **Função de Probabilidade Conjunta** `f(x,y)` (ou `p(xᵢ,yⱼ)`) representa `P(X=xᵢ; Y=yⱼ)`, e precisa satisfazer:

```
p(xᵢ,yⱼ) ≥ 0

  +∞   +∞
  Σ     Σ   p(xᵢ,yⱼ) = 1
 i=-∞  j=-∞
```

(é a mesma ideia de antes — probabilidades não negativas e soma total = 1 — só que agora somando em **duas dimensões**, geralmente organizado numa tabela com X nas linhas e Y nas colunas).

#### Exemplo 7.4 — lançamento de dois dados

X e Y representam os pontos obtidos em cada um dos dois dados (de 1 a 6). Como cada combinação `(x,y)` tem a mesma chance, a tabela conjunta é:

| X \ Y | 1 | 2 | 3 | 4 | 5 | 6 |
|---|---|---|---|---|---|---|
| 1 | 1/36 | 1/36 | 1/36 | 1/36 | 1/36 | 1/36 |
| 2 | 1/36 | 1/36 | 1/36 | 1/36 | 1/36 | 1/36 |
| 3 | 1/36 | 1/36 | 1/36 | 1/36 | 1/36 | 1/36 |
| 4 | 1/36 | 1/36 | 1/36 | 1/36 | 1/36 | 1/36 |
| 5 | 1/36 | 1/36 | 1/36 | 1/36 | 1/36 | 1/36 |
| 6 | 1/36 | 1/36 | 1/36 | 1/36 | 1/36 | 1/36 |

Fórmula equivalente:

```
p(xᵢ,yⱼ) = 1/36     para x,y = 1,2,3,4,5,6
```

(Faz sentido: 6 × 6 = 36 combinações possíveis, todas igualmente prováveis.)

### 5.2. Caso Contínuo — Densidade Conjunta

Uma função `f(x,y)` é dita **Função Densidade de Probabilidade Conjunta** de uma VA `(X,Y)` se satisfaz:

```
f(x,y) ≥ 0

 +∞  +∞
 ∫    ∫   f(x,y) dx dy = 1
-∞   -∞
```

### 5.3. Função de Repartição Conjunta — F(x,y)

> A Função de Repartição Conjunta `F(x,y)` é a probabilidade de, **simultaneamente**, X ser menor ou igual a um valor "x" e Y ser menor ou igual a um valor "y":

```
F(x,y) = P(X ≤ x, Y ≤ y)
```

- Se `(X,Y)` é **discreta**:

```
              x₀    y₀
F(x₀,y₀) =    Σ      Σ    p(x,y)
            x=-∞   y=-∞
```

- Se `(X,Y)` é **contínua**:

```
              x₀    y₀
F(x₀,y₀) =    ∫      ∫    f(x,y) dx dy
            -∞    -∞
```

---

## 6. Distribuições de Probabilidades Marginais

### Ideia central

> Dada uma VA Bidimensional `(X,Y)`, podemos determinar a distribuição **de X sem considerar Y**, e a distribuição **de Y sem considerar X**. Para isso, fixamos uma das variáveis e variamos a outra (no caso discreto, "varrendo" a tabela; no caso contínuo, integrando).

### Explicação simples

Pense na tabela do Exemplo 7.4 (dois dados). Se você quiser saber **apenas** a probabilidade do primeiro dado dar "3" (sem se importar com o segundo dado), basta **somar toda a linha X=3** da tabela. Isso é a **distribuição marginal de X**. O nome "marginal" vem justamente do fato de que, se você somar cada linha/coluna e escrever o resultado na "margem" da tabela, obtém essas distribuições.

### Caso Discreto

```
Distribuição de X:   P(X=xᵢ) = P(X=xᵢ ; -∞ < Y < +∞) =  Σ p(xᵢ,yⱼ)     (soma em j)
                                                          j

Distribuição de Y:   P(Y=yⱼ) = P(-∞ < X < +∞ ; Y=yⱼ) =  Σ p(xᵢ,yⱼ)     (soma em i)
                                                          i
```

### Caso Contínuo

```
Distribuição de X:   g(x) =  ∫ f(x,y) dy      (integra em y, "elimina" o y)
                            -∞..+∞

Distribuição de Y:   h(y) =  ∫ f(x,y) dx      (integra em x, "elimina" o x)
                            -∞..+∞
```

---

## 7. Variáveis Aleatórias Independentes

### Definição

> Duas Variáveis Aleatórias, X e Y, são ditas **independentes** se a probabilidade conjunta de `(X,Y)` é igual ao **produto** das probabilidades marginais de X e de Y.

```
Caso Discreto:    p(xᵢ,yⱼ) = p(xᵢ) × p(yⱼ)     para todo i e j

Caso Contínuo:    f(x,y) = g(x) × h(y)          para todo x e y
```

### Explicação simples

É a mesma ideia de independência da probabilidade "comum": se saber o valor de X **não muda nada** sobre as chances de Y (e vice-versa), elas são independentes — e nesse caso, a probabilidade conjunta é simplesmente o **produto** das probabilidades individuais.

---

## 8. Medidas de Posição

Assim como na Estatística Descritiva calculávamos média, mediana e moda de um conjunto de dados, aqui calculamos as **mesmas medidas**, mas para Variáveis Aleatórias — usando probabilidades em vez de frequências.

### 8.1. Esperança Matemática — E[x] (também chamada de "Valor Esperado" ou "Média")

#### Definição

> A Esperança Matemática é a soma de todos os produtos dos valores da Variável Aleatória pelas suas respectivas probabilidades.

```
Se X é Discreta:    E[x] =   Σ   xᵢ × p(xᵢ)
                            i=-∞..+∞

Se X é Contínua:    E[x] =   ∫   x × f(x) dx
                            -∞..+∞
```

### Explicação simples

É exatamente uma **média ponderada**: cada valor possível `x` é "pesado" pela sua probabilidade. Se você repetisse o experimento infinitas vezes e tirasse a média dos resultados, chegaria perto de `E[x]`.

#### Esperança de uma função de X — E[φ(x)]

Podemos calcular a esperança de **qualquer função** `φ(x)` de X (lembre-se da Observação 1 da seção 3.1: uma função de uma VA também é uma VA):

```
Se X é Discreta:    E[φ(x)] =   Σ   φ(xᵢ) × p(xᵢ)
                               i=-∞..+∞

Se X é Contínua:    E[φ(x)] =   ∫   φ(x) × f(x) dx
                               -∞..+∞
```

#### Exemplo 7.5 — calculando E[φ(x)]

Seja `φ(x) = 5x + 3`. A VA X pode assumir os valores -2, 1, 5, 6 e 7, com probabilidades 0,20 / 0,15 / 0,22 / 0,35 / 0,08, respectivamente.

```
E[φ(x)] = Σ (5xᵢ+3) × p(xᵢ)

E[φ(x)] = [5(-2)+3]×0,20 + [5(1)+3]×0,15 + [5(5)+3]×0,22 + [5(6)+3]×0,35 + [5(7)+3]×0,08

E[φ(x)] = (-7)(0,20) + (8)(0,15) + (28)(0,22) + (33)(0,35) + (38)(0,08)

E[φ(x)] = -1,4 + 1,2 + 6,16 + 11,55 + 3,04

E[φ(x)] = 20,55
```

#### Observação — notação μₓ

> Quando a função `φ(x)` é a **Função Identidade** (ou seja, `φ(x) = x`), representamos `E[x]` também por `μₓ`. **Os dois símbolos significam a mesma média**, e qualquer um dos dois pode ser usado.

### 8.1.2. Propriedades da Esperança Matemática

```
P.1)  E[K] = K
      → A esperança de uma CONSTANTE é a própria constante (uma constante não varia, sua "média" é ela mesma)

P.2)  E[K × φ(x)] = K × E[φ(x)]
      → Pode "tirar" uma constante multiplicativa de dentro da esperança

P.3)  E[φ(x) ± θ(x)] = E[φ(x)] ± E[θ(x)]
      → A esperança da soma/diferença é a soma/diferença das esperanças

P.4)  E[(x - μₓ)] = 0
      → A esperança dos DESVIOS em relação à média é sempre ZERO
        (os desvios positivos e negativos se cancelam)

P.5)  Se X e Y são VA Independentes, então:
      E[xy] = μₓ × μᵥ
      → A esperança do PRODUTO é o produto das esperanças (só vale se forem independentes!)
```

> **Atenção:** a propriedade P.5 **só vale se X e Y forem independentes**. Em geral, `E[xy] ≠ E[x] × E[y]`.

### 8.2. Mediana — (Md)

#### Definição

> A Mediana é o valor da Variável Aleatória X que **divide a distribuição em duas partes iguais**, cada uma contendo 50% dos valores. Em outras palavras, a Mediana é o valor cuja Função de Repartição é igual a 0,5.

```
F(Md) = 0,5
```

#### Exemplo 7.6 — Mediana no caso discreto

| X | f(x) | F(x) |
|---|---|---|
| 1 | 0,15 | 0,15 |
| 4 | 0,20 | 0,35 |
| 7 | 0,20 | 0,55 |
| 8 | 0,30 | 0,85 |
| 12 | 0,15 | 1,00 |

Procuramos onde `F(x)` "cruza" 0,5:

- `F(4) = 0,35` → ainda **não** chegou a 0,5
- `F(7) = 0,55` → **ultrapassou** 0,5

Logo, a Mediana é **7** (é o ponto onde a função acumulada passa de menos de 50% para mais de 50%).

> **Dica:** no caso discreto, a Mediana é o **primeiro valor de x** cuja `F(x)` é ≥ 0,5.

#### Exemplo 7.7 — Mediana no caso contínuo

Seja:

```
        ⎧ 3x²  , 0 ≤ x < 1
f(x) =  ⎨
        ⎩ 0    , caso contrário
```

Sua Função de Repartição é:

```
        ⎧ 0   , x < 0
F(x) =  ⎨ x³  , 0 ≤ x ≤ 1
        ⎩ 1   , x > 1
```

Para achar a mediana, resolvemos `F(Md) = 0,5`:

```
x³ = 0,5
x = ∛0,5
Md ≈ 0,7937
```

### 8.3. Moda — (Mo)

#### Definição

> A Moda é o valor da Variável Aleatória X que possui a **maior probabilidade** (no caso discreto) ou que corresponde a um **ponto de máximo** da Função Densidade (no caso contínuo).

- **Caso Discreto:** basta observar o valor de X com maior `P(x)`.
- **Caso Contínuo:** é preciso analisar os pontos onde a `f(x)` tem inclinação (derivada) igual a zero, e estudar os limites à esquerda e à direita para confirmar que são pontos de **máximo**.

> Assim como nas distribuições de frequência, uma distribuição pode ter **mais de uma moda**: nesse caso ela é chamada de **Bimodal**, **Trimodal** ou **Plurimodal**, dependendo do número de valores modais encontrados.

---

## 9. Medidas de Dispersão

### 9.1. Desvio Padrão — σ(x) ou σₓ

#### Definição

> O Desvio Padrão é definido como a **raiz quadrada** da Esperança do quadrado dos desvios em relação à Esperança de X:

```
σ(x) = √( E[ (x - μₓ)² ] )
```

### Explicação simples

O Desvio Padrão mede, em média, **o quão longe** os valores de X costumam estar da sua média `μₓ` — exatamente o mesmo significado que já vimos na Estatística Descritiva, só que agora calculado a partir das probabilidades.

### 9.2. Variância — σ²(x) ou σₓ²

#### Definição

> A Variância é definida como o **quadrado do Desvio Padrão**:

```
σ²(x) = E[ (x - μₓ)² ]
```

```
Se X é Discreta:    σ²(x) = E[(x-μₓ)²] =   Σ   (xᵢ - μₓ)² × p(xᵢ)
                                          i=-∞..+∞

Se X é Contínua:    VAR(x) = E[(x-μₓ)²] =  ∫  (x-μₓ)² × f(x) dx
                                          -∞..+∞
```

#### Fórmula alternativa (mais prática para cálculo)

A Variância também pode ser obtida por:

```
σ²(x) = E[x²] - (μₓ)²
```

> **Macete:** na prática, essa fórmula alternativa (`E[x²] - μₓ²`) costuma ser **muito mais rápida** de calcular do que a fórmula direta com `(x - μₓ)²`, porque evita ter que elevar cada desvio ao quadrado individualmente.

### 9.2.1. Propriedades da Variância

```
P.1)  σ²(K) = 0
      → A Variância de uma CONSTANTE é ZERO (uma constante não varia, então não tem dispersão)

P.2)  σ²(Kx) = K² × σ²(x)
      → Multiplicar a variável por uma constante K multiplica a Variância por K² (atenção ao quadrado!)

P.3)  σ²(x ± K) = σ²(x)
      → Somar ou subtrair uma CONSTANTE não muda a Variância (apenas "desloca" a distribuição, não a espalha mais ou menos)

P.4)  Se X e Y são VA Independentes, então:
      σ²(X ± Y) = σ²(X) + σ²(Y)
      → A Variância da soma/diferença é a SOMA das Variâncias (sempre soma, mesmo na subtração! Só vale se forem independentes)
```

> **Atenção com P.4:** mesmo quando a operação é `X - Y`, a Variância **soma** (não subtrai) as variâncias individuais — desde que X e Y sejam independentes.

### 9.3. Covariância e Coeficiente de Correlação

#### Por que existem?

> Duas ou mais variáveis podem **variar juntas** (ex: altura e peso de pessoas costumam crescer juntos). O objetivo da Covariância e do Coeficiente de Correlação é **medir o grau de relacionamento** entre duas variáveis, que imaginamos estarem ligadas por algum tipo de relação.

#### Covariância — COVxy

```
COVxy = E[ (x - μₓ)(y - μᵥ) ]
```

ou, de forma mais prática:

```
COVxy = E[xy] - μₓ × μᵥ
```

#### Coeficiente de Correlação — ρxy

```
ρxy = COVxy / (σₓ × σᵥ)        onde   -1 ≤ ρxy ≤ +1
```

### Observações importantes

1. **Se X e Y são independentes, então `ρxy = 0`**, e dizemos que não há relação **LINEAR** entre as Variáveis Aleatórias.
2. **A volta não é necessariamente verdadeira!** Se `ρxy = 0`, isso significa que **não existe relação LINEAR** entre X e Y — **porém** isso **não quer dizer** que não possa existir algum tipo de relação **NÃO LINEAR** entre elas (por exemplo, uma relação em forma de "U" pode ter correlação zero e ainda assim ser uma relação muito forte, só que não linear).

> **Resumo da relação Covariância → Correlação:** a Covariância diz **se** existe relação e em **que direção** (positiva = crescem juntas, negativa = uma cresce quando a outra diminui), mas seu valor depende das unidades de medida. O Coeficiente de Correlação "padroniza" esse valor entre -1 e +1, facilitando a interpretação da **força** da relação.

---

## 10. Tabela-Resumo de Fórmulas

| Conceito | Caso Discreto | Caso Contínuo |
|---|---|---|
| Condição de validade | `f(x) ≥ 0` e `Σf(x)=1` | `f(x) ≥ 0` e `∫f(x)dx=1` |
| Função de Repartição | `F(x₀) = Σ f(x)` (até x₀) | `F(x₀) = ∫ f(x)dx` (até x₀) |
| P(X = x₀) | pode ser > 0 | sempre = 0 |
| Esperança E[x] | `Σ xᵢ·p(xᵢ)` | `∫ x·f(x)dx` |
| Esperança de função E[φ(x)] | `Σ φ(xᵢ)·p(xᵢ)` | `∫ φ(x)·f(x)dx` |
| Variância σ²(x) | `Σ (xᵢ-μₓ)²·p(xᵢ)` ou `E[x²]-μₓ²` | `∫ (x-μₓ)²·f(x)dx` ou `E[x²]-μₓ²` |
| Marginal de X | `Σⱼ p(xᵢ,yⱼ)` | `∫ f(x,y)dy` |
| Independência | `p(xᵢ,yⱼ)=p(xᵢ)·p(yⱼ)` | `f(x,y)=g(x)·h(y)` |

**Propriedades-chave para memorizar:**

```
E[K] = K                    σ²(K) = 0
E[Kφ(x)] = K·E[φ(x)]        σ²(Kx) = K²·σ²(x)
E[φ±θ] = E[φ]±E[θ]          σ²(x±K) = σ²(x)
E[(x-μₓ)] = 0                σ²(X±Y) = σ²(X)+σ²(Y)   (se independentes)
E[xy] = μₓ·μᵥ (se independentes)
```

---

## 11. O que mais cai em prova

- **Montar a Distribuição de Probabilidade** P(x) a partir da descrição de um experimento (tabela, gráfico e fórmula — como no Exemplo 7.1).
- **Construir e interpretar a Função de Repartição F(x)**, tanto no caso discreto (gráfico em "escada") quanto contínuo (curva em "S"), e aplicar as propriedades P.1 a P.6 para calcular `P(a<x≤b)`, `P(a≤x≤b)` etc.
- **Verificar se uma função dada é uma Função Densidade válida** (checar `f(x)≥0` e que a integral total seja 1), geralmente envolvendo encontrar uma constante `k` ou `A` desconhecida.
- **Calcular probabilidades de VA contínuas como áreas** (integrais) — e lembrar que `P(X=x₀)=0` e que abrir/fechar os intervalos não muda o resultado.
- **Montar a Função de Repartição de uma VA contínua** integrando `f(x)` por partes (igual ao Exemplo 7.3).
- **Calcular distribuições marginais** a partir de uma tabela conjunta (somando linhas/colunas) ou de uma densidade conjunta (integrando).
- **Verificar independência** entre duas VAs (testar se `p(xᵢ,yⱼ)=p(xᵢ)·p(yⱼ)` ou `f(x,y)=g(x)·h(y)`).
- **Calcular Esperança E[x] e E[φ(x)]** para funções lineares do tipo `φ(x)=ax+b`, usando as propriedades P.1-P.5 para simplificar contas (muito comum em exercícios de "lucro esperado", "valor esperado de venda" etc.).
- **Calcular Mediana**: achar o valor onde `F(x)=0,5` (no discreto, o primeiro x cuja F(x) ≥ 0,5; no contínuo, resolver a equação F(x)=0,5).
- **Calcular Variância e Desvio Padrão**, preferencialmente usando a fórmula `σ²(x)=E[x²]-μₓ²`, e aplicar as propriedades P.1-P.4 para variáveis transformadas (`ax+b`, somas/diferenças de variáveis independentes).
- **Calcular Covariância e Coeficiente de Correlação** a partir de uma distribuição conjunta, e interpretar corretamente o resultado (lembrando que correlação zero não implica ausência total de relação, só de relação linear).
