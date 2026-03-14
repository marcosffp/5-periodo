# Resumo — Gráficos de Distribuições de Frequências

---

## 1. Visão Geral

Existem 4 tipos de gráficos para representar Distribuições de Frequências:

| Gráfico | Quando usar | Usa qual frequência |
|---|---|---|
| Gráfico de Bastões | Distribuição de **Valores Individuais** | fᵢ |
| Histograma | Distribuição de **Valores Agrupados em Classes** | fᵢ e frᵢ |
| Polígono de Frequências | Junto com o Histograma | fᵢ e frᵢ |
| Ogiva de Galton | Frequências **Acumuladas** | Fᵢ e Frᵢ |

> ⚠️ Regra geral: **Eixo Y = 60% do Eixo X**

---

## 2. Gráfico de Bastões

Usado para **Distribuições de Valores Individuais** (poucos valores distintos).

- Cada valor tem uma **haste (bastão)** cuja altura representa a frequência
- O número de **espaços = número de hastes + 1**

**Cálculo do Eixo X:**
```
Eixo X = número de hastes × espaço + (número de hastes + 1) × espaço
       = (nº hastes + nº espaços) × tamanho do espaço
```

**Cálculo do Eixo Y:**
```
Eixo Y = 60% × Eixo X
```

---

## 3. Histograma

Usado para **Distribuições de Valores Agrupados em Classes**.

- Conjunto de **retângulos justapostos** (sem espaço entre eles)
- **Base** de cada retângulo = Intervalo de Classe
- **Altura** de cada retângulo = proporcional à fᵢ (frequência absoluta simples)
- Possui **dois eixos Y**: esquerdo (fᵢ) e direito (frᵢ)

**Cálculo do Eixo X:**
```
Eixo X = (número de classes × base) + (2 × espaço)
       onde espaço = base ÷ 2
```

**Cálculo do Eixo Y:**
```
Eixo Y = 60% × Eixo X
```

**Altura de cada coluna (regra de três):**
```
valor do topo da escala  →  comprimento do Eixo Y
valor da tabela (fᵢ)     →  altura da coluna = ?

altura = (fᵢ × Eixo Y) ÷ valor do topo da escala
```

**Escala do Eixo Y:**
```
1. Encontre divisores comuns entre o comprimento do Eixo Y (mm) e o maior fᵢ
2. Escolha um divisor → esse será o número de intervalos da escala
3. Comprimento de cada intervalo = Eixo Y ÷ nº intervalos
4. Unidades de cada intervalo   = maior fᵢ ÷ nº intervalos
```

---

## 4. Polígono de Frequências

Linha formada ligando os **pontos médios do topo** de cada barra do Histograma.

- Os extremos da linha devem tocar o **eixo X** (frequência zero)
- Desenhado **sobre** o Histograma

```
Ponto de cada classe = (ponto médio da classe, fᵢ da classe)
```

---

## 5. Ogivas de Galton

Gráficos das **frequências acumuladas**. São curvas (não barras).

### Ogiva "Abaixo de"
> Curva crescente — parte do zero e termina em n (total)

- Plota os pontos nos **limites superiores** de cada classe
- Usa Fᵢ↓ (eixo Y esquerdo) e Frᵢ↓ (eixo Y direito)

```
Ponto de cada classe = (limite superior da classe, Fᵢ↓)
```

### Ogiva "Acima de"
> Curva decrescente — parte de n e termina em zero

- Plota os pontos nos **limites inferiores** de cada classe
- Usa Fᵢ↑ (eixo Y esquerdo) e Frᵢ↑ (eixo Y direito)

```
Ponto de cada classe = (limite inferior da classe, Fᵢ↑)
```

---

## 6. Resumo Visual: Onde plotar os pontos

| Gráfico | Eixo X | Eixo Y |
|---|---|---|
| Bastões | Valor individual | fᵢ |
| Histograma | Intervalo de classe (base) | fᵢ (altura da barra) |
| Polígono | Ponto médio da classe | fᵢ |
| Ogiva Abaixo de | **Limite superior** da classe | Fᵢ↓ |
| Ogiva Acima de | **Limite inferior** da classe | Fᵢ↑ |

---

## 7. O que Mais Cai na Prova

- [ ] Saber **qual gráfico usar** para cada tipo de distribuição
- [ ] Calcular **Eixo X e Eixo Y** (lembrar que Y = 60% de X)
- [ ] Calcular a **altura das colunas** do Histograma pela regra de três
- [ ] Diferenciar **Ogiva Abaixo de** (crescente, limite superior) da **Ogiva Acima de** (decrescente, limite inferior)
- [ ] Saber que no **Polígono** os pontos ficam no **ponto médio** de cada classe
- [ ] Lembrar que o Histograma tem **dois eixos Y**: fᵢ à esquerda e frᵢ à direita