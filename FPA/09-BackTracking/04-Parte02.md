# 🌲 Backtracking Parte 2: O Passeio do Cavalo, as N Rainhas e Critérios de Poda

Na primeira parte vimos a estrutura geral do backtracking — a função `Tenta(estado_atual)` com seus cinco passos (condição de parada, geração de candidatos, poda, recursão e retrocesso) — e um primeiro exemplo com o problema da Mochila. Nesta segunda parte, os slides aplicam essa mesma estrutura a **dois problemas clássicos novos** (Passeio do Cavalo e N Rainhas) e depois mergulham a fundo no que diferencia um backtracking "OK" de um backtracking **realmente eficiente**: os **critérios de poda**.

---

## 🐴 O Passeio do Cavalo (slides 2 a 10)

### O problema

> Dado um tabuleiro com **n x n** posições e um **cavalo** que se movimenta segundo as regras do xadrez: a partir de uma posição inicial **(x₀, y₀)**, encontrar, **se existir**, um caminho que visite **todas as casas do tabuleiro uma única vez** e **volte ao ponto inicial**.

Em outras palavras: é um **ciclo** que passa por todas as casas exatamente uma vez (um "circuito fechado" usando apenas movimentos de cavalo).

### Conjunto de movimentos (slide 4)

A partir de qualquer casa, o cavalo tem **até 8 movimentos possíveis** em forma de "L" — os mesmos 8 saltos do xadrez, em todas as direções. Esse conjunto de movimentos é o que vai gerar os **candidatos** em cada chamada de `Tenta`.

### Encaixando no algoritmo geral `Tenta(estado_atual)`

Vale a pena revisitar a função genérica do [01-Conceito.md](01-Conceito.md) com este problema concreto em mente:

| Parte do algoritmo | No Passeio do Cavalo |
| --- | --- |
| **`estado_atual`** | Posição atual do cavalo no tabuleiro + conjunto de casas já visitadas |
| **`solucaoDefinitiva`** | Todas as n² casas foram visitadas **e** a última casa permite voltar à posição inicial |
| **`gerarCandidatos`** | Os até 8 movimentos de cavalo possíveis a partir da posição atual |
| **`solucaoAceitavel` (poda)** | A casa de destino está **dentro do tabuleiro** e **ainda não foi visitada** |
| **`registraCandidato`** | Marca a casa de destino como visitada e move o cavalo para lá |
| **`apagaRegistroAnterior`** | Desmarca a casa (ela volta a ficar disponível) e devolve o cavalo à posição anterior, para tentar o próximo movimento da lista |

### O exemplo visual (slides 5 a 10)

Os slides mostram um tabuleiro **5x5** com o cavalo começando no centro:

1. **Slide 6** — a partir da posição inicial, são exibidos os movimentos possíveis (as 8 setas do "conjunto de movimentos").
2. **Slide 7** — o cavalo executa o **primeiro movimento**: a casa de origem fica marcada em **vermelho** (visitada) e a seta usada fica **acinzentada** (esse movimento específico, a partir daquela casa, já foi consumido).
3. **Slide 8** — **segundo movimento**: mais uma casa fica marcada em vermelho, e o conjunto de movimentos disponíveis a partir da nova posição é recalculado.
4. **Slides 9 e 10** — **terceiro movimento**: aqui a árvore de busca **se ramifica de fato** — a partir da terceira casa existem múltiplos movimentos candidatos, e o slide mostra o algoritmo testando um deles (linha cheia) enquanto deixa indicada, por uma **linha pontilhada**, a continuação da exploração recursiva.

> 🤔 **Analogia do dia a dia:** imagine percorrer todos os cômodos de uma casa labiríntica, sem repetir nenhum, e precisando voltar ao cômodo de onde você começou no final. A cada cômodo, você anota numa lista as portas por onde ainda não passou (candidatos). Se entrar num cômodo e perceber que dali não dá pra continuar (todas as portas levam a cômodos já visitados), você **volta pelo caminho que veio** (apaga a marca do cômodo anterior) e tenta outra porta.

A poda aqui é simples, mas essencial: **nunca pular para fora do tabuleiro e nunca revisitar uma casa**. Sem essa verificação antes de avançar, o algoritmo geraria movimentos completamente inúteis.

---

## 👑 O Problema das N Rainhas (slides 11 a 32)

### O problema (slides 12 a 15)

A **Rainha do xadrez** pode se mover em **qualquer direção** (horizontal, vertical, diagonal), **quantas casas quiser**.

> **O problema:** colocar **8 rainhas** em um tabuleiro de xadrez de modo que **nenhuma possa atacar a outra**.

Isso significa: nenhuma rainha pode compartilhar **linha**, **coluna** ou **diagonal** com outra. Os slides mostram dois tabuleiros 8x8 diferentes, cada um com uma disposição válida de 8 rainhas — provando que **existe mais de uma solução**.

### Backtracking passo a passo: primeira tentativa (slides 16 a 20)

A estratégia é posicionar as rainhas **uma a uma**, coluna por coluna, e a cada nova rainha testar se a posição escolhida entra em conflito (mesma linha, coluna ou diagonal) com alguma rainha já posicionada. Cada par **(linha, coluna)** representa a posição testada.

| Passo | Tentativa | Resultado |
| --- | --- | --- |
| 1 | Posiciona a 1ª rainha em **(0,0)** | ✅ ponto de partida |
| 2 | Tenta **(0,1)** para a 2ª rainha | ❌ mesma linha → poda |
| 3 | Tenta **(1,1)** | ❌ mesma diagonal → poda |
| 4 | Escolhe a próxima opção, **(2,1)**, e avança para a 3ª rainha | ✅ aceita |
| 5 | *(eventualmente)* o retrocesso pode chegar até a **raiz** (solução inicial) | — |

Cada "❌ poda" é exatamente o passo 3 (`solucaoAceitavel`) da função `Tenta`: a posição é descartada **antes** de gastar tempo explorando o que viria depois dela.

### Quando a busca da 2ª/3ª rainha recomeça (slides 22 a 27)

Os slides seguem o raciocínio mostrando uma sequência de tentativas e podas ao buscar posições para as próximas rainhas:

| Passo | Tentativa | Resultado |
| --- | --- | --- |
| 1 | Tenta **(0,2)** | ❌ mesma linha → poda |
| 2 | Tenta **(1,2)** | ❌ mesma diagonal → poda |
| 3 | Tenta **(1,3)** | ❌ mesma linha → poda |
| 4 | Tenta **(1,4)** | ❌ mesma diagonal → poda |
| 5 | Tenta e **posiciona** em **(1,5)** | ✅ aceita |

> 🤔 **"É uma boa abordagem?"** — essa é a pergunta que o próprio slide faz nesse ponto. Repare quantas tentativas falharam (4 podas!) antes de conseguir posicionar **uma única rainha**. O backtracking *funciona* e *sempre encontra* uma solução se ela existir, mas, sem critérios de poda mais inteligentes, ele pode gastar muito tempo em becos sem saída antes de progredir — esse é exatamente o gancho para a seção de **Critérios de Poda** mais adiante.

### Quando uma coluna inteira fica sem opções (slides 28 a 31)

Pode acontecer de, ao tentar posicionar uma rainha em uma determinada coluna (no exemplo, a **coluna 5**), **nenhuma linha** estar livre — todas as linhas daquela coluna são atacadas por rainhas já posicionadas (o slide 28 mostra setas saindo de cada rainha já colocada e cobrindo a coluna inteira).

Quando isso acontece, o backtracking entra em ação:

1. **Retrocesso para o último passo** (slide 29): desfaz a posição da última rainha colocada e tenta a **próxima opção de linha** para ela (slide 30 destaca, com um círculo vermelho, a rainha que está sendo retestada).
2. **Em último caso, retrocede até a raiz** (slide 31): se nem essa rainha tem mais opções de linha, ela também é desfeita, e o processo se repete para a rainha anterior — e assim por diante, em cascata. No diagrama, os **X vermelhos** marcam posições que já foram tentadas e descartadas nesse processo de retrocesso em cadeia.

> ⚠️ Esse é o **pior caso** do backtracking na prática: o algoritmo "sobe" e "desce" várias vezes na árvore antes de encontrar um novo caminho promissor — ou de concluir que, a partir daquele ponto, **não há solução** (o que obriga a desfazer ainda mais rainhas).

### Generalizando: o Problema das N Rainhas (slide 32)

> Colocar **N** rainhas em um tabuleiro **N x N**, de modo que nenhuma possa atacar a outra. **Para algumas instâncias de N, o problema não tem solução** (por exemplo, não existe solução para N = 2 ou N = 3).

Isso é importante: o backtracking precisa lidar com o caso em que o retrocesso esgota **todas** as possibilidades e volta até a raiz sem nunca encontrar uma `solucaoDefinitiva` — nesse caso, a resposta correta é "não existe solução para este N".

---

## ✂️ Critérios de Poda (slides 33 a 38)

### Backtracking e Poda: dois cenários opostos (slide 34)

| | ✅ Cenário Ideal (Alta Eficiência) | ❌ Pior Caso (Gargalo Computacional) |
| --- | --- | --- |
| **O que acontece** | Ocorrem muitas falhas **prematuras** (condições inaceitáveis logo no início) | As regras do problema geram muitas soluções candidatas / "falsos positivos" que só falham **no final** |
| **Efeito na árvore** | A poda elimina **grandes "galhos"** da árvore de busca | A árvore de busca é percorrida **quase em sua totalidade** |
| **Resultado** | O algoritmo resolve problemas que seriam **intratáveis por força bruta pura** | O comportamento e o tempo de execução se aproximam da **Força Bruta**, com complexidades exponenciais (**O(2ⁿ)**) ou fatoriais (**O(n!)**) |

A diferença entre esses dois cenários **não está no algoritmo em si**, mas em **quão rápido** o algoritmo consegue detectar que um caminho não vale a pena.

### A tentativa de fuga do pior caso (slide 35)

> **Tentativa de fuga do pior caso de tempo:** estabelecer critérios **mais agressivos** de poda.

Ou seja: se o backtracking "puro" (só verificando as regras básicas do problema) já está perto do pior caso, a saída é criar **funções de avaliação extras** que cortem ramos da árvore **antes** mesmo de violar uma regra básica — cortar por *suspeita* de que aquele caminho não será bom, e não apenas por *certeza* de que é inválido.

### O Mecanismo de Poda — Pruning (slides 36 a 38)

> **Conceito:** utilização de uma **função de avaliação** para reduzir o espaço de busca, cortando antecipadamente subproblemas (ramos da árvore) que **não levarão ao objetivo**.

Existem **dois tipos de critérios**:

#### 1) Poda Garantida (Exata)

- Baseia-se em **restrições matemáticas** e **limites lógicos** do próprio problema.
- **Garantia:** é **100% segura**. **Jamais** descarta um caminho que contenha a solução ótima.

#### 2) Poda Estimada (Heurística)

- Baseia-se em **estimativas**, **palpites inteligentes** ou **funções de probabilidade**.
- **Trade-off:** acelera **drasticamente** a busca, mas abre mão da **corretude absoluta** — pode, em casos raros, descartar acidentalmente a melhor solução.

| | Poda Garantida | Poda Estimada |
| --- | --- | --- |
| **Base** | Restrições matemáticas exatas do problema | Heurísticas, médias, estimativas |
| **Corretude** | 100% segura | Pode errar (cortar a solução ótima) |
| **Velocidade** | Boa, mas limitada pelas restrições reais | Muito mais agressiva/rápida |
| **Exemplo** | Mochila (peso excedido) | Caixeiro Viajante (custo estimado) |

---

## 🎒 Poda Garantida na prática: revisitando a Mochila (slide 39)

O slide 39 mostra uma **árvore de decisão** para o mesmo problema da Mochila visto em [02-Exemplo.md](02-Exemplo.md) (5 itens, C = 15kg), destacando visualmente onde a **poda garantida** age:

```
*  (raiz)              CR: 15, V: 0
└── {1}                CR: 3,  V: 4
    ├── {1,2}          CR: 1,  V: 6
    │     ├── {1,2,3}  ❌ (e seus descendentes, todos podados)
    │     ├── {1,2,4}  ❌ podado (peso excede a capacidade)
    │     └── {1,2,5}
    └── {1,3}          CR: 2,  V: 5
          ├── {1,3,4}  ❌ podado (peso excede a capacidade)
          └── {1,3,5}  CR: 1,  V: 7
```

No canto do slide aparece **"Melhor: 8"** — exatamente o valor de **{1,2,5}**, que é a melhor solução encontrada até aquele ponto da busca (compatível com o "Melhor = 8" explicado em 02-Exemplo.md).

Os ramos marcados com **X** são casos em que a **Capacidade Restante (CR)** já ficou **negativa** ao adicionar aquele item — ou seja, **matematicamente impossível** que qualquer extensão futura daquele ramo seja válida. Por isso o algoritmo nem perde tempo gerando os filhos desses nós: é uma **poda garantida**, baseada puramente na restrição de peso ≤ C.

> ✅ Note que essa poda nunca descarta uma solução melhor que a já conhecida — ela só descarta ramos que **já são inviáveis por definição**.

---

## 🗺️ Poda Estimada na prática: o Problema do Caixeiro Viajante (slides 40 a 62)

Agora o exemplo muda para o **Problema do Caixeiro Viajante (PCV / TSP)**: encontrar o **ciclo de menor custo** que visita todas as cidades uma única vez e volta à cidade de origem. Aqui, diferente da Mochila, **não existe uma restrição "dura"** como a capacidade — então a poda precisa ser **estimada**.

### O grafo de exemplo (slide 40)

5 cidades (A, B, C, D, E) com as seguintes distâncias:

| Aresta | Custo |
| --- | --- |
| A–B | 2 |
| A–E | 1 |
| B–C | 7 |
| B–D | 2 |
| C–D | 1 |
| C–E | 7 |
| D–E | 2 |

### Calculando a média de custo por aresta (slide 41)

- **Soma de todos os custos do grafo:** 2+1+7+2+1+7+2 = **22**
- **Número de arestas:** 7
- **Média por aresta:** 22 ÷ 7 ≈ **3,14**

Esse valor (**3,14**) vai funcionar como a **"régua" da heurística**: para cada aresta que ainda falta percorrer num caminho parcial, o algoritmo **estima** que ela vai custar, em média, 3,14.

### A heurística de projeção

Para qualquer caminho parcial:

- **T** = custo real já percorrido até agora
- **arestas_restantes** = quantas arestas ainda faltam para fechar o ciclo (sempre 5 no total, pois há 5 vértices)
- **E (Esperado)** = arestas_restantes × 3,14
- **Projeção** = T + E

A ideia: **Projeção** é uma estimativa de "quanto vai custar esse caminho no total, se as arestas restantes custarem na média". Se essa projeção já é **pior** que a melhor solução conhecida (*Best so far*), **não vale a pena continuar** por esse caminho.

### Caminho 1 — uma volta completa: A → B → C → D → E → A (slides 43 a 46)

| Passo | Aresta | T (acumulado) | Arestas restantes | E = restantes × 3,14 |
| --- | --- | --- | --- | --- |
| A | — | 0 | 5 | 15,7 |
| A→B | 2 | 2 | 4 | 12,56 |
| B→C | 7 | 9 | 3 | 9,42 |
| C→D | 1 | 10 | 2 | 6,28 |
| D→E | 2 | 12 | 1 | 3,14 |
| E→A | 1 | **13** | 0 | 0 |

O ciclo se completa com **custo total 13**. Esse valor passa a ser o **Melhor_Solução** (*Best so far* = 13) — a referência que todos os outros caminhos vão precisar superar.

### Backtracking #1 — voltando em C, tentando C→E em vez de C→D (slides 47 a 48)

- C→E custa 7, então T = 9 + 7 = **16**
- Restam 2 arestas → E = 2 × 3,14 = 6,28
- **Projeção = 16 + 6,28 = 22,28**

Como 22,28 é **muito maior** que o Melhor_Solução (13), esse ramo é marcado como **"nada promissor"** e **podado (❌)** — nem vale a pena seguir explorando a partir de E.

### Backtracking #2 — voltando em B, tentando B→D em vez de B→C (slides 49 a 51)

- B→D custa 2, então T = 2 + 2 = **4**
- Restam 3 arestas → E = 3 × 3,14 = **9,42**
- **Projeção = 4 + 9,42 = 13,42**

13,42 é **pior que 13** (o Melhor_Solução). **"Logo não seguiremos esse caminho"** — ramo podado (❌).

### Backtracking #3 — voltando até A, tentando A→E em vez de A→B (slides 50 a 53)

- A→E custa 1, então T = **1**
- Restam 4 arestas → E = 4 × 3,14 = **12,56**
- **Projeção = 1 + 12,56 = 13,56**

Aqui está o ponto mais interessante de toda a explicação: **13,56 também é pior que 13**. Se o algoritmo fosse **100% rígido**, ele **teria podado o vértice E imediatamente**, no primeiro passo — sem nem considerar esse caminho.

#### A solução: a Margem de Tolerância (slide 53)

> Como o valor 3,14 é **apenas uma média** (uma heurística), ele **não é perfeito**. Pode ser que, por pura sorte, as próximas arestas a partir de E sejam incrivelmente baratas. Para não ser tão agressivo e evitar cortar uma solução ótima por causa de alguns centésimos (0,56 de diferença), o autor do algoritmo adiciona uma **Margem de Tolerância (Margem = 1)**.

Isso significa que o **novo limite de corte** passa a ser:

> **Melhor_Solução + Margem = 13 + 1 = 14**

Como **13,56 < 14**, esse caminho **não é podado** — o algoritmo continua explorando a partir de E.

### Continuando a partir de E, com o novo limite 14 (slides 54 a 61)

**E→C** (slide 54):

- C custa 7 a partir de E, então T = 1 + 7 = **8**
- Restam 3 arestas → E = 3 × 3,14 = **9,42**
- **Projeção = 8 + 9,42 = 17,42**

17,42 > 14 → **"nada promissor"**, ramo podado (❌).

**E→D** (slide 55/61):

- D custa 2 a partir de E, então T = 1 + 2 = **3**
- Restam 3 arestas → E = 3 × 3,14 = **9,42**
- **Projeção = 3 + 9,42 = 12,42**

12,42 < 14 → **"ainda promissor"** — a busca continua por esse ramo.

> 🤔 **Analogia do dia a dia:** é como planejar uma viagem de carro e ter uma estimativa de "R$ 3,14 por km, em média". Se você já gastou R$ 13 numa rota de 5 km e percebe que uma rota alternativa de 5 km custaria, na pior das hipóteses, R$ 13,56 — quase igual ao que você já gastou — vale a pena **conferir essa alternativa**, porque a estimativa pode estar errada para melhor. Mas se uma rota alternativa já projeta R$ 22, claramente não compensa nem checar.

### O algoritmo completo: `BuscaCaixeiro` (slide 62)

Juntando tudo, os slides apresentam o pseudocódigo completo da busca com poda estimada:

```
// Parâmetros Globais do Algoritmo
Média_Aresta = 3.14
Melhor_Solução = 13  // Best So Far
Margem = 1

FUNÇÃO BuscaCaixeiro(vértice_atual, T, arestas_restantes) {

    // 1. Cálculo da Projeção de Custo (A Heurística)
    E = arestas_restantes * Média_Aresta
    Projeção = T + E

    // 2. O CRITÉRIO DE PODA (Branch and Bound)
    se Projeção > (Melhor_Solução + Margem) {
        retorne FALSO  // Fim da linha! O backtracking ocorre aqui.
    }

    // 3. Condição de Parada (encontrou um ciclo completo válido)
    se arestas_restantes == 0 {
        se T < Melhor_Solução {
            Melhor_Solução = T  // Atualiza o novo limite
        }
        retorne VERDADEIRO
    }

    // 4. Geração de Candidatos (Ramificação)
    para cada próximo_vértice a partir de vértice_atual {
        se próximo_vértice não foi visitado {
            Marca próximo_vértice como visitado
            Novo_T = T + peso(vértice_atual, próximo_vértice)

            // Avança para o próximo nível da árvore
            BuscaCaixeiro(próximo_vértice, Novo_T, arestas_restantes - 1)

            // Desfaz o movimento para testar o próximo irmão (Retrocesso)
            Desmarca próximo_vértice
        }
    }
}
```

Comparando com a função genérica `Tenta(estado_atual)` da Parte 1:

| Parte genérica (`Tenta`) | Parte específica (`BuscaCaixeiro`) |
| --- | --- |
| `solucaoDefinitiva` | `arestas_restantes == 0` |
| `gerarCandidatos` | `para cada próximo_vértice a partir de vértice_atual` |
| `solucaoAceitavel` (poda) | `se Projeção > (Melhor_Solução + Margem) → retorne FALSO` |
| `registraCandidato` | `Marca próximo_vértice como visitado` + `Novo_T = ...` |
| `apagaRegistroAnterior` | `Desmarca próximo_vértice` |

A novidade em relação à Mochila é que aqui a poda (passo 2) acontece **logo no início** da função, **antes** mesmo de checar se chegamos à solução — e ela é baseada numa **estimativa**, não numa restrição exata.

---

## 📝 Considerações Finais sobre Backtracking (slides 63 a 66)

- **Geralmente aplicado a problemas combinatórios** para os quais **não existem algoritmos eficientes** conhecidos.
- A **recursividade** provê uma forma simples de resolver problemas complexos — mas fica uma **pendência**: a **geração das soluções candidatas** (`gerarCandidatos`) precisa ser pensada caso a caso para cada problema.
- A **verificação de extensão** (a poda, `solucaoAceitavel`) **precisa ser eficiente** — se ela mesma for cara de calcular, pode anular o ganho de performance da poda.
- **O sucesso do backtracking varia bastante** de problema para problema, e até **entre instâncias diferentes do mesmo problema** (como vimos: para alguns valores de N, o problema das N Rainhas nem tem solução).
- ⚠️ **Quanto mais soluções candidatas existirem em cada passo, mais o algoritmo se aproxima da busca exaustiva** (força bruta) — a poda é o que evita essa aproximação.
- Backtracking pode ter **excesso de memória** em problemas grandes (a recursão e o controle de estados visitados consomem espaço).
- **Limitações e heurísticas:** a poda descarta ramos inviáveis (ou pouco promissores), **reduzindo bastante o tempo total** de execução — mas, como vimos na poda estimada, isso pode envolver um trade-off entre velocidade e garantia de otimalidade.

---

## 🎯 Resumão

| Conceito | O que é |
| --- | --- |
| **Passeio do Cavalo** | Encontrar um ciclo que visite todas as casas de um tabuleiro NxN uma única vez, usando movimentos de cavalo, voltando ao início |
| **Problema das N Rainhas** | Posicionar N rainhas em um tabuleiro NxN sem que nenhuma ataque outra; nem sempre tem solução |
| **Retrocesso em cadeia** | Quando uma coluna/posição fica sem candidatos válidos, desfaz a rainha anterior (e a anterior, e a anterior...) até achar uma nova opção ou chegar à raiz |
| **Poda Garantida** | Baseada em restrições matemáticas exatas; 100% segura; nunca descarta a solução ótima (ex: peso da Mochila) |
| **Poda Estimada** | Baseada em heurísticas/médias; muito mais agressiva, mas pode (raramente) descartar a melhor solução (ex: custo médio por aresta no Caixeiro) |
| **Best so far / Melhor_Solução** | Melhor resultado encontrado até o momento; serve como referência para podar caminhos piores |
| **Margem de tolerância** | Folga adicionada ao limite de corte (`Melhor_Solução + Margem`) para compensar imprecisões da heurística |
| **Cenário ideal** | Muitas falhas prematuras → poda elimina grandes galhos → resolve problemas intratáveis por força bruta |
| **Pior caso** | Falhas só no final → árvore quase inteira é percorrida → comportamento ≈ força bruta, O(2ⁿ) ou O(n!) |

No fim das contas, o backtracking é **sempre correto** (encontra a solução se ela existir, e detecta quando não existe), mas sua **eficiência na prática depende inteiramente da qualidade da poda** — quanto antes e com mais segurança o algoritmo conseguir dizer "esse caminho não vale a pena", mais rápido ele se torna.
