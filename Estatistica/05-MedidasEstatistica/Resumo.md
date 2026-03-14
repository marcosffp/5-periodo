# Resumo — Medidas Estatísticas

---

## 1. Classificação Geral

| Tipo | O que mede | Exemplos |
|---|---|---|
| Medidas de Posição | Ordem de grandeza dos valores | Médias, Moda, Mediana |
| Medidas de Dispersão | Como os valores se espalham em torno da média | Desvio Padrão, Variância, At |
| Medidas de Assimetria e Curtose | Formato da curva da distribuição | Coeficientes de Pearson, K |

---

## 2. Somatório

Símbolo que representa a soma de vários valores:

```
Σxᵢ (i=1 até n)  =  x₁ + x₂ + x₃ + ... + xₙ
```

**Número de parcelas:**
```
P = n − a + 1
```

---

## 3. Médias

> Use **Simples** quando os dados estão em lista (sem frequência).
> Use **Ponderada** quando os dados estão em Distribuição de Frequências (com fᵢ).

### Média Aritmética (x̄) — a mais importante

```
Simples:    x̄ = Σxᵢ ÷ n

Ponderada:  x̄ = Σ(xᵢ × fᵢ) ÷ n
```
> Para distribuição em classes: use o **ponto médio** xᵢ = (lᵢ + lₛ) ÷ 2

### Média Quadrática (x̄q)

```
Simples:    x̄q = √(Σxᵢ² ÷ n)

Ponderada:  x̄q = √(Σxᵢ²×fᵢ ÷ n)
```

### Média Harmônica (x̄h)

```
Simples:    x̄h = n ÷ Σ(1/xᵢ)

Ponderada:  x̄h = n ÷ Σ(fᵢ/xᵢ)
```

### Média Geométrica (x̄g)

```
Simples:    x̄g = ⁿ√(x₁ × x₂ × ... × xₙ)

Ponderada:  x̄g = ⁿ√(x₁^f₁ × x₂^f₂ × ... × xₖ^fₖ)
```

**Propriedade das médias (ordem sempre crescente):**
```
x̄h  ≤  x̄g  ≤  x̄  ≤  x̄q
```

---

## 4. Moda (Mo)

> Valor que aparece com **maior frequência**.

### Para dados não tabulados
- Unimodal: um único valor mais frequente
- Bimodal / Trimodal: dois ou três valores empatados
- Amodal: nenhum valor se repete

### Para distribuição em classes — 3 métodos

**Moda Bruta** — ponto médio da classe modal (mais simples):
```
Mo = (limite inferior + limite superior) ÷ 2   (da classe de maior fᵢ)
```

**Moda por King:**
```
Mo = lᵢ + c × ( f_post ÷ (f_ant + f_post) )
```

**Moda por Czuber** (mais precisa):
```
Mo = lᵢ + c × ( (f_mo − f_ant) ÷ (2×f_mo − f_ant − f_post) )
```

> Onde: lᵢ = limite inferior da classe modal | c = intervalo de classe
> f_mo = frequência da classe modal | f_ant = frequência da classe anterior | f_post = frequência da classe posterior

---

## 5. Separatrizes

Dividem os dados ordenados em intervalos com porcentagens iguais.

| Separatriz | Símbolo | Intervalos | % por intervalo | Posição do elemento |
|---|---|---|---|---|
| Mediana | Md | 2 | 50% | n/2 (par) ou (n+1)/2 (ímpar) |
| Quartil | Qᵢ | 4 | 25% | E = i×n ÷ 4 |
| Decil | Dᵢ | 10 | 10% | E = i×n ÷ 10 |
| Centil | Cᵢ | 100 | 1% | E = i×n ÷ 100 |

> Se o Elemento for **inteiro** → a separatriz é esse valor da lista.
> Se **não for inteiro** → interpola entre os dois valores vizinhos:
```
Separatriz = valor_posição + parte_decimal × (próximo_valor − valor_posição)
```

### Mediana para distribuição em classes:
```
EMd = n ÷ 2    (sempre)

Md = lᵢ + c × (EMd − F_ant) ÷ f_Md
```

### Quartis, Decis e Centis para distribuição em classes (mesma estrutura):
```
Qᵢ = lᵢ + c × (E_Qi − F_ant) ÷ f_Qi
Dᵢ = lᵢ + c × (E_Di − F_ant) ÷ f_Di
Cᵢ = lᵢ + c × (E_Ci − F_ant) ÷ f_Ci
```

---

## 6. Boxplot (Diagrama de Caixa)

Gráfico construído com 5 valores: **Mín, Q1, Md, Q3, Máx**

- Retângulo vai de Q1 até Q3
- Linha dentro do retângulo = Mediana
- Traços externos: do Mín até Q1, e do Q3 até Máx

**Amplitude Interquartílica:**
```
AIQ = Q3 − Q1
```

**Outliers (valores atípicos):** fora do intervalo:
```
[Q1 − 1,5×AIQ  ;  Q3 + 1,5×AIQ]
```

---

## 7. Amplitude Total (At)

```
At = maior valor − menor valor
```

Intervalo onde os valores se encontram (com alta certeza):
```
[x̄ − At  ;  x̄ + At]
```

**Estimativa rápida do Desvio Padrão:**
```
s ≈ At ÷ 4
```

---

## 8. Desvio Padrão (s ou σ) e Variância (s² ou σ²)

> Desvio Padrão = mede o quanto os valores se afastam da média.
> Variância = Desvio Padrão ao quadrado.

**Fórmula direta (amostras):**
```
Para lista:         s = √[ Σ(xᵢ − x̄)² ÷ (n−1) ]

Para tabulados:     s = √[ Σ(xᵢ − x̄)² × fᵢ ÷ (n−1) ]
```

**Processo abreviado (mais fácil na calculadora):**
```
Para lista:         s = √[ 1/(n−1) × (Σxᵢ² − (Σxᵢ)²/n) ]

Para tabulados:     s = √[ 1/(n−1) × (Σxᵢ²fᵢ − (Σxᵢfᵢ)²/n) ]
```

```
Variância:  s² = (valor do desvio padrão)²
```

---

## 9. Medidas de Dispersão Relativa

Compara a dispersão de séries com médias diferentes.

**Coeficiente de Variação de Pearson:**
```
CV = σ ÷ x̄
```

**Variância Relativa:**
```
V = σ² ÷ x̄²
```

> Maior CV = maior dispersão relativa (mesmo que o desvio padrão seja menor).

---

## 10. Assimetria

| Situação | Tipo |
|---|---|
| x̄ = Md = Mo | Simétrica |
| x̄ > Md > Mo | Assimetria Positiva (cauda à direita) |
| x̄ < Md < Mo | Assimetria Negativa (cauda à esquerda) |

**1º Coeficiente de Pearson:**
```
e₁ = (x̄ − Mo) ÷ σ
```

**2º Coeficiente de Pearson:**
```
e₂ = 3×(x̄ − Md) ÷ σ
```

> e = 0 → simétrica | e > 0 → positiva | e < 0 → negativa

---

## 11. Curtose

Mede o **achatamento** da curva em relação à curva normal (Gauss).

**Coeficiente Percentílico de Curtose:**
```
K = (Q3 − Q1) ÷ [ 2 × (C90 − C10) ]
```

| Valor de K | Tipo de Curva | Formato |
|---|---|---|
| K = 0,263 | Mesocúrtica | Normal (padrão) |
| K < 0,263 | Leptocúrtica | Mais afilada |
| K > 0,263 | Platicúrtica | Mais achatada |

---

## 12. O que Mais Cai na Prova

- [ ] Calcular as **4 médias** (aritmética, quadrática, harmônica, geométrica) e lembrar a ordem: x̄h ≤ x̄g ≤ x̄ ≤ x̄q
- [ ] Diferenciar os **3 métodos de Moda** em classes (Bruta, King, Czuber) e saber quando usar cada um
- [ ] Calcular **Mediana, Quartis, Decis e Centis** — saber a fórmula do elemento e a fórmula da separatriz
- [ ] Calcular **Desvio Padrão** pelo processo abreviado (mais rápido)
- [ ] Saber o que é **Boxplot** e seus 5 componentes
- [ ] Diferenciar assimetria **positiva** (x̄ > Md > Mo) da **negativa** (x̄ < Md < Mo)
- [ ] Interpretar o **Coeficiente de Curtose** (K menor = mais afilada = Leptocúrtica)
- [ ] Saber que **CV** é melhor que o desvio padrão para comparar dispersão entre séries diferentes