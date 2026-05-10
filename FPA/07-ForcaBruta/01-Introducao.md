# 💼 O Problema da Mochila (Knapsack Problem)

Imagina que você vai acampar e tem uma mochila com limite de peso. Você tem vários itens, cada um com um **peso** e um **valor**. O objetivo é escolher quais itens levar para **maximizar o valor total** sem ultrapassar a capacidade da mochila.

Formalmente: dados n itens com pesos p₁, p₂, ..., pₙ e valores v₁, v₂, ..., vₙ, e uma mochila de capacidade C, encontre o **subconjunto mais valioso** que caiba.

Esse problema é uma generalização de muitos problemas combinatórios do mundo real, onde sempre há um **valor a maximizar** e uma **restrição a respeitar**.

---

## 🔨 Algoritmos de Força Bruta

A ideia da força bruta é simples e direta: **tente tudo**. Não há inteligência nem estratégia, apenas exploração completa do espaço de busca.

### Características

**Exploração exaustiva:** avalia todas as possibilidades possíveis sem exceção.

**Interrupção precoce (Early Exit):** em alguns casos favoráveis, pode parar antes de testar tudo, quando já encontrou o que precisava.

**Natureza intuitiva:** o algoritmo é uma tradução direta da definição do problema. Se o problema diz "encontre o melhor subconjunto", força bruta gera todos os subconjuntos e compara.

**Implementação trabalhosa:** apesar de conceitualmente simples, modelar e gerar todas as combinações ou permutações pode ser complicado na prática.

**Custo computacional altíssimo:** geralmente resulta em complexidade exponencial O(2ⁿ) ou fatorial O(n!), o que inviabiliza o uso para entradas grandes.

---

## 🎒 A Mochila com Força Bruta

O algoritmo funciona em três passos gerais:

1. **Listar** todas as soluções potenciais (todos os subconjuntos de itens)
2. **Avaliar** cada subconjunto, verificando peso e valor
3. **Retornar** o subconjunto com maior valor cujo peso seja válido

### Por que todos os subconjuntos?

Com n itens, cada item pode estar **dentro ou fora** da mochila. São 2 opções por item, então o número total de subconjuntos é **2ⁿ**. Para 5 itens, são 32 subconjuntos. Para 30 itens, são mais de 1 bilhão. O crescimento é explosivo.

### Exemplo prático

Considere 5 itens e uma mochila de capacidade 15kg:

| Item | Peso | Valor |
| ------ | ------ | ------- |
| 1 | 12kg | $4 |
| 2 | 2kg | $2 |
| 3 | 1kg | $1 |
| 4 | 4kg | $10 |
| 5 | 1kg | $2 |

A força bruta testa todos os subconjuntos possíveis. Alguns exemplos:

- **{4}** → peso 4, valor $10 ✅ (candidato)
- **{2, 4}** → peso 6, valor $12 ✅ (candidato melhor)
- **{2, 4, 5}** → peso 7, valor $14 ✅ (ainda melhor)
- **{2, 3, 4, 5}** → peso 8, valor $15 ✅ (melhor ainda)
- **{1, 2, 4}** → peso 18, valor $16 ❌ (estoura a mochila)

Depois de testar tudo, a melhor solução válida encontrada é **{2, 3, 4, 5}** com peso 8 e valor $15.

---

## ⚡ Quando a Força Bruta funciona (e quando não funciona)

### ✅ Funciona bem quando

- O problema tem solução polinomial (ex: calcular fatorial, busca simples, somas)
- A entrada é **pequena** (poucos elementos)
- O algoritmo será executado **poucas vezes**
- Você precisa de um **protótipo rápido** para validar a lógica

### ❌ Não funciona quando

- O problema é exponencial (ex: a mochila com muitos itens)
- A entrada é **grande** (aí O(2ⁿ) ou O(n!) se tornam inviáveis)
- O algoritmo precisa rodar **frequentemente** em produção

**Analogia:** force bruta é como procurar suas chaves verificando **cada centímetro da sua casa**. Funciona se a casa for pequena. Em uma mansão, você precisa de uma estratégia melhor.

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
| **Estratégia** | Testa todas as possibilidades |
| **Complexidade** | O(2ⁿ) ou O(n!) |
| **Facilidade de entender** | Alta |
| **Facilidade de implementar** | Média (gerar combinações é trabalhoso) |
| **Eficiência prática** | Baixa para entradas grandes |
| **Quando usar** | Entradas pequenas, protótipos, poucas execuções |

O problema da mochila com força bruta é um exemplo clássico de problema **NP-Difícil**: sabemos resolver, mas não de forma eficiente para entradas grandes. Ele serve como porta de entrada para técnicas mais sofisticadas de projeto de algoritmos.
