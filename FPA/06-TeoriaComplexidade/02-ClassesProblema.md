# 📦 As classes de problemas (slide 11)

Existem 4 "categorias" para classificar problemas:

- **P** → problemas fáceis
- **NP** → problemas difíceis de resolver, mas fáceis de verificar
- **NP-Completo** → os mais difíceis dentro do NP
- **NP-Difícil** → ainda mais difíceis (veremos depois)

Vamos entender cada uma.

---

## 🟢 Classe P (slide 12)

São os problemas que **dão pra resolver de forma eficiente** (algoritmo polinomial).

**Exemplos simples:**

- Ordenar uma lista de nomes → fácil, tem vários algoritmos rápidos
- Verificar se um grafo é completo → é só checar se todos os pontos estão conectados
- Pesquisar um eleitor no banco do TSE → uma busca simples

Pensa assim: **P = problemas que o computador resolve tranquilamente.**

---

## 🔍 Verificação de ordenação (slide 13)

O slide mostra um algoritmo que verifica se uma lista já está ordenada. Ele percorre a lista uma vez e checa se cada elemento é menor que o próximo. Isso é O(n), ou seja, **polinomial e rápido**. Esse tipo de verificação também pertence ao NP, o que nos leva ao próximo ponto.

---

## 🔴 Classe NP (slides 14 a 22)

Aqui começa a ficar interessante. NP significa **Nondeterministic Polynomial Time**.

A definição formal é complicada, mas a forma mais simples de entender é:

> **NP = problemas onde, se alguém te der uma solução pronta, você consegue verificar se ela está correta rapidamente.**

**Exemplo do dia a dia:**
Imagina um cubo mágico. Resolver do zero é difícil e demorado. Mas se alguém te entregar um cubo "resolvido", você consegue verificar em segundos se está correto ou não. Esse é o espírito do NP.

---

## 🤖 Algoritmo determinista x não-determinista (slides 14, 15, 16 e 17)

Essa distinção é importante para entender o NP.

**Algoritmo determinista (slide 15):**
É o algoritmo normal que conhecemos. Sempre faz os mesmos passos na mesma ordem, sempre chega ao mesmo resultado. Sem surpresas, sem sorte, sem palpites.

**Exemplo:** uma calculadora. 2+2 sempre vai dar 4, seguindo sempre os mesmos passos.

**Algoritmo não-determinista (slide 16):**
É um algoritmo teórico, imaginário, que tem um "superpoder": em cada passo onde existem várias escolhas possíveis, ele **sempre magicamente escolhe a certa**. É como se ele tivesse sorte infinita.

Ele usa três operações especiais:

- **escolhe(C)** → magicamente escolhe o elemento certo do conjunto C
- **SUCESSO** → sinalizou que chegou numa solução válida
- **INSUCESSO** → sinalizou que não chegou

O mais importante: a função **escolhe tem custo Θ(1)**, ou seja, é instantânea. Na prática isso não existe, mas usamos esse modelo teórico para estudar os limites do que é computável.

---

## 🔎 Exemplo prático: Pesquisa (slide 18)

Quer achar o elemento x numa lista A com n elementos.

**Com algoritmo determinista:** você percorre a lista do começo ao fim até achar. No pior caso, olha todos os n elementos → custo **Θ(n)**.

**Com algoritmo não-determinista:** a função *escolhe* magicamente já aponta o índice certo direto → custo **Θ(1)**. Instantâneo!

Isso ilustra o poder teórico do não-determinismo. Na prática ele não existe, mas nos ajuda a classificar problemas.

---

## ✅ A definição prática de NP (slides 19 a 22)

Na prática, a forma mais usada de pensar no NP é:

> **Um problema está em NP se, dada uma solução candidata, dá pra verificar se ela é válida em tempo polinomial com um algoritmo determinista.**

**Exemplos de problemas em NP:**

**Exemplo 1 (slide 20):** Existe um caminho entre A e B com custo menor que K?
→ Se alguém te der um caminho, você consegue somar os custos das arestas rapidamente e verificar. ✅

**Exemplo 2 (slide 21):** Existe um caminho que passa por todas as cidades sem repetir nenhuma? (Ciclo Hamiltoniano)
→ Se alguém te der uma sequência de cidades, você consegue verificar se ela passa por todas e não repete nenhuma rapidamente. ✅

**Exemplo 3 (slide 22):** Existe um subconjunto dos elementos de S que soma exatamente K?
→ Se alguém te der o subconjunto, você some os elementos e verifica. ✅

Todos esses são difíceis de **resolver**, mas fáceis de **verificar**. Essa é a essência do NP.

---

## 🧪 Como provar que um problema está em NP? (slide 23)

Existem duas formas equivalentes, e basta usar uma delas:

**Forma 1:** Apresentar um algoritmo **não-determinista** que resolve o problema em tempo polinomial.

**Forma 2 (mais usada na prática):** Mostrar que existe um algoritmo **determinista polinomial** que, dada uma solução candidata, consegue **verificar** se ela é válida.

**Exemplo prático da forma 2:**
Para o Ciclo Hamiltoniano, se alguém te der uma sequência de cidades, você verifica em tempo polinomial se:

- Ela passa por todas as cidades ✅
- Nenhuma cidade se repete ✅
- Todas as arestas do caminho existem no grafo ✅

Se consegue verificar assim, o problema está em NP.

---

## 🗺️ Resumindo tudo até aqui

| Classe | Ideia central |
| --- | --- |
| **P** | Dá pra **resolver** rápido (polinomial) |
| **NP** | Dá pra **verificar** uma solução rápido (polinomial) |
| **P ⊆ NP** | Todo problema fácil de resolver também é fácil de verificar |

A grande questão em aberto na ciência da computação é: **P = NP?** Ou seja, será que todo problema fácil de verificar também é fácil de resolver? Ninguém sabe ainda!
