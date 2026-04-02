# Classes de Comportamento Assintótico — Resumo para Prova

Todo algoritmo tem uma função f(n) que descreve quantas operações ele realiza conforme o tamanho da entrada (n) cresce. A notação O(f(n)) representa a complexidade assintótica: o comportamento do algoritmo quando n fica muito grande. Algoritmos com crescimento semelhante pertencem à mesma classe de comportamento, embora existam exceções dentro de cada classe.

As principais classes, da mais eficiente para a pior, são:

O(1) — Constante: o tempo de execução não muda com o tamanho da entrada. As instruções rodam um número fixo de vezes, independente de n.

O(log n) — Logarítmica: muito eficiente. Típica de algoritmos que dividem o problema em partes menores a cada passo. Para n = 1 milhão, o custo é de apenas cerca de 20 operações.

O(n) — Linear: o custo cresce proporcionalmente ao tamanho da entrada. Se n dobra, o tempo dobra. Comum em buscas simples e percurso de vetores.

O(n log n) — Linear-logarítmica: algoritmos que dividem o problema em partes, resolvem cada parte separadamente e depois combinam os resultados. Quando n dobra, o crescimento é um pouco maior que o dobro. Exemplo clássico: algoritmos de ordenação eficientes como Merge Sort.

O(n²) — Quadrática: quando n dobra, o tempo quadruplica. Típica de algoritmos com dois laços aninhados. Ainda aceitável para entradas pequenas e médias.

O(n³) — Cúbica: quando n dobra, o tempo multiplica por 8. Útil apenas para problemas bem pequenos.

O(2ⁿ) — Exponencial: cresce de forma explosiva. Para n = 60, um computador de 1 bilhão de operações por segundo levaria mais de 36 anos. Ocorre em soluções por força bruta.

O(n!) — Fatorial: a pior classe. Cresce mais rápido do que a exponencial. Para n = 40, o tempo necessário equivale a mais de 15 quintilhões de milênios, mesmo no computador mais poderoso do mundo.

A tabela de tempos do material ilustra isso: para n pequeno (10 a 20), até os algoritmos cúbicos rodam em milissegundos. Mas os exponenciais e fatoriais se tornam inviáveis rapidamente.

Para a prova, grave a hierarquia: O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ) < O(n!). Saiba associar cada classe ao seu comportamento ao dobrar n: constante não muda, linear dobra, quadrática quadruplica, cúbica multiplica por 8, exponencial eleva ao expoente. Fique atento à diferença entre O(log n) e O(n log n), pois ambas envolvem divisão do problema, mas a segunda também percorre todos os elementos.
