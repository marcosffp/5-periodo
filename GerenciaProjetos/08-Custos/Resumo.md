# RESUMO — Gerência de Custos do Projeto

## 1. Contexto Geral

A gerência de custos é uma das áreas mais críticas na gestão de projetos de software. A primeira — e mais insistente — pergunta que o cliente faz ao gerente de projeto é:

> **"Quanto vai custar?"**

Essa pergunta, aparentemente simples, **não é fácil de responder**: para respondê-la com precisão é preciso conhecer bem o escopo do projeto e identificar cada item de custo que o compõe. A área de custos é justamente o conjunto de processos que tornam essa resposta possível — e controlada — ao longo de todo o ciclo de vida do projeto.

---

## 2. Conceito de Custo

### 2.1 Definição (PMBOK v6)

> Segundo o PMBOK v6: "O gerenciamento dos custos do projeto inclui os processos usados em **planejamento, estimativa, orçamento, financiamento, gerenciamento e controle dos custos**, para que o projeto possa ser realizado dentro do **orçamento aprovado**."

E, mais especificamente, o gerenciamento de custos do projeto **preocupa-se principalmente com o custo dos recursos necessários para completar as atividades do projeto** — ou seja, só entram no orçamento itens diretamente ligados ao projeto. Um cafezinho consumido pelo desenvolvedor no dia a dia, por exemplo, não é custo do projeto: ele existiria independentemente de o projeto existir ou não; é custo da empresa.

### 2.2 Relação com outras áreas

Custo não existe de forma isolada. Ele está **diretamente ligado** a três outras áreas de conhecimento:

- **Escopo**: se o escopo cresce (novas funcionalidades, por exemplo), o custo tende a crescer também.
- **Cronograma**: alterações no prazo podem impactar o custo — embora não de forma direta e automática; depende das dependências entre as atividades e de como os recursos estão alocados.
- **Recursos**: mais recursos (pessoas, servidores, softwares) significa mais custo.

Toda mudança em qualquer uma dessas três áreas deve ser avaliada quanto ao seu reflexo nos custos.

### 2.3 Considerações importantes sobre custo

1. **Diferentes stakeholders medem custo de formas diferentes.**
   - Do ponto de vista do **cliente**: o orçamento apresentado já inclui o lucro da empresa fornecedora e uma margem de segurança (reserva de contingência). É uma visão "mais cara".
   - Do ponto de vista do **fornecedor**: o orçamento é calculado com base nos itens de custo internos (quanto ele vai gastar para executar o projeto). Ele também considera entradas (recebimentos) e saídas (gastos) separadamente.

2. **Precisão real só existe ao final do projeto.** Durante a execução, o orçamento é uma estimativa — sujeita a mudanças, variações e refinamentos conforme o projeto avança.

3. **Ambientes estáveis geram estimativas mais precisas.** Em projetos com escopo bem definido e metodologia tradicional (cascata), as estimativas são mais confiáveis. Em projetos ágeis ou inovadores, onde mudanças são frequentes, os custos tendem a ser revisados com mais regularidade.

---

## 3. Processos da Gerência de Custos (PMBOK, Capítulo 7)

O PMBOK v6 define **4 processos** na área de gerenciamento de custos. Três pertencem ao grupo de **Planejamento** e um ao grupo de **Monitoramento e Controle**:

| Processo | Grupo | Descrição resumida |
|---|---|---|
| **7.1** Planejar o Gerenciamento dos Custos | Planejamento | Define *como* os custos serão gerenciados |
| **7.2** Estimar os Custos | Planejamento | Calcula os recursos monetários necessários |
| **7.3** Determinar o Orçamento | Planejamento | Agrega os custos e gera a linha de base |
| **7.4** Controlar os Custos | Monitoramento e Controle | Monitora gastos e gerencia desvios |

---

### 3.1 Processo 7.1 — Planejar o Gerenciamento dos Custos

> "O processo de definir como os custos do projeto serão **estimados, orçados, gerenciados, monitorados e controlados**."
> — PMBOK v6

É o processo de planejamento puro: antes de estimar qualquer valor, define-se *a forma como* toda a gestão de custos será conduzida no projeto.

**Entradas:**
- Termo de abertura do projeto
- Plano de gerenciamento do projeto
- Fatores ambientais da empresa
- Ativos de processos organizacionais

**Ferramentas e técnicas:**
- Opinião especializada
- Análise de dados
- Reuniões

**Saída principal:**
- **Plano de gerenciamento dos custos** — o artefato que orienta todos os outros processos de custo.

---

### 3.2 Processo 7.2 — Estimar os Custos

> "O processo de desenvolver uma **aproximação dos recursos monetários** necessários para terminar o trabalho do projeto."
> — PMBOK v6

Aqui o plano é colocado em prática: identificam-se e valoram-se todos os recursos necessários (mão de obra, hardware, software, servidores, treinamentos, consultorias, etc.).

**Entradas:**
- Plano de gerenciamento do projeto
- Documentos do projeto
- Fatores ambientais da empresa
- Ativos de processos organizacionais

**Ferramentas e técnicas** (principais):
- **Opinião especializada**: pessoas com conhecimento do domínio avaliam os custos.
- **Estimativa análoga**: usa dados de projetos anteriores similares como referência.
- **Estimativa paramétrica**: usa fórmulas e modelos estatísticos (ex: custo por ponto de função).
- **Estimativa *bottom-up***: detalha cada tarefa individualmente e soma tudo — é a mais precisa, mas também a mais trabalhosa.
- **Estimativa de três pontos**: considera três cenários — **otimista (O)**, **pessimista (P)** e **mais provável (M)** — para calcular uma estimativa ponderada.
- Análise de dados
- Sistema de informações de gerenciamento de projetos
- Tomada de decisões

**Saídas:**
- Estimativa de custos
- Bases das estimativas
- Atualizações de documentos do projeto

---

### 3.3 Processo 7.3 — Determinar o Orçamento *(destaque)*

> "O processo Determinar o Orçamento agrega os custos estimados de **atividades individuais ou pacotes de trabalho** para estabelecer uma **linha de base dos custos autorizada**. O principal benefício deste processo é a determinação da linha de base dos custos para o monitoramento e o controle do desempenho do projeto. Esse processo é realizado **uma vez ou em pontos predefinidos no projeto**."
> — PMBOK v6

Este é o processo mais destacado da área por sua importância central: é aqui que sai **a resposta para a pergunta do cliente** — "quanto vai custar?". O orçamento é gerado somando todos os custos estimados no processo 7.2, organizados a partir dos **pacotes de trabalho da EAP (Estrutura Analítica do Projeto)**.

O processo pode ser refeito em pontos predefinidos (ex: ao final de cada fase), permitindo revisões e ajustes conforme o projeto avança.

**Entradas:**
- Plano de gerenciamento do projeto
- Documentos do projeto
- Documentos do negócio
- Acordos
- Fatores ambientais da empresa
- Ativos de processos organizacionais

**Ferramentas e técnicas:**
- Opinião especializada
- **Agregação de custos**: soma dos custos dos pacotes de trabalho para formar o custo total.
- Análise de dados
- Revisão de informações históricas
- **Reconciliação dos limites de recursos financeiros**: alinha o orçamento calculado com os limites financeiros disponíveis.
- Financiamento

**Saídas:**
- **Linha de base dos custos** *(saída principal)* — o orçamento aprovado e autorizado, que serve de referência para todo o ciclo de vida do projeto.
- Requisitos de recursos financeiros do projeto
- Atualizações de documentos do projeto

---

### 3.4 Processo 7.4 — Controlar os Custos

> "O processo de monitoramento do status do projeto para **atualizar os custos** e **gerenciar mudanças da linha de base dos custos**."
> — PMBOK v6

É o único processo de custos que ocorre durante toda a fase de **execução** do projeto. Aqui o gerente compara os gastos reais com a linha de base definida no 7.3, identifica desvios e toma decisões para manter o projeto dentro do orçamento.

**Entradas:**
- Plano de gerenciamento do projeto
- Documentos do projeto
- Requisitos de recursos financeiros do projeto
- Dados de desempenho do trabalho
- Ativos de processos organizacionais

---

## 4. O Orçamento do Projeto

O orçamento é o **artefato central** da área de custos — uma referência durante todo o ciclo de vida do projeto. Ele deve conter:

1. **Os valores individuais de cada item de custo**, ponderados pela quantidade (ex: 5 computadores × R$ 2.000,00 = R$ 10.000,00).
2. **O valor total** do projeto (soma de todos os itens).
3. **As datas** em que cada valor será comprometido/desembolsado — não basta saber que o projeto custa R$ 1.000.000,00; é preciso saber que serão R$ 100.000,00 por mês ao longo de 10 meses.

**Qualquer alteração no orçamento deve ser justificada e aprovada formalmente pelo responsável.**

### 4.1 Exemplo de orçamento

| Recursos Necessários | Valor (R$) |
|---|---|
| Recursos humanos | 200.000,00 |
| Hardware | 25.000,00 |
| Rede | 2.400,00 |
| Software | 24.000,00 |
| Serviços | 5.000,00 |
| **TOTAL** | **256.400,00** |

> Observação: os recursos humanos costumam ser o **item mais caro** em projetos de software — mão de obra especializada representa a maior parcela do orçamento na grande maioria dos casos.

### 4.2 Perspectiva do cliente vs. perspectiva do fornecedor

O orçamento pode ser enxergado de dois ângulos distintos — e essa diferença é importante:

| Aspecto | Perspectiva do Cliente | Perspectiva do Fornecedor |
|---|---|---|
| **O que inclui** | Tudo que o cliente desembolsará: custo do fornecedor + lucro + margem de segurança | Itens de custo internos (quanto o fornecedor gastará para executar) |
| **Valor** | Maior (inclui lucro e reservas) | Menor (só os custos diretos de execução) |
| **Visão** | Saída de caixa | Entradas (recebimentos) e saídas (gastos) |

Na prática de gerência de projetos, o **orçamento voltado para o cliente** é o padrão de trabalho — é ele que determina o compromisso financeiro do projeto.

---

## 5. Resumo Visual: Os 4 Processos e seus Grupos

```
PLANEJAMENTO                          MONITORAMENTO E CONTROLE
┌──────────────────────────────────┐  ┌──────────────────────────────┐
│ 7.1 Planejar Gerenciamento       │  │ 7.4 Controlar os Custos      │
│ → Saída: Plano de ger. custos    │  │ → Compara real vs. linha base│
├──────────────────────────────────┤  └──────────────────────────────┘
│ 7.2 Estimar os Custos            │
│ → Saída: Estimativa de custos    │
├──────────────────────────────────┤
│ 7.3 Determinar o Orçamento       │
│ → Saída: Linha de base dos       │
│          custos (orçamento final)│
└──────────────────────────────────┘
```

---

## 6. O que mais cai em prova

- **Definição do PMBOK**: "O gerenciamento dos custos inclui os processos de planejamento, estimativa, orçamento, financiamento, gerenciamento e controle dos custos para que o projeto seja realizado dentro do orçamento aprovado." Saber essa definição com as palavras-chave corretas.
- **Os 4 processos (7.1 a 7.4)** e suas definições exatas: planejar (como será gerenciado), estimar (aproximação dos recursos monetários), determinar o orçamento (agregar → linha de base), controlar (monitorar e gerenciar mudanças).
- **A diferença entre estimar (7.2) e orçar (7.3)**: estimar calcula o valor de cada atividade; orçar agrega essas estimativas para gerar a linha de base autorizada.
- **Linha de base dos custos**: saber que é o orçamento aprovado e autorizado, saída do processo 7.3, e que serve como referência para monitoramento e controle durante todo o projeto.
- **Grupos de processos**: 7.1, 7.2 e 7.3 estão no grupo de **Planejamento**; 7.4 está no grupo de **Monitoramento e Controle**.
- **Técnicas de estimativa do 7.2**: análoga (projetos passados), paramétrica (fórmulas), *bottom-up* (detalha cada tarefa e soma — mais precisa), três pontos (otimista, pessimista, mais provável).
- **Relação custo × escopo × cronograma × recursos**: mudança em qualquer uma dessas áreas pode impactar os custos.
- **Perspectiva cliente vs. fornecedor** no orçamento: cliente vê o total desembolsado (com lucro e margens); fornecedor vê os custos de execução e o fluxo de entradas e saídas.
- **Precisão das estimativas**: maior em ambientes estáveis e projetos com metodologias tradicionais; menor em projetos ágeis ou com alto grau de inovação e mudança.
- **O orçamento deve conter datas**: não basta o valor total — é preciso indicar quando cada valor será gasto (cronograma financeiro), ligando-o ao cronograma físico do projeto.
- **Qualquer alteração no orçamento deve ser justificada e aprovada formalmente**.
