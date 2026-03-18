# RELÓGIOS LÓGICOS — RESUMO PARA PROVA

-- Link do mateiral: <https://cristianoneto.online/puc/damd/aula05>

**Introdução**

Em sistemas distribuídos, vários computadores executam tarefas ao mesmo tempo, cada um com seu próprio relógio físico. O problema é que esses relógios nunca estão perfeitamente sincronizados. O *clock skew* é a diferença entre os relógios de máquinas distintas; o *clock drift* é o fato de que cada relógio avança em velocidade ligeiramente diferente. Resultado: um evento posterior pode ter um timestamp menor que um evento anterior, gerando inconsistências. A solução proposta por Leslie Lamport em 1978 foi radical: abandonar o tempo real e usar apenas a *ordem* dos eventos.

**Happens-Before e Causalidade**

Lamport criou a relação "happens-before" (→), que diz que um evento "a" pode ter influenciado "b". Ela funciona por três regras: dentro do mesmo processo, o que ocorre antes precede o que vem depois; o envio de uma mensagem sempre precede seu recebimento; e a relação é transitiva. Se dois eventos não têm nenhuma relação de precedência entre si, são chamados de *concorrentes* (a ∥ b) e aconteceram de forma independente.

**Relógio de Lamport**

Cada processo mantém um contador inteiro iniciado em zero. A regra R1 diz que, antes de qualquer evento interno, o processo incrementa seu contador. A regra R2 diz que, ao enviar uma mensagem, o processo incrementa o contador e envia o valor junto com a mensagem. A regra R3 diz que, ao receber uma mensagem, o processo atualiza seu contador para o maior valor entre o seu contador local e o timestamp recebido, e então incrementa mais um. Isso garante que se a → b, então C(a) < C(b). A limitação crítica: a recíproca não vale. Se C(a) < C(b), isso não significa que a causou b — podem ser eventos concorrentes. Lamport não detecta concorrência.

**Relógios Vetoriais**

Proposto independentemente por Fidge e Mattern em 1988/1989. Em vez de um único número, cada processo mantém um vetor com um contador por processo. O próprio contador só é incrementado pelo processo dono. Ao receber uma mensagem, o processo atualiza cada posição do vetor com o máximo entre o valor local e o valor recebido, depois incrementa sua própria posição. A grande vantagem é o bicondicional: a → b se e somente se V(a) < V(b), onde V(a) < V(b) significa que todos os valores de V(a) são menores ou iguais aos de V(b), com pelo menos um estritamente menor. Se os vetores são incomparáveis, os eventos são concorrentes. Isso Lamport não consegue fazer.

**Comparação Direta**

O relógio de Lamport usa um único inteiro (custo fixo, O(1)) e serve bem para ordenação de eventos e exclusão mútua distribuída. O relógio vetorial usa um vetor de tamanho N (cresce com o número de processos, O(N)) e é necessário quando é preciso detectar conflitos e concorrência com precisão, como em sistemas de replicação.

**Aplicações Reais**

O Amazon Dynamo usa relógios vetoriais para controlar versões de objetos. Quando dois vetores são incomparáveis, o sistema retorna ambas as versões ao cliente, que decide como reconciliar (no carrinho de compras, une os itens). O Git implementa o mesmo conceito: cada branch é um processo independente, e um merge commit equivale ao recebimento de uma mensagem. O Cassandra faz a escolha oposta e usa timestamp físico com last-write-wins, correndo o risco de perda silenciosa de dados em caso de dessincronia de relógios.

**O que mais cai em prova**

Saiba aplicar as três regras do Lamport passo a passo, especialmente o R3 (max + 1 no recebimento). Saiba comparar dois vetores e concluir se há precedência causal ou concorrência. Lembre que Lamport garante só uma direção (a → b implica C(a) < C(b)), enquanto o vetorial garante as duas. E saiba o que o Dynamo faz quando encontra versões concorrentes: retorna ambas para o cliente resolver.