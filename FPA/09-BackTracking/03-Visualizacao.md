# 🌳 A Árvore Completa e o Segundo Exemplo (slides 36–41)

## A árvore completa de combinações (slide 36)

Até agora acompanhamos o algoritmo explorando um galho de cada vez. O slide 36 mostra o **panorama completo** de todas as combinações possíveis para os 5 itens, organizado em níveis:

- **Nível 1:** os 5 subconjuntos com 1 item: {1}, {2}, {3}, {4}, {5}
- **Nível 2:** os subconjuntos com 2 itens: {1,2}, {1,3}, {1,4}, {1,5}, {2,3}, {2,4}, {2,5}, {3,4}, {3,5}, {4,5}
- **Nível 3:** subconjuntos com 3 itens
- **Nível 4:** subconjuntos com 4 itens
- **Nível 5:** o único subconjunto com todos os 5 itens: {1,2,3,4,5}

No total, com 5 itens existem **2⁵ = 32 combinações possíveis** (contando o conjunto vazio). Sem backtracking, a força bruta avaliaria todas elas. Com backtracking, boa parte é podada.

---

## A poda em ação na árvore completa (slide 37)

O slide 37 mostra a mesma árvore agora com os **X vermelhos** — os nós que foram podados no exemplo anterior (mochila de 15kg, itens com pesos 12, 2, 1, 4 e 1).

O padrão fica evidente: qualquer combinação que incluía o **item 1 (12kg) junto com o item 4 (4kg)** era imediatamente descartada, porque só esses dois já somam 16kg, estourando o limite de 15kg. Isso elimina de uma vez todos os galhos abaixo de {1,4}, {1,2,4}, {1,3,4}, {1,4,5} e assim por diante.

É exatamente aqui que a elegância do backtracking se manifesta de forma visual: alguns galhos são podados logo no segundo nível da árvore, eliminando automaticamente tudo que cresceria abaixo deles.

---

## Um segundo exemplo com itens diferentes (slides 38 e 39)

Os slides 38 e 39 trocam os pesos dos itens para mostrar como a poda muda dependendo dos dados de entrada. A nova tabela é:

| Item | Peso | Valor |
| ------ | ------ | ------- |
| 1 | 8kg | $4 |
| 2 | 4kg | $2 |
| 3 | 5kg | $1 |
| 4 | 7kg | $10 |
| 5 | 6kg | $2 |

A mochila continua com capacidade de 15kg. Agora nenhum item sozinho ultrapassa o limite — os pesos são mais distribuídos — então a primeira poda só começa a acontecer nas combinações de dois ou mais itens que somem mais de 15kg.

O slide 39 mostra que agora as podas acontecem mais **fundo na árvore**: os nós {1,2,3}, {1,2,4} e {1,2,5} do primeiro galho todos são cortados (o item 1 pesa 8kg e o item 2 pesa 4kg, então 12kg consumidos — adicionar qualquer item de 5, 7 ou 6kg estoura). O nó {1,3,4} também é podado (8+5+7=20kg).

Isso ilustra uma ideia importante: **a eficiência do backtracking depende dos dados**. Quando itens são pesados e a capacidade é apertada, as podas acontecem mais cedo e a busca fica muito mais rápida. Quando os itens são leves, o algoritmo precisa explorar mais fundo antes de podar.

---

## 💡 Para Pensar: O Leilão de Energia (slides 40 e 41)

Os slides terminam com um exercício de raciocínio que transporta a estrutura do problema da mochila para um contexto real: um **leilão de energia elétrica**.

O cenário é o seguinte: uma empresa tem **X Megawatts (MW)** disponíveis para vender. Vários clientes fazem lances, onde cada cliente oferece um valor **V** por um lote exato de **K MW**. A regra é tudo ou nada — o cliente só aceita a quantidade exata que pediu, nada a menos.

O objetivo é escolher a combinação de clientes que maximize o lucro da empresa sem ultrapassar a energia disponível X.

Perceba que esse problema é **estruturalmente idêntico ao problema da mochila**: a energia disponível X é a capacidade C da mochila, cada cliente é um "item", o lote K MW é o "peso" e o valor V do lance é o "valor" do item.

O slide 41 então propõe três perguntas para exercitar o mapeamento para o algoritmo de backtracking:

**Quem é o "estado atual"?** É o conjunto de clientes já aceitos até aquele momento na árvore de busca, junto com a energia restante disponível e o lucro acumulado — exatamente como o CR e o V da mochila.

**Qual é a condição de parada?** Quando todos os clientes já foram considerados (aceitou ou rejeitou cada um deles), ou quando a energia restante chegou a zero e não é possível atender mais nenhum cliente. Nesse ponto registra-se o lucro acumulado e compara com o melhor encontrado até então.

**O que torna um lance "inaceitável" e força a poda?** Se o lote K MW que o cliente pediu for maior que a energia restante disponível — ou seja, se aceitar esse cliente estoura o limite X. Nesse caso, não faz sentido continuar expandindo esse galho da árvore, pois nenhuma combinação que inclua esse cliente será viável.

A sacada do slide é mostrar que o backtracking não é um algoritmo específico para um problema — é um **molde de raciocínio** que se adapta a qualquer problema onde você constrói uma solução passo a passo e consegue detectar cedo quando um caminho é inviável.
