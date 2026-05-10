# 📋 Backtracking (Retrocesso)

## 🔍 O que é Backtracking? (slides 1, 2 e 3)

Backtracking é uma técnica de projeto de algoritmos também chamada de **retrocesso** ou **tentativa e erro**. Mas atenção: não é qualquer tentativa e erro — é uma versão muito mais inteligente da busca exaustiva.

A ideia funciona em dois momentos:

**Tentativa e Erro Estruturada:** O algoritmo constrói uma solução aos poucos, adicionando uma peça de cada vez — como preencher uma célula do Sudoku, ou colocar um item numa mochila.

**O Retrocesso em si:** O grande diferencial é a capacidade de **desfazer**. Se em algum momento o algoritmo percebe que a escolha que acabou de fazer torna a solução impossível de ser completada, ele **desfaz esse passo** e tenta outra opção. Ele tem "memória" de onde parou e continua a partir daí.

---

## ✂️ A diferença para a Força Bruta (slide 4)

A força bruta gera **todas as combinações possíveis até o final** e só depois verifica se cada uma serve. Isso é um desperdício enorme.

O backtracking vai além: ele verifica as **regras do problema durante a construção** da resposta. Se já deu errado no meio do caminho, ele nem termina de construir aquela solução — corta ela ali mesmo.

Isso é chamado de **poda** (ou *pruning*).

**Exemplo da Mochila (slide 5):**

Imagine uma mochila que suporta 20kg.

1. O algoritmo tenta colocar um item de 25kg.
2. Percebe imediatamente que já estourou o limite.
3. Em vez de continuar gerando todas as combinações que incluem esse item (que seriam inválidas de qualquer forma), ele **poda esse galho inteiro** da árvore de possibilidades.
4. Resultado: milhares de combinações ruins são descartadas sem nunca serem testadas.

---

## 🌳 A Árvore de Sub-tarefas (slides 6 e 7)

O backtracking constrói e percorre uma **árvore de sub-tarefas**. Cada nó da árvore representa um estado parcial da solução. Os galhos representam as escolhas possíveis a partir daquele estado.

No diagrama dos slides, os nós verdes são estados válidos que vale a pena explorar, os nós vermelhos são becos sem saída (podados), e o nó amarelo é a solução encontrada.

O problema é que essa árvore cresce rapidamente — em muitos casos com **comportamento exponencial**. Por isso a poda é tão crucial: sem ela, explorar a árvore inteira seria inviável.

---

## ⚙️ Como funciona na prática (slide 8)

O backtracking se aplica a problemas que possuem **soluções parciais candidatas**, ou seja, problemas onde dá para construir a resposta gradativamente e avaliar a cada passo se ainda faz sentido continuar.

Para funcionar, o algoritmo precisa de três coisas:

- Construir soluções **gradativamente**, passo a passo.
- Ter um **teste rápido** para saber se uma solução parcial ainda pode levar a uma solução válida.
- Usar uma **estrutura de controle** (normalmente uma árvore ou pilha de recursão) para saber de onde voltar quando precisar retroceder.

---

## 🎯 Aplicações (slide 9)

O backtracking aparece em muitos contextos do dia a dia e da computação:

- **Satisfação de restrições:** Sudoku, palavras cruzadas — problemas onde cada escolha precisa respeitar um conjunto de regras.
- **Parsing:** Interpretação de expressões e linguagens, onde o compilador tenta encaixar a entrada em regras gramaticais e volta atrás quando não funciona.
- **Linguagens de programação lógicas:** Prolog, por exemplo, funciona internamente com backtracking.
- **Problemas combinatórios:** Qualquer problema onde você precisa encontrar uma combinação ou arranjo que satisfaça certas condições.

---

## 📝 O Algoritmo Geral (slides 10 a 16)

O coração do backtracking é uma função recursiva chamada `Tenta(estado_atual)`. Ela tem cinco partes bem definidas:

` ` `
Função: Tenta(estado_atual) {

  // 1. Condição de Parada
  se solucaoDefinitiva(estado_atual) {
      retornaSolucao(estado_atual);
      retorna VERDADEIRO;
  }

  // 2. Geração dos candidatos
  candidatos = gerarCandidatos(estado_atual);

  para cada candidato em candidatos {

    // 3. Poda
    se solucaoAceitavel(candidato, estado_atual) {

      registraCandidato(candidato);  // Avança

      // 4. Recursão
      se Tenta(estado_atual_atualizado) == VERDADEIRO {
          retorna VERDADEIRO;
      }

      // 5. Backtracking
      apagaRegistroAnterior(candidato);  // Retrocede
    }
  }

  retorna FALSO;
}
` ` `

Vamos entender cada parte:

### 1. Condição de Parada — `solucaoDefinitiva` (slide 12)

É a **primeira pergunta** ao entrar na função: "Já chegamos na solução?"

Se sim, comemoramos, salvamos a resposta e encerramos. É o caso base da recursão — o equivalente a sair pela porta do labirinto.

### 2. Laço de Opções — `para cada candidato` (slide 13)

Se ainda não chegamos na solução, precisamos olhar para frente: "Quais são minhas opções agora?"

Num labirinto: ir para frente, direita ou esquerda. No Sudoku: testar os dígitos de 1 a 9. A função `gerarCandidatos` lista todas as possibilidades disponíveis a partir do estado atual.

### 3. A Poda — `solucaoAceitavel` (slide 14)

Aqui entra a inteligência do algoritmo. Antes de dar o passo, ele verifica: "Essa opção faz sentido?"

Se há uma parede na direita, a opção "direita" não é aceitável — o algoritmo nem tenta ir por ali. Essa verificação antecipada é o que elimina galhos inteiros da árvore de busca.

### 4. Ação e Recursão — `registraCandidato` + `Tenta()` (slide 15)

O candidato passou na verificação — o passo é válido! Então o algoritmo dá esse passo (`registraCandidato`) e chama `Tenta()` novamente a partir do novo estado.

É como dizer: "Beleza, dei um passo. E agora, a partir deste novo lugar, o que eu faço?" A solução vai sendo **construída de forma incremental**.

### 5. O Coração do Backtracking — `apagaRegistroAnterior` (slide 16)

Se a chamada recursiva retornou `FALSO`, significa que aquele caminho deu num beco sem saída lá na frente.

O que fazemos? **Desfazemos o passo** — apagamos o registro daquele candidato, voltamos ao estado anterior, e o laço naturalmente passa a testar o próximo candidato disponível. É exatamente aqui que o "retrocesso" acontece.

Se todos os candidatos foram testados e nenhum funcionou, retornamos `FALSO` — indicando ao nível acima que este caminho também não leva a lugar nenhum.

---

## 🎯 Resumão

| Conceito | O que é |
| --- | --- |
| **Backtracking** | Busca com capacidade de desfazer escolhas ruins |
| **Poda** | Descartar caminhos inválidos antes de explorá-los |
| **`solucaoAceitavel`** | Verifica se vale a pena continuar por aqui |
| **`registraCandidato`** | Faz o movimento (avança) |
| **`apagaRegistroAnterior`** | Desfaz o movimento (retrocede) |
| **`solucaoDefinitiva`** | Verifica se chegamos na solução completa |

A grande sacada do backtracking é que ele é **sistematicamente completo** (sempre encontra a solução se ela existir) porém muito mais eficiente que a força bruta, porque evita explorar partes do espaço de busca que já sabemos que são inválidas.
