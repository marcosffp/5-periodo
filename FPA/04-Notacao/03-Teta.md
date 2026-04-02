# Resumo para Prova — Notação Theta (Θ)

**Introdução**

Na análise de algoritmos, queremos entender como o tempo de execução ou uso de memória cresce conforme a entrada aumenta. Para isso, usamos notações assintóticas, que descrevem o comportamento de funções para valores grandes de n. A Notação Theta (Θ) é uma das principais ferramentas dessa análise.

**O que é a Notação Theta?**

Quando dizemos que g(n) = Θ(f(n)), estamos afirmando que f(n) é um limite assintótico justo (ou firme) para g(n). Isso significa que g(n) cresce na mesma "velocidade" que f(n) — nem muito mais rápido, nem muito mais devagar. Em linguagem simples: as duas funções têm o mesmo comportamento dominante para entradas grandes.

**Definição Formal**

g(n) é Θ(f(n)) se existirem constantes positivas c1, c2 e um valor mínimo m, tais que, para todo n maior ou igual a m, vale a seguinte desigualdade:

0 ≤ c1 · f(n) ≤ g(n) ≤ c2 · f(n)

Traduzindo: g(n) fica sempre "espremida" entre duas versões escaladas de f(n) — uma por baixo (c1 · f(n)) e outra por cima (c2 · f(n)). A partir de um certo ponto m, essa relação se mantém para sempre.

**Exemplo Prático**

Tome f(n) = n² + 2n e g(n) = 5n² + 10, com c1 = 1 e c2 = 5. A partir de n = 2, verifica-se que g(n) sempre está entre 1·f(n) e 5·f(n). Logo, g(n) = Θ(f(n)), e ambas pertencem à mesma classe de crescimento quadrático.

**Interpretação Gráfica**

No gráfico, c1·f(n) é a curva inferior e c2·f(n) é a curva superior. A partir do ponto m no eixo horizontal, g(n) passa a ficar permanentemente entre essas duas curvas. Antes de m, g(n) pode estar fora dessa faixa — isso é aceitável, pois a notação só exige o comportamento a partir de um certo ponto.

**O que diferencia o Theta das outras notações?**

O Theta é mais preciso do que O (Big-O) e Omega (Ω) isolados. O Big-O fornece apenas um limite superior (g(n) cresce no máximo tão rápido quanto f(n)), enquanto o Omega fornece apenas um limite inferior. O Theta combina os dois: garante que g(n) cresce exatamente na mesma ordem que f(n), por isso é chamado de limite justo.

**O que mais cai em prova**

O ponto central é entender que Theta exige simultaneamente um limite superior e inferior. Para provar que g(n) = Θ(f(n)), é preciso encontrar c1, c2 e m válidos. Funções polinomiais com o mesmo grau dominante são sempre Theta entre si — termos de menor grau e constantes multiplicativas são ignorados na análise assintótica. Também é comum a prova pedir para identificar se uma função é ou não Theta de outra, verificando se as constantes c1 e c2 existem a partir de algum m.