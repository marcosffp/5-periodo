# Resumo — Distribuições de Frequências

---

## 1. Organização dos Dados

**Dados Brutos**
> Lista de valores coletados sem nenhuma organização. Estão bagunçados e com repetições.

**Rol**
> Dados brutos ordenados em ordem crescente ou decrescente. Ainda apresentam repetições.

**Diagrama Ramo-e-Folhas**
> Separa cada valor em duas partes para visualizar a distribuição rapidamente.

```
Valor 47  →  Ramo = 4 (dezena)  |  Folha = 7 (unidade)
```

---

## 2. Tipos de Distribuição de Frequências

### Valores Distintos

Usada quando há muitos dados, mas **poucos valores diferentes**.

> Exemplo: número de descargas elétricas por dia (0, 1, 2, 3...)

### Valores Agrupados em Classes

Usada quando há **muita variação** nos valores ou quando a variável é contínua.
Os dados são reunidos em intervalos chamados **Classes**.

---

## 3. Como Construir uma Distribuição em Classes

**Passo 1** — Conte o total de valores observados → **n**

**Passo 2** — Calcule a Amplitude Total:

```
At = maior valor − menor valor
```

**Passo 3** — Calcule o número de classes **k** (use uma das duas):

```
Critério da Raiz:     k = √n

Fórmula de Sturges:   k = 1 + 3,32 × log(n)
```

> Arredonde k para o inteiro mais próximo.

**Passo 4** — Calcule o Intervalo de Classe:

```
c = At ÷ k
```

**Passo 5** — Verifique se a distribuição cobre todos os valores:

```
k × c + valor inicial > maior valor
```

> ⚠️ Todos os intervalos devem ter o **mesmo tamanho**. Nenhum valor pode ficar de fora.

---

## 4. Ponto Médio (xᵢ)

Como os valores exatos dentro de uma classe são desconhecidos, usa-se o **ponto médio** para representar a classe nos cálculos.

```
xᵢ = (limite inferior + limite superior) ÷ 2
```

---

## 5. Os 5 Tipos de Frequência

| Frequência | Símbolo | Fórmula | O que informa |
| --- | --- | --- | --- |
| Absoluta Simples | fᵢ | contagem direta | Quantas vezes o valor aparece |
| Relativa Simples | frᵢ | frᵢ = fᵢ ÷ n | Proporção de cada classe (soma = 1) |
| Absoluta Acumulada **Abaixo de** | Fᵢ↓ | F₁=f₁, F₂=f₁+f₂, ... | Quantos valores são ≤ ao limite da classe |
| Relativa Acumulada **Abaixo de** | Frᵢ↓ | Fr₁=fr₁, Fr₂=fr₁+fr₂, ... | Proporção acumulada até a classe atual |
| Absoluta Acumulada **Acima de** | Fᵢ↑ | Fᵢ = fᵢ + fᵢ₊₁ + ... + fₖ | Quantos valores são ≥ ao limite da classe |
| Relativa Acumulada **Acima de** | Frᵢ↑ | Frᵢ = frᵢ + frᵢ₊₁ + ... + frₖ | Proporção dos valores maiores ou iguais |

> ⚠️ A soma de todas as **frᵢ deve ser igual a 1** (100%). Se não bater por arredondamento, ajuste as casas decimais dos maiores dígitos sobrando.

---

## 6. O que Mais Cai na Prova

- [ ] Calcular **k** pelas duas fórmulas e arredondar para inteiro
- [ ] Calcular **c** e verificar com a inequação se cobre todos os dados
- [ ] Diferenciar os **5 tipos de frequência** e saber as fórmulas de cada um
- [ ] Lembrar que **Σfrᵢ = 1** (com correção de arredondamento se necessário)
- [ ] Calcular o **Ponto Médio** de uma classe
- [ ] Saber a diferença entre Dados Brutos, Rol e Diagrama Ramo-e-Folhas
