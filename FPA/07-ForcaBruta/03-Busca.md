# 🔍 Busca de Padrões em Strings

## O Problema

Dado um texto s de tamanho n e um padrão p de tamanho m, queremos saber: **o padrão p aparece dentro de s? Se sim, em qual posição?**

É exatamente o que acontece quando você aperta Ctrl+F num documento e digita uma palavra para encontrar. O computador precisa de um algoritmo para fazer isso.

---

## A Abordagem por Força Bruta

A solução mais direta é a chamada **busca ingênua** (naive search): tente encaixar o padrão em cada posição possível do texto, uma por uma.

### O raciocínio passo a passo

**Passo 1 — Listar as posições candidatas:** o padrão p de tamanho m pode começar nas posições 0, 1, 2, ..., até n-m. Não adianta tentar além disso, pois o padrão não caberia no espaço restante.

**Passo 2 — Avaliar cada posição:** para cada posição i do texto, compare letra por letra com o padrão. Se todas as m letras baterem, encontrou.

**Passo 3 — Retornar o resultado:** se chegou ao fim do padrão com todas as letras conferindo, retorna a posição i onde o padrão começa. Se percorreu todo o texto sem sucesso, retorna -1.

### O algoritmo em pseudocódigo

` ` `
para (i = 0; i < (n - m + 1); i++) {
    j = 0;
    enquanto ((j < m) e (p[j] == s[i+j])) {
        j++;
    }
    se (j == m) {
        retorne i;       // padrão encontrado na posição i
    }
}
retorne -1;              // padrão não encontrado
` ` `

### Entendendo o código linha a linha

O laço externo (`i`) percorre cada posição do texto onde o padrão poderia começar. O limite é `n - m + 1` porque tentar depois disso seria impossível — o padrão ultrapassaria o fim do texto.

O laço interno (`j`) tenta casar letra por letra: compara `p[j]` com `s[i+j]`. Enquanto as letras forem iguais, j avança. Se j chegar a m, significa que **todas as m letras do padrão casaram** — padrão encontrado na posição i.

Se o laço interno parar antes (uma letra diferente), o algoritmo descarta essa posição e o laço externo avança para tentar a próxima.

### Exemplo visual

Texto s = `"ABCABCABD"` (n=9), Padrão p = `"ABD"` (m=3)

` ` `
Tentativa i=0: A B C  vs  A B D  → C ≠ D, falhou
Tentativa i=1: B C A  vs  A B D  → B ≠ A, falhou
Tentativa i=2: C A B  vs  A B D  → C ≠ A, falhou
Tentativa i=3: A B C  vs  A B D  → C ≠ D, falhou
Tentativa i=4: B C A  vs  A B D  → B ≠ A, falhou
Tentativa i=5: C A B  vs  A B D  → C ≠ A, falhou
Tentativa i=6: A B D  vs  A B D  → ✅ todas casaram!
Retorna 6
` ` `

---

## Complexidade

No **pior caso**, para cada uma das (n - m + 1) posições do texto, o algoritmo compara até m caracteres. Isso dá **O(n · m)**.

Na prática, quando o texto e o padrão têm caracteres variados (como texto em português), o laço interno quase sempre falha na primeira ou segunda letra, tornando o algoritmo bem mais rápido do que o pior caso sugere.

**Exemplo de pior caso:** buscar `"AAAB"` em `"AAAAAAAAAA"`. A cada posição, o algoritmo compara quase o padrão inteiro antes de falhar — esse cenário é raro no mundo real mas matematicamente é O(n·m).

---

## Conexão com as Transformações

Este algoritmo é força bruta pura: sem inteligência, sem aprendizado sobre o que já foi comparado. Se uma tentativa falha na terceira letra, a próxima posição começa do zero, ignorando completamente essa informação.

Algoritmos mais sofisticados como **KMP (Knuth-Morris-Pratt)** e **Boyer-Moore** usam exatamente a ideia de **transformação** vista anteriormente: processam o padrão antes da busca (mudança de representação) para nunca repetir comparações desnecessárias, chegando a O(n + m).

A busca ingênua é o ponto de partida — simples de entender, correta, mas com espaço claro de melhoria.

---

## Resumo

| Aspecto | Busca Ingênua em Strings |
| --------- | -------------------------- |
| **Ideia** | Tenta casar o padrão em cada posição do texto |
| **Posições testadas** | n - m + 1 |
| **Comparações por posição** | até m |
| **Complexidade** | O(n · m) |
| **Vantagem** | Simples, sem pré-processamento |
| **Desvantagem** | Repete trabalho já feito a cada posição |
