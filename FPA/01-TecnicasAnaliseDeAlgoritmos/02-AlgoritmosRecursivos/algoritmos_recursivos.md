# Análise de Algoritmos Recursivos — Guia Completo

---

## O que é um Algoritmo Recursivo?

É um algoritmo que **chama a si mesmo** com um valor menor, até chegar em um caso simples que sabe resolver sozinho (o caso base).

Exemplo — rec1 (fatorial):
```java
int rec1(int n){
    if(n == 0)
        return 1;           // caso base: para aqui
    else
        return n * rec1(n-1); // chama a si mesmo com n-1
}
```

Execução com n = 3:
```
rec1(3) = 3 × rec1(2)
rec1(2) = 2 × rec1(1)
rec1(1) = 1 × rec1(0)
rec1(0) = 1             ← caso base, para aqui
```
Resultado: 3 × 2 × 1 × 1 = **6**

---

## Os 6 Passos para Analisar Algoritmos Recursivos

| Passo | O que fazer |
|---|---|
| 0 | Não entre em pânico |
| 1 | Identifique base e recursividade |
| 2 | Defina o que é "n" e qual o custo da base |
| 3 | Identifique as operações relevantes |
| 4 | Marque os custos no código |
| 5 | Monte a equação de recorrência |
| 6 | Resolva por expansão |

---

## Passo a Passo: analisando o rec1

### Passo 1 — Identificar base e recursividade

```java
if(n == 0)
    return 1;            // BASE: quando n = 0, para aqui
else
    return n * rec1(n-1); // RECURSIVIDADE: chama com n-1
```

### Passo 2 — Custo do caso base

Quando n = 0, só executa o `return 1` → **1 operação**

> **S(0) = 1**

### Passo 3 — Custo do caso recursivo

Na linha `return n * rec1(n-1)` acontecem **duas coisas**:

- `n *` → 1 multiplicação = **1 operação local**
- `rec1(n-1)` → chamada recursiva que custa **S(n-1)**

> **S(n) = 1 + S(n-1)**

### Passo 4 — A Equação de Recorrência (Equação Definida por Partes)

```
S(0) = 1            ← caso base
S(n) = S(n-1) + 1   ← caso recursivo
```

Isso é a forma matemática de dizer:
> *"Para calcular o custo de n, gasto 1 operação agora e pago o custo de (n-1) depois"*

---

## Resolvendo a Equação — Método da Expansão

### Por que precisa resolver?

A equação `S(n) = S(n-1) + 1` ainda tem **S dentro** — é circular. A gente quer S(n) em função **só de n**, sem S no lado direito.

### A ideia: substituir repetidamente

Começa com a definição e vai substituindo S pelo que ele vale:

**Passo 1:**
```
S(n) = S(n-1) + 1
```

**Passo 2** — substitui S(n-1) = S(n-2) + 1:
```
S(n) = [S(n-2) + 1] + 1
S(n) = S(n-2) + 2
```

**Passo 3** — substitui S(n-2) = S(n-3) + 1:
```
S(n) = [S(n-3) + 1] + 2
S(n) = S(n-3) + 3
```

### O padrão que aparece

```
S(n) = S(n-1) + 1   ← expandiu 1 vez,  somou 1
     = S(n-2) + 2   ← expandiu 2 vezes, somou 2
     = S(n-3) + 3   ← expandiu 3 vezes, somou 3
```

A cada expansão: o número **dentro** do S diminui 1, o número **fora** aumenta 1.

**Fórmula Geral (F.G) após i expansões:**
```
F.G = S(n-i) + i
```

---

## A Parte Mais Importante: onde parar?

### Por que parar no caso base?

A F.G ainda tem S dentro — `S(n-i)`. Para se livrar do S, a gente precisa que o argumento seja **0**, porque S(0) = 1 é o único valor que a gente **já sabe sem calcular mais nada**.

> Pensa assim: S(n-i) é uma caixa lacrada. A única chave que abre qualquer caixa S é quando o número dentro é **0** — porque aí você sabe que tem o valor **1** dentro.

### Encontrando o valor de i

Pergunta: para que valor de i o argumento `(n-i)` vira 0?

```
n - i = 0
i = n
```

**Depois de n expansões, chegamos em S(0).**

### Substituindo i = n na F.G

```
S(n) = S(n-i) + i     ← F.G
S(n) = S(n-n) + n     ← substitui i = n
S(n) = S(0) + n       ← n-n = 0
S(n) = 1 + n          ← S(0) = 1
```

> **Resultado: S(n) = n + 1 → O(n)**

---

## Confirmando com n = 4

```
S(4) = S(3) + 1   ← i=1, argumento = 4-1 = 3
     = S(2) + 2   ← i=2, argumento = 4-2 = 2
     = S(1) + 3   ← i=3, argumento = 4-3 = 1
     = S(0) + 4   ← i=4, argumento = 4-4 = 0 ✅ BASE!
     = 1 + 4
     = 5
```

Quando i = n = 4, o argumento zerou, chegamos em S(0), e substituímos. ✅

---

## Resumo Visual do Método Completo

```
CÓDIGO          →   EQUAÇÃO              →   EXPANDE        →   RESOLVE

if(n==0)            S(0) = 1             S(n-1) + 1        F.G = S(n-i) + i
  return 1;                              S(n-2) + 2        i = n  →  S(n) = 1+n
else                S(n) = S(n-1) + 1   S(n-3) + 3
  return n*rec1(n-1)                     ...
```

---

## Tabela Resumo do Método

| Etapa | O que faz | Como faz |
|---|---|---|
| **Monta a equação** | Lê o código e escreve o custo | S(0) = custo da base; S(n) = custo local + S(chamada) |
| **Expande** | Substitui S(n-1) pela sua definição | Repete até aparecer padrão |
| **F.G** | Escreve o padrão geral | S(n-i) + i após i expansões |
| **Para na base** | Descobre quando chega em S(0) | Resolve n-i = 0 → i = n |
| **Substitui** | Troca i = n na F.G | S(n) = S(0) + n = 1 + n |

---

## Regra Geral: como identificar o caso base na F.G

O caso base é sempre quando o **argumento de S vira o valor da base** do seu problema:

| Caso base do código | Argumento que zera | Equação |
|---|---|---|
| `if(n == 0)` | n - i = 0 → i = n | S(n) = S(0) + ... |
| `if(n == 1)` | n - i = 1 → i = n-1 | S(n) = S(1) + ... |
| `if(i == 0)` | i - k = 0 → k = i | S(i) = S(0) + ... |
