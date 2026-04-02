# Resumo para Prova — Teorema Mestre

**Introdução**

Quando analisamos algoritmos recursivos, precisamos entender como o tempo de execução cresce com a entrada. Uma equação de recorrência descreve esse tempo de forma recursiva. O Teorema Mestre é uma ferramenta que resolve automaticamente equações de recorrência de um formato específico, entregando diretamente o comportamento assintótico do algoritmo — ou seja, sua complexidade usando a notação Theta (Θ).

**Formato da Equação**

O Teorema Mestre se aplica a recorrências do tipo T(n) = a·T(n/b) + f(n), onde a é o número de subproblemas gerados a cada chamada recursiva (a ≥ 1), b é o fator pelo qual o problema é dividido a cada nível (b ≥ 1), e f(n) é o custo do trabalho feito fora das chamadas recursivas, como comparações ou combinações de resultados. O objetivo do teorema é identificar qual das duas partes domina a complexidade total: o custo recursivo, representado por n^log_b(a), ou o custo externo f(n).

**Os Três Casos**

O resultado depende de qual parte cresce mais rápido. No Caso 1, se f(n) cresce mais devagar que n^log_b(a) — formalmente, f(n) = O(n^(log_b(a) − ε)) para algum ε > 0 — então a parte recursiva domina e T(n) = Θ(n^log_b(a)). No Caso 2, se f(n) e n^log_b(a) crescem na mesma velocidade — f(n) = Θ(n^log_b(a)) — então as duas partes empatam e o resultado ganha um fator logarítmico: T(n) = Θ(n^log_b(a) · log(n)). No Caso 3, se f(n) cresce mais rápido que n^log_b(a) — f(n) = Ω(n^(log_b(a) + ε)) — e ainda satisfaz a condição de regularidade a·f(n/b) ≤ c·f(n) para algum c < 1, então f(n) domina e T(n) = Θ(f(n)).

**Exemplo Prático**

Para T(n) = T(n/3) + n, temos a = 1, b = 3 e f(n) = n. Calcula-se n^log_3(1) = n^0 = 1. Como f(n) = n cresce muito mais rápido que 1, estamos no Caso 3, e a complexidade é T(n) = Θ(n).

**O que mais cai em prova**

É fundamental saber identificar os valores de a, b e f(n) a partir da equação, e calcular corretamente n^log_b(a). A seguir, basta comparar o crescimento de f(n) com esse valor e enquadrar no caso correto. Atenção especial ao Caso 3, que exige verificar a condição de regularidade. O teorema não se aplica a equações que não seguem o formato T(n) = a·T(n/b) + f(n).