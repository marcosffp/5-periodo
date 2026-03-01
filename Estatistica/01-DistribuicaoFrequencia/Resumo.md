# Distribuição de Frequências — Material Completo

---

## Para que serve?

Quando você tem muitos dados (como 50 idades), fica impossível analisar número por número. A tabela de frequências **organiza esses dados em grupos (classes)**, permitindo enxergar padrões como: qual faixa tem mais ocorrências, como os dados se distribuem, etc.

---

## Dados de Exemplo

> **Idades de 50 clientes de uma loja**
> n = 50 | Menor = 15 | Maior = 74

| | Col 1 | Col 2 | Col 3 | Col 4 | Col 5 |
|---|---|---|---|---|---|
| 1 | 23 | 45 | 31 | 52 | 18 |
| 2 | 67 | 29 | 41 | 55 | 38 |
| 3 | 19 | 72 | 44 | 26 | 61 |
| 4 | 33 | 48 | 57 | 22 | 70 |
| 5 | 15 | 39 | 64 | 43 | 28 |
| 6 | 58 | 35 | 47 | 21 | 66 |
| 7 | 30 | 53 | 17 | 74 | 42 |
| 8 | 60 | 27 | 49 | 36 | 56 |
| 9 | 25 | 71 | 34 | 63 | 46 |
| 10 | 20 | 40 | 59 | 32 | 68 |

---

## Construindo a Tabela — Passo a Passo

### Passo 1 — Amplitude Total (At)

**Para que serve:** mede o "tamanho" do conjunto de dados, ou seja, a distância entre o menor e o maior valor.

```
At = maior − menor
At = 74 − 15 = 59
```

---

### Passo 2 — Número de Classes (k)

**Para que serve:** define quantas "fatias" você vai dividir seus dados. Poucas classes = informação perdida. Muitas classes = tabela confusa.

**Fórmula recomendada — Regra de Sturges:**
```
k = 1 + 3,32 · log(n)
k = 1 + 3,32 · log(50)
k = 1 + 3,32 · 1,699
k = 1 + 5,64 = 6,64  →  k = 7
```

**Fórmula alternativa — Critério da Raiz:**
```
k = √n  →  k = √50 = 7,07  →  k = 7
```

> ⚠️ De preferência use a **Regra de Sturges**. Se as fórmulas derem resultados muito diferentes, ajuste k ou c para que a tabela faça sentido.

---

### Passo 3 — Intervalo de Classe (c)

**Para que serve:** define o "tamanho" de cada classe. Deve ser **constante** (igual) em toda a tabela.

```
c = At / k
c = 59 / 7 = 8,43  →  c ≈ 9  (arredonda para facilitar)
```

---

### ⚠️ Verificação Obrigatória: k × c + menor > maior

Antes de montar as classes, confirme que elas cobrem **todos os dados**:

```
k × c + menor > maior
7 × 9 + 15 = 63 + 15 = 78 > 74  ✅
```

Se o resultado for **menor ou igual** ao maior valor, aumente **c** ou **k** e refaça até passar.

---

### Passo 4 — Elementos de Cada Classe

Cada classe possui três elementos importantes:

| Elemento | Símbolo | Fórmula |
|---|---|---|
| Limite Inferior | li | valor à esquerda do intervalo |
| Limite Superior | ls | valor à direita do intervalo |
| Intervalo de Classe | c | c = ls − li |
| Ponto Médio da Classe | xi | xi = (li + ls) / 2 |

---

### Passo 5 — Montar as Classes

Começa sempre no **menor valor** e soma c repetidamente. Use **uma única simbologia** em toda a tabela:

| Simbologia | Significado |
|---|---|
| ⊢ | fechado à esquerda, aberto à direita *(mais usada)* |
| ├— | mesma coisa, notação alternativa |
| → | mesma coisa, outra variação |

**Classes do nosso exemplo (c = 9, começa em 15):**

| Classes | xi (ponto médio) |
|---|---|
| 15 ⊢ 24 | 19,5 |
| 24 ⊢ 33 | 28,5 |
| 33 ⊢ 42 | 37,5 |
| 42 ⊢ 51 | 46,5 |
| 51 ⊢ 60 | 55,5 |
| 60 ⊢ 69 | 64,5 |
| 69 ⊢ 78 | 73,5 |

> O símbolo **⊢** significa que o limite inferior **pertence** à classe e o limite superior **não pertence** (vai para a próxima classe). Ex: um valor de 24 vai para a classe 24⊢33, não para 15⊢24.

---

## Os 4 Tipos de Frequência

### Macete para lembrar a notação

| Letra | Significado |
|---|---|
| **f** minúsculo | simples |
| **F** maiúsculo | acumulada |
| **r** no meio | relativa |
| sem **r** | absoluta |

> Exemplo: **Fri** = F (acumulada) + r (relativa) + i (classe i)

---

### 1. Frequência Absoluta Simples (fi)

**Para que serve:** conta quantos valores caem dentro de cada classe. É uma contagem pura.

**Como calcular:** percorra todos os dados e marque um palito para cada valor que cair no intervalo da classe.

```
Classe 15⊢24 → contar quantas idades são ≥ 15 e < 24
```

> ⚠️ Sempre número **inteiro**. A soma de todos os fi deve ser igual a **n**.
> **∑ fi = n = 50**

---

### 2. Frequência Relativa Simples (fri)

**Para que serve:** mostra a proporção de cada classe em relação ao total. Responde: *"que percentual dos dados está nessa faixa?"*

```
fri = fi / n
```

Exemplo: fi = 12, n = 50 → fri = 12/50 = **0,240** (ou 24%)

> ⚠️ A soma deve ser **∑ fri = 1,000**. Por causa de arredondamentos pode dar 0,999 ou 1,001 — veja a seção de arredondamento abaixo.

---

### 3. Frequência Absoluta Acumulada — "Abaixo de" (Fi↓)

**Para que serve:** responde *"quantos dados estão abaixo desse limite?"*. Vai somando de **cima para baixo**.

**Como calcular:**
```
F1 = f1
F2 = F1 + f2
F3 = F2 + f3
... e assim por diante
```

| Classes | fi | Fi ↓ (abaixo de) |
|---|---|---|
| 15 ⊢ 24 | 8 | 8 |
| 24 ⊢ 33 | 10 | 18 |
| 33 ⊢ 42 | 12 | 30 |
| 42 ⊢ 51 | 9 | 39 |
| 51 ⊢ 60 | 6 | 45 |
| 60 ⊢ 69 | 3 | 48 |
| 69 ⊢ 78 | 2 | **50** |

> ⚠️ O último Fi↓ deve ser igual a **n = 50**.

---

### 4. Frequência Absoluta Acumulada — "Acima de" (Fi↑)

**Para que serve:** responde *"quantos dados estão acima desse limite?"*. Vai somando de **baixo para cima**.

**Como calcular:**
```
Última linha = fi da última classe
Linha anterior = linha de baixo + fi atual
... sobe até o topo
```

| Classes | fi | Fi ↑ (acima de) |
|---|---|---|
| 15 ⊢ 24 | 8 | **50** |
| 24 ⊢ 33 | 10 | 42 |
| 33 ⊢ 42 | 12 | 32 |
| 42 ⊢ 51 | 9 | 20 |
| 51 ⊢ 60 | 6 | 11 |
| 60 ⊢ 69 | 3 | 5 |
| 69 ⊢ 78 | 2 | 2 |

> ⚠️ O **primeiro valor** começa em **n = 50** e o **último** termina no fi da última classe.

---

### 5. Frequência Relativa Acumulada — "Abaixo de" (Fri↓)

**Para que serve:** igual à Fi↓, mas em proporção. Responde *"qual percentual dos dados está abaixo desse limite?"*

**Como calcular:**
```
Fri = Fi / n   (em qualquer dos dois sentidos)
```
Ou somando as fri de cima para baixo.

| Classes | fri | Fri ↓ |
|---|---|---|
| 15 ⊢ 24 | 0,160 | 0,160 |
| 24 ⊢ 33 | 0,200 | 0,360 |
| 33 ⊢ 42 | 0,240 | 0,600 |
| 42 ⊢ 51 | 0,180 | 0,780 |
| 51 ⊢ 60 | 0,120 | 0,900 |
| 60 ⊢ 69 | 0,060 | 0,960 |
| 69 ⊢ 78 | 0,040 | **1,000** |

> ⚠️ O último Fri↓ deve ser igual a **1,000**.

---

### 6. Frequência Relativa Acumulada — "Acima de" (Fri↑)

Vai somando as fri de **baixo para cima**.

| Classes | fri | Fri ↑ |
|---|---|---|
| 15 ⊢ 24 | 0,160 | **1,000** |
| 24 ⊢ 33 | 0,200 | 0,840 |
| 33 ⊢ 42 | 0,240 | 0,640 |
| 42 ⊢ 51 | 0,180 | 0,400 |
| 51 ⊢ 60 | 0,120 | 0,220 |
| 60 ⊢ 69 | 0,060 | 0,100 |
| 69 ⊢ 78 | 0,040 | 0,040 |

> ⚠️ O **primeiro valor** começa em **1,000** e o **último** termina no fri da última classe.

---

## Tabela Resumo — Todos os Conceitos

| Conceito | Símbolo | Fórmula | Direção | Começa em | Termina em |
|---|---|---|---|---|---|
| Freq. Absoluta Simples | fi | contagem direta | — | — | ∑ fi = n |
| Freq. Relativa Simples | fri | fi ÷ n | — | — | ∑ fri = 1,000 |
| Freq. Abs. Acumulada ↓ | Fi↓ | Fi anterior + fi | cima → baixo | f1 | n |
| Freq. Abs. Acumulada ↑ | Fi↑ | Fi abaixo + fi | baixo → cima | n | fi última classe |
| Freq. Rel. Acumulada ↓ | Fri↓ | Fri anterior + fri | cima → baixo | fri1 | 1,000 |
| Freq. Rel. Acumulada ↑ | Fri↑ | Fri abaixo + fri | baixo → cima | 1,000 | fri última classe |

---

## Regras de Arredondamento da fri

### O problema

Ao dividir fi ÷ n e arredondar para 3 casas decimais, a soma pode dar **0,999 ou 1,001** em vez de 1,000 exato. Isso não é erro — é consequência matemática do arredondamento.

### A técnica correta — passo a passo

**Passo 1:** calcule todas as fri com **2 casas a mais** do que o pedido.
- Pede 3 casas → calcule com **5 casas**
- Pede 5 casas → calcule com **7 casas**

**Passo 2:** some apenas as casas que você quer (as 3 primeiras). Veja o que falta para completar 1,000.
```
Soma deu 0,998  →  falta 0,002  →  precisa corrigir 2 valores
Soma deu 1,001  →  sobra 0,001  →  precisa corrigir 1 valor
```

**Passo 3:** a quantidade de correções é:
```
nº de correções = diferença × 1000   (para 3 casas decimais)
```

**Passo 4:** olhe as **casas extras (4ª e 5ª)** de cada fri. Ordene da maior para a menor.
- Se a soma ficou **abaixo de 1** → arredonde **para cima** os maiores resíduos
- Se a soma ficou **acima de 1** → arredonde **para baixo** os maiores resíduos

### Exemplo prático (n = 71, pede 3 casas)

| fi | fri (5 casas) | Casas extras | fri (3 casas) |
|---|---|---|---|
| 9 | 0,12**676** | **76** | 0,127 |
| 5 | 0,07042 | 42 | 0,070 |
| 7 | 0,09859 | 59 | 0,099 |
| 4 | 0,05634 | 34 | 0,056 |
| 19 | 0,26760 | 60 | 0,268 |
| 11 | 0,15492 | 92 | 0,155 |
| 8 | 0,11267 | 67 | 0,113 |
| 2 | 0,02816 | 16 | 0,028 |
| 6 | 0,08450 | 50 | 0,085 |
| **71** | **1,00000** | — | **1,001** ❌ |

A soma deu **1,001** → sobra 0,001 → corrigir **1 valor para baixo**.

Olhando as casas extras, o maior resíduo é **76** (do fri = 0,12676). Ele absorve a correção:

> 0,12676 → arredonda para **0,126** em vez de 0,127

Soma final: **1,000** ✅

### Resumo da regra

| Situação | O que fazer | Quais corrigir |
|---|---|---|
| Soma < 1,000 | Arredondar **para cima** | Os de **maior** resíduo nas casas extras |
| Soma > 1,000 | Arredondar **para baixo** | Os de **maior** resíduo nas casas extras |
| Qtd. de correções | diferença × 1000 | Em ordem decrescente do resíduo |

---

## Checklist Final — Antes de Entregar

- [ ] At = maior − menor ✓
- [ ] k calculado pela Regra de Sturges e arredondado ✓
- [ ] c = At/k arredondado para número inteiro ou simples ✓
- [ ] Verificação k × c + menor > maior passou ✓
- [ ] Todas as classes têm o mesmo intervalo c ✓
- [ ] Usou uma única simbologia em toda a tabela ✓
- [ ] Primeira classe começa no menor valor ✓
- [ ] Última classe contém o maior valor ✓
- [ ] ∑ fi = n ✓
- [ ] ∑ fri = 1,000 (com correção de arredondamento se necessário) ✓
- [ ] Último Fi↓ = n ✓
- [ ] Primeiro Fi↑ = n ✓
- [ ] Último Fri↓ = 1,000 ✓
- [ ] Primeiro Fri↑ = 1,000 ✓