## Backtracking Parte 2: Critérios de Poda

**Contexto geral**

Backtracking é uma técnica de busca recursiva usada em problemas combinatórios onde não existe algoritmo eficiente. A ideia é explorar candidatos a solução e recuar (retroceder) quando um caminho se mostra inviável. Esta parte trata de dois problemas clássicos e do mecanismo de poda, que torna a busca muito mais eficiente.

**Passeio do Cavalo**

Um cavalo de xadrez deve percorrer todas as casas de um tabuleiro NxN exatamente uma vez e retornar ao ponto inicial. O algoritmo tenta cada movimento possível e, quando fica sem opções válidas, retrocede ao estado anterior e tenta outra direção. A árvore de decisão cresce rapidamente, tornando a poda essencial.

**Problema das N Rainhas**

Consiste em posicionar N rainhas num tabuleiro NxN sem que nenhuma ataque outra. O algoritmo posiciona uma rainha por coluna e verifica restrições antes de avançar: mesma linha, mesma coluna ou mesma diagonal invalidam a posição. Quando todas as posições de uma coluna são inviáveis, o algoritmo retrocede para a coluna anterior e tenta a próxima opção disponível. Para alguns valores de N não existe solução, e o algoritmo pode voltar até a raiz.

**Eficiência do Backtracking**

No melhor caso, muitas falhas ocorrem cedo, a poda elimina grandes ramos da árvore e o algoritmo resolve problemas intratáveis por força bruta. No pior caso, as restrições só falham no fim dos caminhos, a árvore é percorrida quase inteira e o comportamento se aproxima da força bruta, com complexidade exponencial O(2^n) ou fatorial O(n!).

**Mecanismo de Poda (Pruning)**

Poda é o uso de uma função de avaliação para descartar ramos da árvore de busca que certamente não levarão a uma solução boa. Existem dois tipos. A poda garantida baseia-se em restrições matemáticas exatas e jamais descarta uma solução ótima. A poda estimada usa heurísticas (estimativas inteligentes), acelera muito a busca, mas pode, em casos raros, descartar a melhor solução.

**Exemplo Prático: Mochila e Caixeiro Viajante**

Na Mochila, a poda garantida descarta combinações de itens cuja capacidade restante já foi excedida, nunca sacrificando a solução ótima.

No Caixeiro Viajante, usa-se poda estimada. Calcula-se a média de custo por aresta, multiplica-se pelas arestas restantes e soma-se ao custo atual. Se essa projeção superar a melhor solução já encontrada, o ramo é descartado. Para evitar cortes incorretos causados pela imprecisão da média, adiciona-se uma margem de tolerância. O limite real de corte passa a ser: melhor solução + margem.

**Considerações finais**

Backtracking funciona melhor em problemas onde as restrições eliminam candidatos cedo. Quanto mais candidatos passam pelas verificações sem falhar, mais o algoritmo se aproxima de uma busca exaustiva. A poda é o principal mecanismo para fugir desse pior caso. A escolha entre poda garantida e estimada define o trade-off entre corretude absoluta e velocidade.

**O que mais cai em prova:** diferença entre poda garantida e estimada; funcionamento do backtracking no problema das N rainhas; conceito de best-so-far e margem de tolerância no Caixeiro Viajante; e a relação entre qualidade da poda e complexidade resultante do algoritmo.