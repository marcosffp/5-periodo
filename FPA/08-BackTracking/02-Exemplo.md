# 🎒 O Problema da Mochila com Backtracking (slides 17–35)

## O que é o Problema da Mochila? (slides 17 e 18)

O Problema da Mochila (*Knapsack Problem*) é um dos exemplos clássicos para ilustrar o backtracking na prática. A situação é simples de entender:

Você tem **n itens**, cada um com um **peso** e um **valor**. Você tem uma mochila que suporta no máximo uma capacidade **C** de peso. O objetivo é escolher o **subconjunto mais valioso** de itens que caiba dentro da mochila sem estourar o limite de peso.

No exemplo dos slides, temos 5 itens e uma mochila de 15kg:

| Item | Peso | Valor |
| ------ | ------ | ------- |
| 1 | 12kg | $4 |
| 2 | 2kg | $2 |
| 3 | 1kg | $1 |
| 4 | 4kg | $10 |
| 5 | 1kg | $2 |

O desafio é: qual combinação desses itens maximiza o valor total sem passar dos 15kg?

---

## Como o backtracking resolve isso? (slides 19 em diante)

O algoritmo constrói a solução **item por item**, percorrendo uma árvore de combinações. Em cada nó da árvore, temos dois dados importantes:

**CR (Capacidade Restante):** quanto espaço ainda sobra na mochila.
**V (Valor acumulado):** quanto vale a combinação que temos até agora.

A poda acontece quando CR fica negativo — ou seja, quando tentamos colocar um item que não cabe. Nesse momento, o galho inteiro é descartado sem explorar nada mais abaixo dele.

---

## Percorrendo a árvore passo a passo (slides 19 a 35)

Vamos acompanhar o que o algoritmo faz:

**Raiz (estado inicial):** nenhum item escolhido, CR = 15, V = 0.

**Nó {1}:** o algoritmo tenta colocar o item 1 (12kg, $4). Cabe! CR passa para 3, V = 4.

**Nó {1,2}:** tenta adicionar o item 2 (2kg, $2). Cabe ainda! CR = 1, V = 6.

**Nó {1,2,3}:** tenta adicionar o item 3 (1kg, $1). Cabe! CR = 0, V = 7. A mochila está cheia — qualquer próximo item será **podado** porque CR seria negativo. Os nós {1,2,3,4} e {1,2,3,5} são cortados na hora. V = 7 é registrado como melhor solução até aqui.

**Retrocede para {1,2}** e tenta o próximo candidato: o item 5. **Nó {1,2,5}:** item 5 tem 1kg e $2. CR = 0, V = 8. Melhor que 7! A melhor solução atual atualiza para **8**.

**Retrocede para {1}** e tenta adicionar o item 3 diretamente. **Nó {1,3}:** CR = 2, V = 5.

A partir de {1,3}, tenta adicionar o item 4. **Nó {1,3,4}:** item 4 pesa 4kg mas só resta 2kg de espaço — CR ficaria -3. **PODA!** Esse galho é descartado.

Tenta então o item 5. **Nó {1,3,5}:** 1kg, $2. CR = 1, V = 7. Válido, mas não supera o melhor atual de 8.

E assim o algoritmo continua percorrendo os demais galhos da árvore, sempre comparando com o melhor valor encontrado e podando quando a capacidade estoura.

---

## A lógica da poda nesse problema

No problema da mochila, a condição de poda é direta: **se o peso total dos itens escolhidos superar C, o nó é inválido e toda a sua subárvore é descartada**. Não adianta continuar adicionando itens numa mochila que já estourou.

Isso é poderoso porque com 5 itens existem **2⁵ = 32 combinações possíveis** na força bruta. O backtracking elimina boa parte delas sem precisar avaliá-las, especialmente quando itens pesados são escolhidos logo no início da árvore.

**Analogia:** Imagine que você está montando sua mala para uma viagem e testa colocar sua televisão primeiro. Ela já ocupa todo o espaço disponível, então você nem precisa pensar em qualquer combinação que inclua a televisão mais qualquer outra coisa — você já sabe que não cabe.

---

## O que é o "Melhor" que aparece nos slides?

Nos slides, o valor "Melhor" é uma variável global que o algoritmo mantém durante toda a busca. Cada vez que chega a uma folha válida (quando não há mais itens para adicionar ou a mochila está cheia), o algoritmo compara o valor acumulado com o melhor registrado e **atualiza se for superior**.

No exemplo, a evolução foi: Melhor = 7 (ao chegar em {1,2,3}), depois Melhor = **8** (ao chegar em {1,2,5}), e o algoritmo continua para verificar se algum outro galho supera esse valor.

Ao final da exploração completa da árvore, o "Melhor" guardado é a resposta ótima do problema.
