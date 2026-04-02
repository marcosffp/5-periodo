# Resumo para Prova — Teorema Mestre: Caso 1

**Introdução**

O Teorema Mestre é aplicado a algoritmos recursivos cuja equação de recorrência tem o formato T(n) = a·T(n/b) + f(n). Para determinar a complexidade do algoritmo, o teorema compara o crescimento de f(n) — o custo do trabalho feito fora das chamadas recursivas — com o valor n^log_b(a), que representa o custo acumulado da parte recursiva. O Caso 1 ocorre quando a parte recursiva cresce muito mais rápido do que f(n), ou seja, f(n) é relativamente pequeno.

**Condição do Caso 1**

O Caso 1 se aplica quando f(n) = O(n^(log_b(a) − ε)) para algum ε > 0. Traduzindo: f(n) cresce estritamente mais devagar que n^log_b(a), com uma diferença real entre os expoentes (garantida pelo ε positivo). Quando isso acontece, o custo das chamadas recursivas domina completamente o custo externo, e a complexidade total do algoritmo é T(n) = Θ(n^log_b(a)).

**Passo a passo do exemplo**

A recorrência do exemplo é T(n) = 4·T(n/2) + n. Os parâmetros são a = 4, b = 2 e f(n) = n. O primeiro passo é calcular n^log_b(a): como log_2(4) = 2, temos n^log_2(4) = n². Agora comparamos f(n) = n com n². Claramente, n cresce muito mais devagar do que n². Para confirmar formalmente que estamos no Caso 1, usamos ε = 1: calculamos n^(2 − 1) = n¹ = n, e verificamos que f(n) = n é O(n), ou seja, f(n) está dentro do limite exigido pelo caso. A condição é satisfeita. Portanto, pelo Caso 1, a conclusão é T(n) = Θ(n²).

**Intuição por trás do resultado**

Pense em um algoritmo que divide o problema em 4 subproblemas a cada chamada, mas faz apenas um trabalho linear (n) entre as chamadas. Como cada divisão gera 4 novos problemas, o número de subproblemas cresce muito mais rápido do que o trabalho externo diminui. O custo total acaba sendo dominado pela "explosão" de chamadas recursivas, que no total custa Θ(n²).

**O que mais cai em prova**

O ponto central do Caso 1 é identificar que f(n) cresce mais lentamente que n^log_b(a) com uma folga real (representada por ε > 0). Basta calcular log_b(a), comparar o expoente resultante com o grau de crescimento de f(n), e verificar se existe um ε positivo que satisfaça a condição. Quando sim, a resposta é sempre Θ(n^log_b(a)), ignorando completamente f(n) no resultado final. Saber calcular log_b(a) corretamente é essencial — nesse exemplo, log_2(4) = 2 porque 2² = 4.