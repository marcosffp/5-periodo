# 🔔 Distribuição Normal (páginas 171 a 178)

Essa é a distribuição mais famosa e mais usada de toda a Estatística — também chamada de **Distribuição de Gauss**, **de Laplace** ou **Gauss-Laplace**. É aquela curva em formato de sino que aparece em praticamente todo fenômeno natural: alturas, pesos, notas de prova, erros de medição...

$$f(x) = \frac{1}{\sigma\sqrt{2\pi}} \, e^{\frac{-(x-\mu)^2}{2\sigma^2}}, \quad -\infty < x < \infty$$

**Notação:** X ~ N(μ; σ)

## 🎯 Principais características da curva (página 172)

- A curva tem **Média μ** e **Desvio-Padrão σ**;
- **Moda e Mediana coincidem com a Média** (x = μ) — é uma distribuição perfeitamente equilibrada;
- É **simétrica** em relação à reta x = μ (metade dos dados de cada lado);
- Tem pontos de inflexão em x = μ − σ e x = μ + σ (é onde a curva muda de "concavidade");
- É **assintótica ao eixo X**, ou seja, nunca toca o eixo, só se aproxima cada vez mais dele;
- A área total sob a curva é sempre igual a **1** (100% de probabilidade).

**Analogia:** pensa na distribuição das alturas numa sala de aula. A maioria das pessoas fica perto da média, e conforme a altura se afasta da média (pra cima ou pra baixo), fica cada vez mais rara. Isso é a "forma de sino" da Normal.

Quanto **menor o σ**, mais "fina e alta" é a curva (dados concentrados perto da média). Quanto **maior o σ**, mais "achatada e espalhada" ela fica.

---

## 🔄 Variável Normal Padronizada (página 172)

Aqui está o problema prático: como μ e σ podem ser **quaisquer valores**, existem infinitas curvas Normais possíveis — uma para cada combinação de (μ, σ). Seria impossível ter uma tabela pronta para cada uma delas!

**A solução:** transformar qualquer variável Normal X numa variável **padronizada Z**, que sempre tem média 0 e desvio-padrão 1:

$$Z = \frac{X - \mu}{\sigma}$$

Com essa transformação, **todas** as variáveis Normais (não importa o μ e σ originais) podem ser resolvidas usando a **mesma tabela única** de Z. É basicamente uma "tradução universal" para o mesmo idioma.

**Notação:** Z ~ N(0; 1)

---

## 📖 Usando a Tabela da Distribuição Normal (páginas 173 a 176)

A tabela usada aqui é a **"Tabela Reduzida"**: ela só fornece probabilidades do tipo **P(0 < Z < Zα)**, ou seja, a área entre o centro (zero) e um ponto positivo qualquer. Os valores negativos de Z não aparecem porque a curva é **simétrica** — a área de "0 até -1,5" é idêntica à área de "0 até 1,5".

Isso significa que, para qualquer probabilidade pedida, é preciso **montar um "quebra-cabeça"** combinando a área central (0,5 de cada lado da média) com o que a tabela fornece. O Exemplo 9.3 mostra todos os casos possíveis:

### Exemplo 9.3 (páginas 173 a 176) — Os padrões de cálculo com Z

**a) P(0 < Z < 1,825)** → direto da tabela, sem quebra-cabeça:
$$P(0<Z<1,825) = 0,465999$$

**b) P(Z > 1,330)** → a área total à direita do centro é 0,5. Basta tirar a fatia que a tabela já dá:
$$P(Z>1,330) = 0,5 - P(0<Z<1,330) = 0,5 - 0,408241 = 0,091759$$

**c) P(Z < 1,165)** → aqui juntamos a metade esquerda inteira (0,5) com o pedaço até 1,165:
$$P(Z<1,165) = 0,5 + P(0<Z<1,165) = 0,5 + 0,377991 = 0,877991$$

**d) P(0,845 < Z < 1,820)** → intervalo que não começa no zero: pega a área maior e subtrai a menor:
$$P(0,845<Z<1,820) = P(0<Z<1,820) - P(0<Z<0,845) = 0,465620 - 0,300945 = 0,164675$$

**e) P(−1,445 < Z < 0)** → pela simetria, a área de um lado negativo é igual à do lado positivo espelhado:
$$P(-1,445<Z<0) = P(0<Z<1,445) = 0,425771$$

**f) P(Z < −1,150)** → usa simetria + a mesma lógica do item (b):
$$P(Z<-1,150) = P(Z>1,150) = 0,5 - P(0<Z<1,150) = 0,5 - 0,374928 = 0,125072$$

**g) P(Z > −1,385)** → simetria + lógica do item (c):
$$P(Z>-1,385) = P(Z<1,385) = 0,5 + P(0<Z<1,385) = 0,5 + 0,416974 = 0,916974$$

**h) P(−2,020 < Z < −0,645)** → intervalo inteiramente negativo: espelha os dois limites e subtrai como no item (d):
$$P(-2,020<Z<-0,645) = P(0,645<Z<2,020) = P(0<Z<2,020) - P(0<Z<0,645) = 0,478308 - 0,240536 = 0,237772$$

**i) P(−1,175 < Z < 1,640)** → intervalo que atravessa o zero (um lado negativo, outro positivo): soma as duas metades:
$$P(-1,175<Z<1,640) = P(0<Z<1,175) + P(0<Z<1,640) = 0,380003 + 0,449497 = 0,859500$$

**j) P(0 < Z < 2,237)** → quando o Z exato não está na tabela, **aproxima** (nunca arredonda) para o valor mais próximo disponível:
$$P(0<Z<2,237) \approx P(0<Z<2,235) = 0,487291$$

**k) P(0 < Z < 7,325)** → quando Z é maior que o maior valor da tabela (Z = 4,095), a curva já está tão colada no eixo que a probabilidade é praticamente toda a metade:
$$P(0<Z<7,325) \cong 0,5$$

**Resumo dos padrões:** todo problema de Normal se resolve combinando três operações básicas — **soma** (intervalo atravessando o zero), **subtração** (intervalo "fatiado" fora do zero) e **simetria** (espelhar valores negativos para positivos).

---

## 🔍 Exemplo 9.4 (páginas 176-177) — Do problema "direto" ao "de trás pra frente"

X ~ N(23; 7). 

**a) P(X < 20)?** — Aqui o caminho é direto: primeiro **padronizar** X para Z, depois consultar a tabela.

$$Z = \frac{X-\mu}{\sigma} = \frac{20-23}{7} = -0,430$$
$$P(X<20) = P(Z<-0,430) = 0,5 - P(0<Z<0,430) = 0,5 - 0,166402 = 0,333598$$

**b) Qual valor de X divide a distribuição em 80% menores e 20% maiores?** — Esse é o caminho **inverso**: em vez de calcular a probabilidade, temos a probabilidade e precisamos achar o valor de X correspondente.

Como a tabela só trabalha com a área entre 0 e Z, é preciso primeiro descobrir que fatia da tabela precisamos: se queremos 80% abaixo, e metade da curva (50%) já fica abaixo de zero, sobra achar o Z que cobre os **30%** restantes entre 0 e Z (50% + 30% = 80%).

Procura-se na tabela o valor mais próximo de 0,30 → encontra-se Z ≈ 0,840. Depois, "desfaz-se" a padronização para achar X:

$$0,84 = \frac{X-23}{7} \implies X = 7 \times 0,84 + 23 = 28,88$$

Ou seja: 80% dos valores dessa distribuição são menores que 28,88.

**Sacada importante:** sempre que o problema disser "qual valor faz tal porcentagem acontecer", é um problema **inverso** — primeiro se acha o Z na tabela (procurando a probabilidade), depois se converte Z de volta pra X usando a fórmula isolada.

---

## 🧮 A Normal como aproximação da Binomial (página 178)

Assim como a Poisson pode aproximar a Binomial em certos casos (capítulo 8), a **Normal também aproxima a Binomial** quando n é grande e p não está muito perto de 0 nem de 1.

| Medida | Binomial | Normal |
| --- | --- | --- |
| Esperança | np | μ |
| Desvio-Padrão | √(npq) | σ |

Padronizamos a variável Binomial da mesma forma que qualquer Normal:

$$Z = \frac{X-np}{\sqrt{npq}}$$

Só que, como a Binomial é **discreta** e a Normal é **contínua**, é preciso um pequeno ajuste chamado **correção de continuidade** (a constante 0,5), para "converter" um valor discreto pontual num intervalo contínuo:

$$P(a \le X \le b) = P\left(\frac{a-0,5-np}{\sqrt{npq}} \le z \le \frac{b+0,5-np}{\sqrt{npq}}\right)$$

**Analogia:** imagina que X só pode valer números inteiros (discreta) e queremos "emprestar" a régua contínua da Normal para medir essa probabilidade. Para não perder nem sobrar área, esticamos meio passo pra cada lado (±0,5) antes de medir.

### Exemplo 9.5 (página 178) — Comparando os dois métodos

X ~ B(15; 0,4). Queremos P(7 ≤ X ≤ 10).

**Pela Binomial (tabela exata):**
$$P(7 \le X \le 10) = P(X\le 10) - P(X\le 6) = 0,9907 - 0,6098 = 0,3809$$

**Pela Normal (aproximação, com correção de continuidade):**
$$P(7 \le X \le 10) = P\left(\frac{7-0,5-6}{\sqrt{3,6}} \le z \le \frac{10+0,5-6}{\sqrt{3,6}}\right) = P(0,2635 \le z \le 2,3717)$$
$$= 0,988089 - 0,602568 = 0,385521$$

Os dois resultados (0,3809 e 0,385521) ficam bem próximos — prova de que a aproximação funciona bem, e é bem mais rápida de calcular quando n é grande.

---

## 🗺️ Resumindo o capítulo inteiro

| Distribuição | Formato | Quando usar | Esperança | Variância |
| --- | --- | --- | --- | --- |
| **Uniforme** | Retângulo | Todos os valores do intervalo igualmente prováveis | (β+α)/2 | (β-α)²/12 |
| **Exponencial** | Cauda decrescente | Tempo/distância até o próximo evento de um processo de Poisson | 1/λ | 1/λ² |
| **Normal** | Sino simétrico | Fenômenos naturais em geral; aproxima a Binomial para n grande | μ | σ² |

A grande sacada do capítulo é: **toda distribuição contínua vira uma pergunta sobre área sob a curva**, e a Normal é especial porque, ao padronizar qualquer X em Z, conseguimos resolver **qualquer** problema Normal usando uma única tabela.
