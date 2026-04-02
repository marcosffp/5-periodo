# Resumo para Prova — Teorema Mestre: Caso 2

**Introdução**

O Caso 2 do Teorema Mestre trata da situação de equilíbrio: quando o custo do trabalho externo f(n) e o custo acumulado das chamadas recursivas, representado por n^log_b(a), crescem na mesma velocidade. Nenhum dos dois domina o outro, e o resultado combina os dois em uma complexidade levemente maior, com um fator logarítmico multiplicando a expressão.

**Condição do Caso 2**

O Caso 2 ocorre quando f(n) = Θ(n^log_b(a)), ou seja, f(n) e n^log_b(a) têm o mesmo comportamento assintótico — crescem proporcionalmente para valores grandes de n. Quando isso acontece, a complexidade total do algoritmo é T(n) = Θ(n^log_b(a) · log(n)). Intuitivamente, como nenhuma parte vence, o custo se repete em cada nível da recursão e acumula um fator logarítmico proporcional à profundidade da árvore de chamadas.

**Passo a passo do exemplo**

A recorrência do exemplo é T(n) = 2·T(n/2) + n − 1. Os parâmetros são a = 2, b = 2 e f(n) = n − 1. O primeiro passo é calcular n^log_b(a): como log_2(2) = 1, temos n^log_2(2) = n¹ = n. Agora é preciso verificar se f(n) = Θ(n^log_b(a)), ou seja, se f(n) = Θ(n). Como f(n) = n − 1, e subtrair uma constante não altera a ordem de crescimento, f(n) = n − 1 é assintoticamente equivalente a n, portanto f(n) = Θ(n). A condição do Caso 2 está satisfeita. Aplicando o resultado, a complexidade final é T(n) = Θ(n · log_2(n)).

**Detalhe importante: por que n − 1 é Θ(n)?**

Na análise assintótica, constantes somadas ou subtraídas são irrelevantes para entradas grandes. Para n muito grande, n − 1 e n crescem praticamente igual. Por isso, f(n) = n − 1 pertence à mesma classe de crescimento que n, confirmando f(n) = Θ(n). Esse raciocínio é frequentemente cobrado em prova, então vale guardar bem.

**Diferença em relação aos outros casos**

No Caso 1, f(n) era pequeno demais e a recursão dominava — o resultado era só n^log_b(a). No Caso 3, f(n) domina e o resultado é só f(n). No Caso 2, há empate e o log(n) aparece no resultado como consequência do equilíbrio entre as partes. O Caso 2 é o único que produz esse fator extra de logaritmo.

**O que mais cai em prova**

O ponto crítico é reconhecer quando f(n) e n^log_b(a) têm a mesma ordem de crescimento, mesmo que f(n) tenha termos adicionais como constantes ou pequenas subtrações. Calcular corretamente log_b(a) e simplificar f(n) assintoticamente são passos essenciais. Neste exemplo: log_2(2) = 1, n^1 = n, f(n) = n − 1 ~ Θ(n), logo T(n) = Θ(n · log(n)).