# 🪙 Algoritmos Gulosos — Guia de Estudos

## 1. 🎯 Definição

**Algoritmos Gulosos** (do inglês *greedy algorithms*) são tipicamente usados para resolver **problemas de otimização**. A aplicação é mais adequada em problemas que possuem **subestrutura ótima** (seção 2).

> Um algoritmo guloso sempre faz a escolha que **parece ser a melhor no momento** — uma escolha **localmente ótima**, na esperança de que ela leve a uma solução **globalmente ótima**.

Essa esperança é o ponto central do tema: às vezes funciona perfeitamente (seção 4 — Mochila Exemplo 1), às vezes falha de formas sutis (seções 4 e 5).

---

## 2. 🧱 Subestrutura Ótima

**Definição:** acontece quando uma **solução ótima** para o problema contém, em seu interior, **soluções ótimas para subproblemas**.

### Exemplo — Caminho mais curto

Imagine viajar de carro da **Cidade A** até a **Cidade C**, passando por uma cidade intermediária **B**. Suponha que a rota A→B→C seja a mais rápida possível (a solução ótima).

**Raciocínio (prova por contradição):**

1. O trajeto de A até B, dentro dessa rota ótima, **precisa obrigatoriamente** ser o caminho mais rápido possível entre A e B (o subproblema).
2. Se existisse um atalho mais rápido entre A e B que **não** foi usado, esse atalho poderia substituir o trecho A→B, tornando a viagem completa A→C ainda mais rápida. Mas isso contradiz a premissa de que A→C já era a rota mais rápida possível.
3. Logo, a solução ótima do problema maior (A até C) **contém em seu interior** a solução ótima do subproblema (A até B).

**Analogia:** se o caminho mais rápido do trabalho até a praia passa pelo shopping, então o trecho "casa → shopping" precisa, ele mesmo, ser o caminho mais rápido até o shopping — senão você trocaria esse trecho por um melhor e a viagem inteira ficaria mais rápida ainda.

> ⚠️ **Atenção:** subestrutura ótima é uma **propriedade do problema**, não uma garantia de que um algoritmo guloso vai resolvê-lo corretamente. A seção 6 mostra um problema (Mochila 0-1) que **tem** subestrutura ótima, mas onde o guloso **falha**.

---

## 3. 🧩 Critério Guloso e Algoritmo Geral

**Critério guloso:** a cada passo, verifica-se o conjunto de candidatos atuais (sem visão do problema todo) e seleciona-se o candidato **mais promissor** — uma **decisão local**.

- A função de seleção pode ser **maximizadora** (Max: maior ganho local) ou **minimizadora** (Min: menor custo local).

**Algoritmo geral (4 passos):**

1. **Listar** todos os candidatos no passo atual.
2. Usando o **critério guloso**, **selecionar** o melhor local.
3. **Adicionar** o melhor local na solução e **retirá-lo** do conjunto de candidatos.
4. **Repetir** até chegar a uma solução.

```
Função AlgoritmoGulosoGeral(Candidatos)
    Solucao <- {}

    Enquanto (Solucao não estiver completa) e (Candidatos não estiver vazio) faça:
        // Listar candidatos e, usando o critério guloso, selecionar o melhor
        MelhorCandidato <- SelecionarMelhor(Candidatos)

        // Adicionar o melhor local/candidato na solução
        Solucao <- Solucao + {MelhorCandidato}

        // Retirá-lo do conjunto de candidatos
        Candidatos <- Candidatos - {MelhorCandidato}
    Fim Enquanto

    Retornar Solucao
Fim Função
```

Note que esse algoritmo **nunca reconsidera** uma escolha já feita — diferente da força bruta (que testa tudo) e da D&C (que recombina subproblemas), o guloso **anda em uma única direção, sem volta**. É essa "miopia" que torna o guloso rápido, mas também a fonte de todos os problemas vistos a seguir.

---

## 4. 🗺️ Exemplo: O Caixeiro Viajante (TSP)

**Problema:** um caixeiro viajante precisa visitar **N** cidades, passando por cada uma exatamente uma vez e retornando à origem, com o **menor custo total**.

**Modelagem como problema de otimização:**

| Elemento | Definição no TSP |
|---|---|
| **Função Objetivo** | Minimizar a distância total do percurso completo |
| **Restrições** | Visitar cada cidade exatamente uma vez; começar e terminar na mesma cidade |

**Critério guloso:** a cada iteração, viajar para a **cidade vizinha não visitada mais próxima** (menor custo imediato).

```
Função CaixeiroViajanteGuloso(C, D)
    cidadeInicial <- EscolhaDoUsuario()
    ci <- cidadeInicial
    Caminho <- {ci}
    DistanciaTotal <- 0
    MarcarComoVisitada(ci)

    Enquanto (houver cidades não visitadas em C) faça:
        // Critério Guloso: seleciona a vizinha disponível de menor custo
        cj <- SelecioneCidadeNaoVisitadaMaisProximaDe(ci, D)
        Caminho <- Caminho + {cj}
        DistanciaTotal <- DistanciaTotal + Distancia(ci, cj)
        MarcarComoVisitada(cj)
        ci <- cj
    Fim Enquanto

    // Fechamento do ciclo (restrição do TSP)
    Caminho <- Caminho + {cidadeInicial}
    DistanciaTotal <- DistanciaTotal + Distancia(ci, cidadeInicial)
    Retornar Caminho, DistanciaTotal
Fim Função
```

### 🌍 O grafo de exemplo

4 cidades (A, B, C, D), todas interligadas, com as seguintes distâncias:

| | A | B | C | D |
|---|---|---|---|---|
| **A** | — | 20 | 10 | 15 |
| **B** | 20 | — | 35 | 30 |
| **C** | 10 | 35 | — | 25 |
| **D** | 15 | 30 | 25 | — |

### Exemplo 1 — começando em D

| Passo | Cidade atual | Candidatos (custo) | Escolhido | Caminho | Distância |
|---|---|---|---|---|---|
| 1 | D | A=15, B=30, C=25 | **A (15)** | {D,A} | 15 |
| 2 | A | B=20, C=10 | **C (10)** | {D,A,C} | 25 |
| 3 | C | B=35 (D,A já visitados) | **B (35)** | {D,A,C,B} | 60 |
| 4 | B | retorno a D = 30 | **D (30)** | {D,A,C,B,D} | **90** |

### Exemplo 2 — começando em A

| Passo | Cidade atual | Candidatos (custo) | Escolhido | Caminho | Distância |
|---|---|---|---|---|---|
| 1 | A | B=20, C=10, D=15 | **C (10)** | {A,C} | 10 |
| 2 | C | B=35, D=25 | **D (25)** | {A,C,D} | 35 |
| 3 | D | B=30 (A,C já visitados) | **B (30)** | {A,C,D,B} | 65 |
| 4 | B | retorno a A = 20 | **A (20)** | {A,C,D,B,A} | **85** |

### 🔍 Análise: ótimo local ≠ ótimo global

Os dois exemplos usam o **mesmo grafo** e o **mesmo critério guloso**, mas partindo de cidades diferentes chegam a resultados diferentes: **90** (partindo de D) vs **85** (partindo de A).

> Comparando com **todos** os ciclos possíveis do grafo (força bruta): existem apenas 3 ciclos distintos, com custos **85, 90 e 95**. O resultado partindo de A (85) coincidiu com o **ótimo global** — mas isso foi "sorte" do ponto de partida, não uma garantia do algoritmo. Partindo de D, o guloso ficou **preso em 90**, sem nunca saber que existia uma rota melhor.

**A armadilha da escolha local:** o algoritmo busca o ótimo local a cada passo, mas ignora o panorama geral. Fazer a melhor escolha *agora* (D→A custando só 15) pode obrigar o caixeiro a usar uma aresta cara no final só para satisfazer a restrição de "voltar à origem" (B→D custando 30). Como mostram os slides: **se a distância até a cidade restante for muito grande, a solução é obrigada a escolher esse caminho.**

```
Ótimo local  ≠  Ótimo global
```

---

## 5. 🎒 Exemplo: O Problema da Mochila (Knapsack 0-1)

**Objetivo:** maximizar o valor somado de um conjunto de itens sem exceder a capacidade da mochila. Itens são **indivisíveis** — leva-se o item inteiro (1) ou nada (0).

### 5.1 Critério 1: maior valor — Exemplo 1 (funciona)

| Item | Peso | Valor |
|---|---|---|
| 1 | 12 | 4 |
| 2 | 2 | 2 |
| 3 | 1 | 1 |
| 4 | 4 | 10 |
| 5 | 1 | 2 |

Capacidade C = 15 (o mesmo cenário da mochila vista em `08-ForcaBruta/01-Introducao.md`, onde a força bruta encontrou `{2,3,4,5}` = peso 8, valor **15**).

**Critério guloso, alternativa 1:** ordenar os itens por **valor decrescente** e ir adicionando o que couber.

Ordem: Item4(10) → Item1(4) → Item5(2) → Item2(2) → Item3(1)

| Passo | Item avaliado | Cabe? (peso ≤ restante) | Mochila | Valor | Restante |
|---|---|---|---|---|---|
| 0 | — | — | {} | 0 | 15 |
| 1 | 4 (peso 4) | sim | {4} | 10 | 11 |
| 2 | 1 (peso 12) | **não** (12 > 11) | {4} | 10 | 11 |
| 3 | 5 (peso 1) | sim | {4,5} | 12 | 10 |
| 4 | 2 (peso 2) | sim | {4,5,2} | 14 | 8 |
| 5 | 3 (peso 1) | sim | {4,5,2,3} | **15** | 7 |

**Resultado:** `{4,5,2,3}` = valor **15** → **idêntico ao ótimo da força bruta!** 🎉 Aqui o guloso por valor funcionou.

### 5.2 Critério 1 falha — Exemplo 2

Troca-se apenas o peso do Item 1 (de 12 para 11):

| Item | Peso | Valor |
|---|---|---|
| 4 | 4 | 10 |
| 1 | 11 | 4 |
| 5 | 1 | 2 |
| 2 | 2 | 2 |
| 3 | 1 | 1 |

Capacidade C = 15. Mesma ordem por valor decrescente: 4(10) → 1(4) → 5(2) → 2(2) → 3(1)

| Passo | Item avaliado | Cabe? | Mochila | Valor | Restante |
|---|---|---|---|---|---|
| 0 | — | — | {} | 0 | 15 |
| 1 | 4 (peso 4) | sim | {4} | 10 | 11 |
| 2 | 1 (peso 11) | sim (11 ≤ 11) | {4,1} | **14** | 0 |

A mochila ficou **cheia** (`Restante: 0`) e o algoritmo para. **Resultado guloso: `{4,1}` = valor 14.**

Mas existe uma solução melhor: `{4,5,2,3}` = peso 4+1+2+1=8, valor 10+2+2+1=**15** > 14!

> **Quem errou? O algoritmo ou quem projetou?** A resposta é o **critério**: ele desconsiderou completamente a variável **peso**. Escolher o item de maior valor (Item 1, valor 4) consumiu **11 dos 15 kg** disponíveis para ganhar apenas 4 de valor — um péssimo "custo por kg".

### 5.3 Critério 2: razão Valor/Peso (V/P) — Exemplo 2 revisitado

**Critério guloso, alternativa 2:** selecionar o item ainda não escolhido que **caiba** na mochila e tenha a **maior razão valor/peso**.

| Item | Peso | Valor | V/P |
|---|---|---|---|
| 4 | 4 | 10 | 2,5 |
| 5 | 1 | 2 | 2 |
| 3 | 1 | 1 | 1 |
| 2 | 2 | 2 | 1 |
| 1 | 11 | 4 | 0,36 |

Ordem por V/P decrescente: 4(2,5) → 5(2) → 3(1) → 2(1) → 1(0,36)

| Passo | Item avaliado | Cabe? | Mochila | Valor | Restante |
|---|---|---|---|---|---|
| 0 | — | — | {} | 0 | 15 |
| 1 | 4 (peso 4) | sim | {4} | 10 | 11 |
| 2 | 5 (peso 1) | sim | {4,5} | 12 | 10 |
| 3 | 3 (peso 1) | sim | {4,5,3} | 13 | 9 |
| 4 | 2 (peso 2) | sim | {4,5,3,2} | 15 | 7 |
| 5 | 1 (peso 11) | **não** (11 > 7) | {4,5,3,2} | **15** | 7 |

**Resultado:** `{4,5,3,2}` = valor **15** → agora **bate com o ótimo**! O critério V/P corrigiu o problema do Exemplo 2.

**Analogia:** valor absoluto é como escolher pelo "preço da etiqueta"; V/P é como escolher pelo "preço por quilo" — o segundo é quase sempre uma medida melhor de eficiência quando o espaço (peso) é o recurso escasso.

### 5.4 Mas… até o V/P pode falhar

| Item | Peso | Valor | V/P |
|---|---|---|---|
| 1 | 10 | 60 | 6 |
| 2 | 20 | 100 | 5 |
| 3 | 30 | 120 | 4 |

Capacidade C = 50. Os itens já estão em ordem de V/P decrescente: 1(6) → 2(5) → 3(4)

| Passo | Item avaliado | Cabe? | Mochila | Valor | Restante |
|---|---|---|---|---|---|
| 0 | — | — | {} | 0 | 50 |
| 1 | 1 (peso 10) | sim | {1} | 60 | 40 |
| 2 | 2 (peso 20) | sim | {1,2} | **160** | 20 |
| 3 | 3 (peso 30) | **não** (30 > 20) | {1,2} | **160** | 20 |

**Resultado guloso (V/P):** `{1,2}` = valor **160**, sobrando 20 kg vazios.

**Mas a solução ótima real é** `{2,3}` = peso 20+30=50 (mochila **cheia**), valor 100+120=**220**!

> Mesmo o critério "mais inteligente" (V/P) pode falhar: pegar o Item 1 primeiro (a melhor razão individual) **consome espaço e bloqueia** a combinação Item2+Item3, que juntos formam a solução ótima.

---

## 6. 🔬 Subestrutura Ótima vs. Guloso: por que ele falha?

A grande pergunta após o exemplo 5.4: **se o guloso falhou, será que a Mochila 0-1 não tem subestrutura ótima?**

**Resposta: tem, sim.** Vamos provar com os números do exemplo 5.4 (capacidade 50, solução ótima global = `{2,3}` = valor 220):

1. **Reduzindo para o subproblema:** suponha que decidimos levar o **Item 3** (peso 30, valor 120). O que sobra para resolver é uma "mochila menor": **nova capacidade = 50 − 30 = 20**, com os itens restantes disponíveis sendo {Item 1, Item 2}.
2. **A solução do subproblema está contida no todo:** qual é a solução ótima absoluta para uma mochila de capacidade 20 com {Item1, Item2}? É levar o **Item 2** (peso 20, valor 100) — Item1 (peso10) sozinho daria só 60.
3. **Conclusão:** a solução ótima do problema maior (`{2,3}` = 220) contém, em seu interior, a solução ótima do subproblema de capacidade 20 (`{2}` = 100). **120 + 100 = 220.** ✅ Isso é exatamente a definição de subestrutura ótima.

> 🧠 **A lição central:** o problema da Mochila 0-1 **tem** subestrutura ótima. O motivo do algoritmo guloso falhar **não é a falta de subestrutura**, mas sim o fato de que **escolhas locais míopes** (como pegar o Item 1 primeiro, por ter a melhor razão V/P individual) **quebram o caminho** para alcançar essa solução ótima — a decisão tomada no passo 1 não pode mais ser desfeita.

Essa distinção é o que a literatura chama de **"propriedade da escolha gulosa" (greedy-choice property)**: além de ter subestrutura ótima, um problema só é resolvido corretamente por um algoritmo guloso se **a escolha localmente ótima no primeiro passo fizer parte de pelo menos uma solução globalmente ótima**. A Mochila 0-1 tem o primeiro requisito, mas não o segundo.

---

## 7. 💬 O Foco dos Algoritmos Gulosos

> *"One thing you will notice about greedy algorithms is that they are usually easy to design, easy to implement, easy to analyze, and they are very fast, but they are almost always difficult to prove correct."*
> — **Ian Parberry**

Essa frase resume o trade-off:

| Vantagem | Cuidado |
|---|---|
| Fácil de **projetar** | A escolha "óbvia" nem sempre é a correta |
| Fácil de **implementar** | Poucas linhas, mas correção exige prova cuidadosa |
| Fácil de **analisar** | Complexidade geralmente baixa (ex: O(n log n) pela ordenação) |
| **Muito rápido** | Rapidez não significa corretude — **provar** que funciona é o difícil |

---

## 8. ⚖️ Guloso vs. Força Bruta

| Aspecto | Força Bruta | Guloso |
|---|---|---|
| **Estratégia** | Testa **todas** as combinações possíveis | Faz **uma única passada**, escolhendo o melhor candidato local a cada passo |
| **Garantia de ótimo** | Sempre encontra o ótimo global | **Não garante** — depende do problema ter a propriedade da escolha gulosa |
| **Complexidade** | Exponencial/fatorial (ex: O(2ⁿ) na mochila) | Geralmente polinomial (ex: O(n log n)) |
| **Reversibilidade** | N/A (avalia tudo) | **Nunca volta atrás** numa escolha já feita |
| **Quando usar** | Entradas pequenas, precisão garantida | Entradas grandes, quando o problema tem subestrutura ótima **e** propriedade da escolha gulosa (ex: Mochila Fracionária, Huffman, Dijkstra, Kruskal/Prim) |

---

## 9. 📝 Exercícios Propostos

### Exercício 1 — Bin Packing (transporte de cargas)

Uma empresa precisa transportar cargas em um caminhão com caçamba de capacidade **42 m³**. As cargas têm volumes:

```
16, 12, 13, 11, 10, 12, 9, 8, 15, 14  (m³)
```

A empresa quer fazer **no máximo 3 viagens**. Projete e execute um algoritmo guloso (ex: ordenar as cargas e ir preenchendo cada viagem até não couber mais nada, passando para a próxima viagem).

### Exercício 2 — Problema do Troco (Coin Change)

Dado um sistema monetário **S** com moedas pré-definidas, qual é a **menor quantidade de moedas** para retornar um troco **C**? (Critério guloso clássico: sempre usar a maior moeda possível que ainda caiba no troco restante.)

- **A)** S = {1, 5, 10, 25, 50, 100}, C = 147
- **B)** S = {1, 5, 15, 20, 50, 100}, C = 180

> 💡 **Dica conceitual:** o problema do troco é o exemplo clássico onde o critério "sempre a maior moeda" **funciona perfeitamente** para sistemas como o (A) — moedas "bem comportadas" — mas **pode falhar** para sistemas arbitrários como o (B). Vale testar ambos e comparar com a quantidade mínima real (por força bruta/programação dinâmica) para o caso (B).

---

## 10. 🎯 Resumo Final

| Aspecto | Algoritmos Gulosos |
|---|---|
| **Estratégia** | A cada passo, escolher o candidato localmente ótimo (critério guloso) e nunca reconsiderar |
| **Pré-requisito ideal** | Problema com **subestrutura ótima** |
| **Garantia de corretude** | Só se o problema também tiver a **propriedade da escolha gulosa** |
| **Complexidade** | Geralmente baixa (ordenação + uma passada → O(n log n)) |
| **Quando funciona** | Mochila Fracionária, Huffman, Dijkstra, Kruskal/Prim, alguns sistemas de troco |
| **Quando falha** | Mochila 0-1 (mesmo com critério V/P), TSP, troco com moedas "mal comportadas" |
| **Maior risco** | Escolhas precoces e irreversíveis que bloqueiam a solução ótima |
| **Citação-chave** | "Fácil de projetar e implementar, difícil de provar correto" (Ian Parberry) |

---

## 11. 🧠 Resumo Mental

```
Guloso funciona bem quando:
  ✅ O problema tem subestrutura ótima
  ✅ A escolha localmente ótima do 1º passo pertence
     a alguma solução globalmente ótima (greedy-choice property)
  ✅ Não há necessidade de "voltar atrás"

Guloso não funciona bem quando:
  ❌ A escolha local "gasta recursos" de forma que bloqueia
     combinações melhores no futuro (Mochila 0-1)
  ❌ O resultado depende fortemente do ponto de partida (TSP)
  ❌ Subestrutura ótima existe, mas a propriedade da escolha
     gulosa não (subestrutura ótima ≠ guloso funciona!)
```
