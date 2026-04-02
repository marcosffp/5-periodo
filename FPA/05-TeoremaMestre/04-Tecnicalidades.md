# Teorema Mestre — Tecnicalidades e Resumo dos Casos

O Teorema Mestre é uma ferramenta para resolver recorrências de algoritmos recursivos do tipo T(n) = aT(n/b) + f(n). O ponto central do teorema é comparar f(n) com a expressão n elevado a log_b(a), chamada aqui de p(n). Essa comparação define qual dos três casos se aplica e, portanto, qual é a complexidade final do algoritmo.

O Caso 1 ocorre quando f(n) é polinomialmente menor que p(n). Não basta ser apenas menor: f(n) precisa ser menor por um fator de n elevado a epsilon, para alguma constante epsilon maior que zero. Nesse caso, a recursão domina e a solução é Theta(n^log_b(a)).

O Caso 2 ocorre quando f(n) e p(n) têm crescimento equivalente. A solução passa a ser Theta(n^log_b(a) vezes log n). O fator logarítmico aparece porque os dois lados do problema contribuem igualmente em todos os níveis da recursão.

O Caso 3 ocorre quando f(n) é polinomialmente maior que p(n), ou seja, f(n) domina. A solução é Theta(f(n)). Porém, há uma exigência adicional chamada condição de regularidade: é preciso que a vezes f(n/b) seja menor ou igual a c vezes f(n), com c menor que 1. Isso garante que, ao descer na árvore de recursão, o custo em cada nível diminui de forma constante, como uma progressão geométrica decrescente, assegurando que f(n) no nível raiz domina de fato todo o custo. Essa condição é satisfeita pela maioria das funções polinomiais comuns.

Uma observação importante: o Teorema Mestre não cobre todos os casos possíveis. Existem funções que ficam em zonas de transição, sendo menores que p(n) mas sem a diferença polinomial exigida pelo Caso 1, ou maiores sem a diferença exigida pelo Caso 3. Nesses casos, ou quando a condição de regularidade falha, o teorema simplesmente não se aplica e é necessário outro método para resolver a recorrência.

Intuitivamente, a lógica do teorema é simples: compara-se f(n) com p(n) e a maior das duas dita o resultado. Se as duas são equivalentes, multiplica-se por log n.

Para a prova, memorize os três casos e suas condições. O ponto mais cobrado é saber identificar qual caso se aplica a uma recorrência dada, calcular corretamente n^log_b(a) e verificar a condição de regularidade no Caso 3 encontrando um c menor que 1. Lembre também que o teorema tem limitações e não se aplica a toda função f(n).
