# 📐 Projeto de Algoritmos — Uma Introdução

Depois de entendermos **o que torna um problema difícil** (Teoria da Complexidade), agora a pergunta muda de foco: **dado que um problema é difícil, como eu escrevo um algoritmo melhor para ele?** É disso que trata o **Projeto de Algoritmos**.

---

## 🔄 Recapitulando (slide 2)

Antes de seguir, o slide retoma três ideias da aula anterior:

- **Custo e complexidade de algoritmos** → funções de complexidade (o quanto o tempo cresce com n)
- **Classe de complexidade e comportamento assintótico** → algoritmos exponenciais (aqueles que "explodem")
- **Classe de problemas** → problemas **tratáveis** (tem solução polinomial) x **intratáveis** (não tem)

Essas três ideias são a base para tudo que vem a seguir.

---

## 🧳 Um problema clássico: o roteiro de férias (slide 3)

Uma empresa de turismo quer montar o roteiro de férias de um cliente:

- As cidades a visitar já estão **pré-definidas**
- O ponto de **partida e chegada é o mesmo**
- A **ordem das visitas não importa**...
- ...desde que o **custo** seja minimizado (pode ser preço, distância, tempo de deslocamento, etc.)

> 💡 Reconhece esse problema? É exatamente o **Ciclo Hamiltoniano de menor custo**, ou seja, o famoso **Problema do Caixeiro Viajante**, que vimos na Teoria da Complexidade como um problema **NP-Difícil**!

---

## 🧮 Uma solução óbvia: força bruta (slides 4 a 6)

A primeira ideia que vem à cabeça é simples:

1. Calculamos **todas as combinações** de rotas possíveis
2. Comparamos o **custo de cada rota**
3. Pegamos a **melhor combinação**

Parece resolvido... mas será que **saberemos a melhor combinação** em tempo viável?

### A explosão combinatória (slide 6)

O slide mostra uma tabela considerando um computador que testa **100 milhões de combinações por segundo**. Como o número de rotas possíveis é dado por **(n-1)!** (fatorial), o tempo cresce de forma absurda:

| Cidades | Combinações (n-1)! | Tempo |
| --- | --- | --- |
| 5 | 24 | insignificante |
| 10 | 362.880 | 0,003 segundos |
| 15 | 87 bilhões | 14,5 minutos |
| 20 | 1,2 × 10¹⁷ | 38 anos |
| 25 | 6,2 × 10²³ | ??? (impraticável) |

**O que isso mostra na prática:** essa é a tal explosão **exponencial/fatorial** da aula passada saindo do papel. A "solução óbvia" funciona para 5 ou 10 cidades, mas se torna **inviável** rapidinho. Por isso precisamos de algo mais inteligente do que simplesmente "testar tudo".

---

## 🛠️ O que é "Projeto de Algoritmos"? (slides 7 a 9)

### O problema não é só "qual classe", mas "como resolver" (slide 7)

Como o problema do roteiro é **intratável** (nenhum algoritmo conhecido resolve em tempo viável), além de saber a classificação do problema, importa muito **a maneira como o algoritmo aborda o problema** — uma abordagem ruim pode levar a um desempenho péssimo mesmo em problemas mais simples.

**Exemplos do dia a dia (slide 8):**

- Verificar se um array tem elementos repetidos → dá pra fazer de um jeito ingênuo O(n²) ou de um jeito mais inteligente O(n log n)
- Algoritmos de ordenação → existem dezenas, e a escolha certa muda drasticamente o desempenho

### Definição (slide 9)

> **Projeto de Algoritmos** é o conjunto de métodos de codificação de algoritmos de forma a **salientar sua complexidade**, levando em conta a forma como o algoritmo chega à solução desejada.

E o slide fecha com uma frase de Dijkstra:

> *"A arte de programar consiste na arte de organizar e dominar a complexidade."* — E. Dijkstra

**Em outras palavras:** programar bem não é só "fazer funcionar", é **escolher a estratégia certa** para que o algoritmo seja eficiente.

---

## 🏅 Algoritmos ótimos (slide 10)

O objetivo do projeto de algoritmos é, sempre que possível, **determinar o menor custo possível** para resolver problemas de uma certa classe.

> **Algoritmo ótimo:** é aquele cujo custo é **igual ao menor custo possível** para a classe de problemas.

**Analogia:** se o menor número de comparações possível para ordenar n elementos é n·log(n), um algoritmo de ordenação que atinge exatamente esse custo é considerado **ótimo** — não dá pra fazer melhor que isso.

---

## 🔍 Por que existem problemas tão difíceis assim? (slides 11 e 12)

Alguns problemas intratáveis têm três características em comum:

- **Aparentemente são simples** (o enunciado é curto e fácil de entender, como o roteiro de férias)
- **Não se sabe se existe** um algoritmo determinista que os resolva em tempo polinomial
- **Aplicam-se a áreas muito importantes** (logística, redes, biologia, criptografia...)

Ou seja: são problemas **simples de descrever, difíceis de resolver e importantes na prática** — uma combinação que justifica todo o esforço da área.

---

## ⚖️ Solução de compromisso (slide 13)

Já que não dá pra resolver o problema "perfeitamente" em tempo viável, a estratégia é:

> Contentar-se em encontrar uma solução que **aproxime** a solução ideal, **em tempo útil**. Isso é chamado de **solução de compromisso**.

Podem existir **vários algoritmos diferentes** para o mesmo problema, cada um fazendo um "trade-off" diferente (mais rápido e menos preciso, mais lento e mais exato, etc.).

---

## ⏱️ Tempo x Espaço: uma relação antagônica (slides 14 a 16)

O projeto de algoritmos é fortemente influenciado pelo estudo de **como o algoritmo se comporta em tempo de execução e em espaço (memória) ocupado**.

> **Tempo e espaço tendem a se comportar de forma antagônica** — melhorar um geralmente piora o outro.

- **Para reduzir o tempo de execução:** o algoritmo "**lembra**" de coisas. Armazena resultados intermediários, cria índices ou tabelas de pesquisa → isso **aumenta o consumo de espaço** (memória).
- **Para reduzir o consumo de memória:** o algoritmo "**esquece**" coisas. Não guarda resultados prévios e recalcula valores sob demanda sempre que necessário → isso **aumenta o tempo de execução**.

**Analogia do dia a dia:** é como decorar a tabuada (rápido na hora de responder, mas você "gastou memória" memorizando) versus calcular cada multiplicação na mão sempre que precisar (não gasta memória nenhuma, mas é mais lento a cada vez).

---

## 🎓 Exemplo prático: cadastro de alunos (slides 17 a 22)

> Buscam o equilíbrio entre recursos, ou entre recursos, precisão e qualidade dos resultados.

**O problema:** uma universidade tem **n** alunos, cada um com um número de identificação de **k** dígitos. Dado o número de um aluno, queremos saber o seu nome (80 caracteres = 160 bytes).

Duas soluções extremas são propostas:

| | **Opção A** | **Opção B** |
| --- | --- | --- |
| Estrutura | Array com 10ᵏ posições | Lista com n posições |
| Tamanho de cada posição | 160 bytes | 160 bytes |
| **Tempo** | O(1) | O(n) |
| **Espaço** | 160 × 10ᵏ | n × 160 |

- **Opção A (array indexado pelo número de matrícula):** acesso instantâneo O(1) — basta usar o número como índice. Mas o espaço é gigantesco, porque o array precisa ter uma posição para **cada número possível de k dígitos** (10ᵏ), mesmo que a universidade tenha poucos alunos.
- **Opção B (lista simples):** gasta só o espaço necessário para os n alunos que realmente existem. Mas para achar um aluno é preciso **percorrer a lista inteira** → O(n).

### 🤔 Qual seria uma solução de compromisso? (slide 22)

O slide deixa essa pergunta em aberto para reflexão. A ideia é buscar algo **entre** os dois extremos:

- Uma **tabela hash**, por exemplo, gasta espaço proporcional a n (como a Opção B), mas com tempo de acesso próximo de O(1) (como a Opção A) — só que sem o desperdício de espaço.
- Uma **árvore de busca balanceada** (como uma AVL) também gasta espaço O(n), com tempo de busca O(log n) — pior que O(1), mas muito melhor que O(n), e sem precisar reservar 10ᵏ posições.

Essas são exatamente **soluções de compromisso**: nenhuma é "perfeita" nos dois quesitos, mas equilibram tempo e espaço de forma muito mais razoável que os extremos A e B.

---

## 🎯 Conclusão: Projetos e Soluções (slide 23)

O slide final resume a essência de tudo:

> De maneira geral, **analisa-se o recurso crítico** que o algoritmo solicita e sua **operação principal**. O projeto de algoritmos deve considerar **soluções de compromisso** no uso dos recursos.

---

## 🗺️ Resumão Final

| Conceito | Ideia central |
| --- | --- |
| **Roteiro de férias** | Versão "disfarçada" do Caixeiro Viajante — um problema NP-Difícil |
| **Solução óbvia (força bruta)** | Testar todas as (n-1)! combinações → inviável para n grande |
| **Projeto de Algoritmos** | Conjunto de métodos para codificar algoritmos destacando sua complexidade |
| **Algoritmo ótimo** | Tem custo igual ao menor custo possível para a classe |
| **Solução de compromisso** | Aproximar a solução ideal em tempo útil, equilibrando recursos |
| **Tempo x Espaço** | Relação antagônica: lembrar (mais memória, menos tempo) x esquecer (menos memória, mais tempo) |

A partir de agora, o curso vai apresentar **técnicas concretas** (como Força Bruta, Backtracking, Algoritmos Gulosos, Divisão e Conquista...) que são justamente diferentes **estratégias de projeto de algoritmos**, cada uma com seus próprios trade-offs de tempo, espaço e qualidade da solução.
