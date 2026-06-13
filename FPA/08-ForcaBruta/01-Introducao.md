# 🎒 O Problema da Mochila e Algoritmos de Força Bruta

Este conteúdo está dentro do tema **"Problemas Intratáveis"**: vamos usar o **Problema da Mochila (Knapsack Problem)** como exemplo guia para entender o que é, na prática, um **algoritmo de Força Bruta**.

---

## 💰 O Problema da Mochila (Knapsack Problem) (slides 3 e 4)

Imagina que você vai acampar e tem uma mochila com limite de peso. Você tem vários itens, cada um com um **peso** e um **valor**. O objetivo é escolher quais itens levar para **maximizar o valor total** sem ultrapassar a capacidade da mochila.

**Formalmente:** dados **n** itens, cada um com:

- pesos: p₁, p₂, ..., pₙ
- valores: v₁, v₂, ..., vₙ

E uma mochila de capacidade **C**, o problema é:

> Encontrar o **subconjunto mais valioso** de itens que **caiba** dentro da mochila.

### Por que esse problema é tão importante? (slide 4)

A mochila é uma **generalização** de muitos problemas combinatórios do mundo real. Praticamente qualquer problema que tenha:

- algo a **maximizar** (lucro, valor, eficiência...) → **Valor**
- algo que **limita** as escolhas (peso, tempo, orçamento, espaço...) → **Restrição**

...pode ser modelado como uma variação do problema da mochila. Por isso ele é tão estudado.

---

## 📋 Nosso exemplo de mochila (slide 5)

Vamos usar este cenário ao longo de toda a explicação: **5 itens** e uma mochila de **capacidade C = 15kg**.

| Item | Peso | Valor |
| ---- | ---- | ----- |
| 1 | 12 | $4 |
| 2 | 2 | $2 |
| 3 | 1 | $1 |
| 4 | 4 | $10 |
| 5 | 1 | $2 |

---

## 🔨 O que é um Algoritmo de Força Bruta? (slides 6 a 9)

### Travessia do espaço de busca (slide 7)

> **Força Bruta = travessia completa no espaço de busca.**

Ou seja, o algoritmo passa por **todas** as soluções possíveis do problema (o "espaço de busca"), uma a uma, sem pular nada.

A única exceção é a **travessia parcial** (também chamada de **Early Exit** / interrupção precoce): em alguns casos de sorte, o algoritmo pode encontrar a resposta antes de terminar de percorrer tudo e parar mais cedo. Mas isso é a exceção — no **pior caso**, ele sempre percorre o espaço inteiro.

A força bruta é uma **abordagem direta do problema**: você praticamente "traduz" a definição do problema em código, sem pensar em estratégias mais inteligentes.

### Características (slide 8)

| Característica | O que significa |
| --- | --- |
| **Exploração Exaustiva** | Avalia **todas** as possibilidades dentro do espaço de busca |
| **Interrupção Precoce (Early Exit)** | Em melhores casos, pode parar a travessia antes do fim |
| **Natureza Intuitiva** | É a tradução direta da definição do problema para o algoritmo |
| **Geração do Espaço de Busca** | A lógica é simples de entender, mas **gerar** todas as combinações/permutações pode ser trabalhoso de implementar |
| **Custo Computacional Elevado** | Geralmente resulta em complexidades inviáveis para entradas grandes, como **O(2ⁿ)** (exponencial) ou **O(n!)** (fatorial) |

**Resumindo:** força bruta é fácil de **pensar**, mas cara de **executar** e nem sempre trivial de **implementar**.

---

## ⚖️ Quando a Força Bruta é eficiente x quando é intratável (slide 9)

A força bruta não é sempre ruim — depende do tipo de problema:

| | Eficiente ✅ | Intratável ❌ |
| --- | --- | --- |
| **Tipo de problema** | Problemas **polinomiais** | Problemas **exponenciais** importantes |
| **Exemplos** | Calcular o fatorial de um número, fazer uma busca isolada em um conjunto, calcular somas e médias | Ordenação de dados, caminho mínimo, alocação de recursos |

> ⚠️ **Atenção com o segundo exemplo!** Ordenar dados e achar o caminho mínimo **têm**, sim, algoritmos eficientes conhecidos (Merge Sort, Dijkstra, etc.). O que o slide quer dizer é: **se você tentar resolver esses problemas usando força bruta** — por exemplo, testando *todas* as ordens possíveis de uma lista, ou *todos* os caminhos possíveis entre dois pontos — você cai numa complexidade **exponencial/fatorial**, mesmo que exista uma solução polinomial mais inteligente esperando para ser usada.

Esse é exatamente o motivo de existirem técnicas mais sofisticadas (que vamos ver depois): elas evitam a "armadilha" da força bruta nesses casos.

---

## 🎒 Resolvendo a Mochila com Força Bruta (slides 10 a 14)

O algoritmo geral de força bruta segue **sempre o mesmo roteiro**:

1. **Listar** todas as soluções potenciais para o problema
2. **Avaliar** as soluções, uma a uma
3. **Encontrando** a melhor solução válida, **retornar**

Aplicado à mochila, isso fica mais detalhado (slide 11):

1. **Gerar todos os subconjuntos** de itens
2. **Em cada subconjunto**, avaliar peso e valor
   - Armazenar o subconjunto de **maior valor** entre os que têm **peso válido** (≤ C)
3. **Retornar** o subconjunto armazenado

### Por que gerar *todos* os subconjuntos? (slides 12 a 14)

Com **n** itens, cada item pode estar **dentro ou fora** da mochila — são **2 opções por item**, logo o total de subconjuntos possíveis é **2ⁿ**. Para 5 itens, são **2⁵ = 32** subconjuntos (incluindo o vazio).

Os slides mostram a geração sendo feita de forma organizada, por tamanho crescente de subconjunto:

```
Subconjuntos de 1 item:    {1}, {2}, {3}, {4}, ..., {n}

Subconjuntos de 2 itens:   {1,2}, {1,3}, {1,4}, ..., {1,n}
                            {2,3}, {2,4}, ..., {2,n}
                            {3,4}, ..., {3,n}
                            ...

Subconjuntos de 3 itens:   {1,2,3}, {1,2,4}, ..., {1,2,n}
                            {2,3,4}, ..., {2,3,n}
                            ...

... e assim por diante, até o subconjunto {1,2,3,4,...,n}
```

**Para 5 itens**, isso dá: 5 subconjuntos de 1 item, 10 de 2 itens, 10 de 3 itens, 5 de 4 itens e 1 de 5 itens (+ o conjunto vazio) = **32 no total**. É exatamente esse crescimento combinatório (2ⁿ) que torna a força bruta inviável para n grande.

---

## 🔍 Passo a passo: testando os subconjuntos (slides 15 a 20)

Agora vamos seguir o algoritmo testando, subconjunto por subconjunto, **igual aos slides**. A cada passo guardamos o **melhor subconjunto válido encontrado até agora** (peso total ≤ 15).

### 1️⃣ Subconjuntos de 1 item (slides 15 e 16)

| Subconjunto | Peso Total | Valor Total |
| --- | --- | --- |
| {1} | 12 | 4 |
| {2} | 2 | 2 |
| {3} | 1 | 1 |
| **{4}** | **4** | **10** ⭐ |
| {5} | 1 | 2 |

Todos têm peso ≤ 15 (válidos). O melhor até agora é **{4}**, com **valor 10**.

### 2️⃣ Subconjuntos de 2 itens (slides 17 e 18)

| Subconjunto | Peso Total | Valor Total |
| --- | --- | --- |
| {1,2} | 14 | 6 |
| {1,3} | 13 | 5 |
| {1,4} | **16** 🚫 | 14 |
| {1,5} | 13 | 6 |
| {2,3} | 3 | 3 |
| **{2,4}** | **6** | **12** ⭐ |
| {2,5} | 3 | 4 |
| {3,4} | 5 | 11 |
| {3,5} | 2 | 3 |
| {4,5} | 5 | 12 |

O par {1,4} pesa **16kg**, ultrapassando a capacidade de 15kg → **inválido**, é descartado mesmo tendo valor alto (14).

Entre os válidos, **{2,4}** tem o maior valor (**12**), superando o recorde anterior ({4} = 10). Novo melhor: **{2,4}**, valor 12.

### 3️⃣ Subconjuntos de 3 itens (slides 19 e 20)

| Subconjunto | Peso Total | Valor Total |
| --- | --- | --- |
| {1,2,3} | 15 | 7 |
| {1,2,4} | **18** 🚫 | 16 |
| {1,2,5} | 15 | 8 |
| {1,3,4} | **17** 🚫 | 15 |
| {1,3,5} | 14 | 7 |
| {1,4,5} | **17** 🚫 | 16 |
| {2,3,4} | 7 | 13 |
| {2,3,5} | 4 | 5 |
| **{2,4,5}** | **7** | **14** ⭐ |
| {3,4,5} | 6 | 13 |

Note que vários subconjuntos com valor alto ({1,2,4}=16, {1,4,5}=16, {1,3,4}=15) são **inválidos** porque pesam mais que 15kg.

Entre os válidos, **{2,4,5}** tem o maior valor (**14**), superando o recorde anterior ({2,4} = 12). Novo melhor: **{2,4,5}**, valor 14.

### 4️⃣ Subconjuntos de 4 e 5 itens (continuação do raciocínio)

Seguindo o mesmo processo para os subconjuntos restantes:

| Subconjunto | Peso Total | Valor Total |
| --- | --- | --- |
| {1,2,3,4} | 19 🚫 | 17 |
| {1,2,3,5} | 16 🚫 | 9 |
| {1,2,4,5} | 19 🚫 | 18 |
| {1,3,4,5} | 18 🚫 | 17 |
| **{2,3,4,5}** | **8** | **15** ⭐ |
| {1,2,3,4,5} | 20 🚫 | 19 |

**{2,3,4,5}** pesa 8kg (válido) e tem valor **15**, superando o recorde anterior ({2,4,5} = 14).

### 🏆 Resultado final

Depois de testar **todos os 32 subconjuntos**, a melhor solução válida é:

> **{2, 3, 4, 5}** → peso total **8kg**, valor total **$15**

Esse é o subconjunto que a força bruta retorna — e só "temos certeza" de que é o melhor porque **literalmente todas as outras opções foram testadas**.

---

## ⚡ Formas de implementação e quando usar (slide 21)

A força bruta pode ser implementada tanto na **forma iterativa** quanto na **forma recursiva** — a lógica de "gerar e testar tudo" funciona dos dois jeitos.

Ela é especialmente **útil para**:

- ✅ **Desenvolvimento rápido de algoritmos** — útil como protótipo para validar se a lógica do problema está correta
- ✅ **Entradas pequenas** — quando 2ⁿ ou n! ainda são números administráveis
- ✅ **Algoritmos executados poucas vezes** — quando o custo de "pensar numa solução melhor" não compensa

---

## 🆚 Quando a Força Bruta funciona (e quando não funciona)

### ✅ Funciona bem quando

- O problema tem solução polinomial (ex: calcular fatorial, busca simples, somas)
- A entrada é **pequena** (poucos elementos)
- O algoritmo será executado **poucas vezes**
- Você precisa de um **protótipo rápido** para validar a lógica

### ❌ Não funciona quando

- O problema é exponencial (ex: a mochila com muitos itens)
- A entrada é **grande** (aí O(2ⁿ) ou O(n!) se tornam inviáveis)
- O algoritmo precisa rodar **frequentemente** em produção

**Analogia:** força bruta é como procurar suas chaves verificando **cada centímetro da sua casa**. Funciona se a casa for pequena. Em uma mansão, você precisa de uma estratégia melhor.

---

## 🔗 Conexão com a Teoria da Complexidade

A força bruta é, na prática, o algoritmo exponencial que os slides do material anterior descreviam. Quando dizemos que um problema é **intratável**, estamos dizendo que a única solução conhecida tem complexidade exponencial, como a força bruta.

É exatamente por isso que a **Teoria da Complexidade** existe: para identificar problemas difíceis e saber quando vale a pena procurar uma alternativa mais inteligente, como:

- **Programação dinâmica** (resolve a mochila em O(n·C))
- **Algoritmos gulosos** (aproximações rápidas)
- **Heurísticas** (soluções boas o suficiente em tempo razoável)

---

## 🎯 Resumo Final

| Aspecto | Força Bruta |
| --------- | ------------- |
| **Estratégia** | Travessia completa do espaço de busca: gerar tudo, avaliar tudo, retornar o melhor |
| **Complexidade** | O(2ⁿ) (subconjuntos) ou O(n!) (permutações) |
| **Eficiente em** | Problemas polinomiais (fatorial, busca isolada, somas/médias) |
| **Intratável em** | Problemas exponenciais importantes (ordenação por permutação, caminho mínimo por enumeração, alocação de recursos) |
| **Implementação** | Iterativa ou recursiva |
| **Facilidade de entender** | Alta |
| **Facilidade de implementar** | Média (gerar combinações é trabalhoso) |
| **Quando usar** | Entradas pequenas, protótipos, poucas execuções |

O problema da mochila com força bruta é um exemplo clássico de problema **NP-Difícil**: sabemos resolver (testando tudo), mas não de forma eficiente para entradas grandes. Ele serve como porta de entrada para técnicas mais sofisticadas de projeto de algoritmos — como as **transformações** que veremos a seguir.
