# RESUMO — Gerência de Riscos do Projeto

## 1. Contexto Geral

A Gerência de Riscos é a área de conhecimento do PMBOK (Capítulo 11) responsável por identificar, analisar e responder a riscos ao longo do ciclo de vida do projeto. Riscos existem em todos os contextos da vida humana — financeiro, engenharia, saúde — e em projetos não é diferente.

> Segundo o PMBOK v6: o gerenciamento dos riscos do projeto "inclui os processos de condução do planejamento, identificação, análise, planejamento de respostas, implementação de respostas e monitoramento de riscos em um projeto."

A área de riscos está intimamente ligada ao conceito **oposto** ao de Fatores Críticos de Sucesso (FCS): enquanto os FCS enumeram o que leva o projeto ao êxito, a gerência de riscos enumera o que pode levá-lo ao fracasso.

---

## 2. Conceito de Risco

### 2.1 Definição

> **Riscos em projetos são eventos incertos que, caso aconteçam, poderão provocar consequências aos objetivos do projeto.**

Três elementos definem o risco em projetos:
1. **Incerteza**: o risco nunca é certo — se fosse, seria um fato, não um risco;
2. **Probabilidade de ocorrência**: a chance de o evento acontecer;
3. **Impacto**: o dano ou prejuízo que o evento causaria ao projeto.

> **Risco = f(Probabilidade × Impacto)** — somente analisando as duas dimensões juntas é possível avaliar um risco corretamente.

### 2.2 Riscos Positivos e Negativos

| Tipo | Nome | Característica |
|---|---|---|
| **Positivo** | Oportunidade | Chance de melhoria, inovação ou ganho; abre novas possibilidades |
| **Negativo** | Ameaça | Evento destrutivo que pode impedir o alcance dos objetivos do projeto |

> Os riscos que mais preocupam o gerente de projetos são as **ameaças**, pois são destrutivas e podem impedir o alcance do objetivo do projeto. O risco positivo (oportunidade) é tratado de forma separada nas respostas.

### 2.3 Por que os Riscos são Importantes?

Os riscos devem ser uma das maiores preocupações em projetos de tecnologia. Os principais motivos:

- Riscos estão associados a **perdas financeiras**;
- Um risco pode levar ao **fracasso** do projeto como um todo;
- As **pessoas da equipe** podem ser afetadas, ficando desestimuladas ou com baixa autoestima;
- Os **patrocinadores** não gostariam de ver o projeto sucumbir diante de riscos não tratados;
- A **equipe pode não estar preparada** para enfrentar os riscos a que o projeto está sujeito.

Além disso, os **processos de software envolvem riscos** por natureza.

---

## 3. Origens dos Riscos: Incerteza e Complexidade

O autor **Maximiano (2002)** propõe um diagrama que relaciona três elementos fundamentais:

- **Incerteza**: determinada pelo grau de domínio sobre a tecnologia envolvida;
- **Complexidade**: determinada pelo número de variáveis do projeto;
- **Risco**: resultado da combinação dos dois.

```
                         INCERTEZA
                            ▲
                    Maior   │
          ┌─────────────────┼──────────────────┐
          │ Pequeno nº de   │ Grande nº de      │
          │ variáveis +     │ variáveis +        │
          │ elevada         │ elevada incerteza  │
          │ incerteza       │                    │
          │                 │ Grandes projetos   │
          │ Projetos mono-  │ multidisciplinares │
          │ disciplinares   │ de P&D             │  → RISCO ALTO
          │ de pesquisa     │                    │
          ├─────────────────┼──────────────────┤
          │ Pequeno nº de   │ Grande nº de      │
          │ variáveis +     │ variáveis +        │
          │ pouca           │ pouca incerteza    │
          │ incerteza       │                    │
          │                 │ Projetos que       │
          │ Pequenos        │ demandam           │
          │ projetos em     │ organização        │
          │ geral           │ complexa           │
          └─────────────────┼──────────────────┘
                    Menor   │                     Maior
                            └────────────────────────► COMPLEXIDADE
```

> **Conclusão de Maximiano**: deve-se atuar sobre **incerteza** e **complexidade** para minimizar riscos. Projetos com grande complexidade e grande incerteza são os de maior risco.

---

## 4. Processos da Gerência de Riscos (PMBOK, Capítulo 11)

O PMBOK v6 define **7 processos** para a área de riscos:

| Processo | Grupo | Descrição resumida |
|---|---|---|
| **11.1** Planejar o Gerenciamento dos Riscos | Planejamento | Define como conduzir as atividades de gerenciamento de riscos |
| **11.2** Identificar os Riscos | Planejamento | Identifica riscos individuais e fontes de risco geral |
| **11.3** Realizar a Análise Qualitativa | Planejamento | Prioriza riscos por probabilidade, impacto e outras características |
| **11.4** Realizar a Análise Quantitativa | Planejamento | Analisa numericamente o efeito combinado dos riscos |
| **11.5** Planejar as Respostas aos Riscos | Planejamento | Desenvolve alternativas e estratégias para lidar com os riscos |
| **11.6** Implementar Respostas a Riscos | Execução | Implementa os planos acordados de resposta aos riscos |
| **11.7** Monitorar os Riscos | Monitoramento e Controle | Monitora riscos identificados, identifica novos e avalia eficácia |

> **Atenção**: diferentemente da maioria das áreas de conhecimento (que têm 3 processos), a gerência de riscos possui **7 processos** — a maioria deles no grupo de planejamento.

---

### 4.1 Processo 11.1 — Planejar o Gerenciamento dos Riscos

> "Processo de **definição de como conduzir as atividades de gerenciamento dos riscos** de um projeto."
> — PMBOK v6

É nesse processo que se define a **política de gerenciamento de riscos** do projeto: quais ações serão tomadas, quem será responsável, qual modelo será usado. O planejamento se baseia no conhecimento existente na organização e no tipo de projeto que se realiza.

Organizações mais maduras já possuem **listas de riscos pré-definidas** que servem como referência e facilitam as etapas seguintes.

**Entradas:**
- Termo de abertura do projeto
- Plano de gerenciamento do projeto
- Documentos do projeto
- Fatores ambientais da empresa
- Ativos de processos organizacionais

**Ferramentas e técnicas:**
- Opinião especializada
- Análise de dados
- Reuniões

**Saída principal:**
- **Plano de gerenciamento dos riscos**

---

### 4.2 Processo 11.2 — Identificar os Riscos

> "Processo de **identificação dos riscos individuais do projeto**, bem como fontes de risco geral do projeto, e de documentar suas características."
> — PMBOK v6

É o **estudo individualizado** dos riscos que podem atingir o projeto. Esta é a **primeira etapa no processo de tratamento de riscos** — sem identificação, não há como gerenciar.

**Entradas:**
- Plano de gerenciamento do projeto
- Documentos do projeto
- Acordos
- Ativos de processos de aquisições
- Fatores ambientais da empresa
- Ativos de processos organizacionais

**Ferramentas e técnicas:**
- Opinião especializada
- Coleta de dados
- Análise de dados
- Habilidades interpessoais e de equipe
- **Listas de alertas** (checklists de riscos conhecidos)
- Reuniões

**Saídas:**
- **Registro dos riscos** *(saída principal — lista de todos os riscos identificados)*
- Relatório de riscos
- Atualizações de documentos do projeto

---

### 4.3 Processo 11.3 — Realizar a Análise Qualitativa dos Riscos

> "Processo de **priorização de riscos individuais do projeto** para análise ou ação posterior, através da avaliação de sua probabilidade de ocorrência e impacto, assim como outras características."
> — PMBOK v6

Após identificar os riscos, faz-se a **qualificação**: determina-se de que tipo ou grupo cada risco se trata, organizando-os em categorias para análise. O objetivo é saber quais riscos merecem atenção imediata e quais podem ser desconsiderados ou monitorados.

**Exemplo de categorias de risco** (Machado, 2002):

| Categoria | Exemplos de fatores de risco |
|---|---|
| **Cliente** | Ausência de participação, resistência a mudanças, conflitos entre clientes, atitudes negativas |
| **Equipe de Desenvolvimento** | Membros treinados inadequadamente, inexperientes, falta de boas práticas, conflitos internos |
| **Política Organizacional** | Recursos retirados do projeto, mudanças na gerência, políticas corporativas negativas, instabilidade organizacional |

**Entradas:**
- Plano de gerenciamento do projeto
- Documentos do projeto
- Fatores ambientais da empresa
- Ativos de processos organizacionais

**Ferramentas e técnicas:**
- Opinião especializada
- Coleta de dados
- Análise de dados
- Habilidades interpessoais e de equipe
- **Categorização dos riscos**
- Representação de dados
- Reuniões

**Saída principal:**
- Atualizações de documentos do projeto (registro de riscos com prioridades)

---

### 4.4 Processo 11.4 — Realizar a Análise Quantitativa dos Riscos

> "Processo de **analisar numericamente o efeito combinado** dos riscos individuais identificados no projeto e outras fontes de incerteza nos objetivos gerais do projeto."
> — PMBOK v6

Após conhecer os riscos qualitativamente, determina-se **numericamente** a probabilidade e o impacto de cada um. Um projeto não deve ser iniciado sem antes realizar esse estudo — seria como **entrar num quarto escuro sem uma lanterna**.

**Como funciona:**
- Cada risco recebe um valor de probabilidade (0 a 1) e um valor de impacto (0 a 1);
- A pontuação individual é: **Pontuação = Probabilidade × Impacto**;
- O conjunto de pontuações individuais resulta no **Risco Global do Projeto (RGP)** — indicador que mede o risco do projeto como um todo (baixo, médio ou alto).

**Exemplo — Matriz Probabilidade × Impacto:**

| Risco | Probabilidade | Impacto | Pontuação |
|---|---|---|---|
| Viabilidade do fornecedor | 0,4 | 0,8 | **0,32** |
| Capacidade de resposta do fornecedor | 0,2 | 0,4 | **0,08** |
| Compatibilidade do software | 0,4 | 0,4 | **0,16** |
| Compatibilidade do hardware | 0,4 | 0,4 | **0,16** |
| Conexão com computador central | 0,8 | 0,8 | **0,64** |
| Treinamento | 0,2 | 0,2 | **0,04** |

**Curva S** — outra ferramenta da análise quantitativa: mostra a relação entre a **probabilidade cumulativa de conclusão** do projeto e a **previsão de custo total**. À medida que o projeto avança, o risco tende a diminuir e a probabilidade de sucesso tende a aumentar.

**Entradas:**
- Plano de gerenciamento do projeto
- Documentos do projeto
- Fatores ambientais da empresa
- Ativos de processos organizacionais

**Ferramentas e técnicas:**
- Opinião especializada
- Coleta de dados
- Habilidades interpessoais e de equipe
- **Representações da incerteza**
- Análise de dados (incluindo Simulação de Monte Carlo)

**Saída principal:**
- Atualizações de documentos do projeto (registro de riscos com análise quantitativa)

---

### 4.5 Processo 11.5 — Planejar as Respostas aos Riscos

> "Processo de **desenvolver alternativas, selecionar estratégias e acordar ações** para lidar com a exposição geral de riscos, e também tratar os riscos individuais do projeto."
> — PMBOK v6

Uma vez determinados os riscos negativos (ameaças) e suas probabilidades e impactos, é necessário definir **qual ação será adotada** para cada um. Há **5 estratégias** possíveis para ameaças:

| Estratégia | Postura | Como funciona |
|---|---|---|
| **Escalar** | — | Transfere a decisão para um nível hierárquico superior ao do GP; usado quando o risco está fora do seu escopo de atuação |
| **Prevenir** | Pró-ativa | Toma medidas para **evitar que o risco ocorra**; antecipa e neutraliza a causa raiz |
| **Transferir** | Pró-ativa | Passa o risco para um terceiro (ex: contratar seguro, terceirizar equipe especializada) |
| **Mitigar** | Reativa | Reduz o impacto ou a probabilidade quando o risco se concretiza |
| **Aceitar** | Flexível | Não toma nenhuma medida preventiva; geralmente usada quando o risco é **baixo** e o custo de tratá-lo supera o benefício |

> **Regra prática**: aceitar o risco é uma atitude válida quando o impacto é tão baixo que não vale o investimento em medidas preventivas. Enquanto o risco se mantiver baixo, apenas **monitora-se**.

**Entradas:**
- Plano de gerenciamento do projeto
- Documentos do projeto
- Fatores ambientais da empresa
- Ativos de processos organizacionais

**Ferramentas e técnicas:**
- Opinião especializada
- Coleta de dados
- Habilidades interpessoais e de equipe
- **Estratégias para ameaças** (Escalar, Prevenir, Transferir, Mitigar, Aceitar)
- **Estratégias para oportunidades**
- **Estratégias de respostas de contingência**
- **Estratégias para o risco geral do projeto**
- Análise de dados
- Tomada de decisões

**Saídas:**
- Solicitações de mudança
- Atualizações do plano de gerenciamento do projeto
- Atualizações de documentos do projeto

---

### 4.6 Processo 11.6 — Implementar Respostas a Riscos

> "Processo de **implementar planos acordados de resposta aos riscos**."
> — PMBOK v6

Não basta definir as respostas — elas devem ser efetivamente **implementadas**. Esse processo exige do gerente de projeto **visão e atitude**: agir na hora certa e da maneira correta.

> Um **Escritório de Projetos (PMO)**, caso exista na organização, pode ajudar bastante nesse processo — tanto na implementação quanto no monitoramento das respostas.

**Entradas:**
- Plano de gerenciamento do projeto
- Documentos do projeto
- Ativos de processos organizacionais

**Ferramentas e técnicas:**
- Opinião especializada
- Habilidades interpessoais e de equipe
- **Sistema de informações de gerenciamento de projetos (SIGP)**

**Saídas:**
- Solicitações de mudança
- Atualizações de documentos do projeto

---

### 4.7 Processo 11.7 — Monitorar os Riscos

> "Processo de **monitorar a implementação de planos acordados de resposta aos riscos**, acompanhar riscos identificados, identificar e analisar novos riscos, e avaliar a eficácia do processo de risco ao longo do projeto."
> — PMBOK v6

O monitoramento acontece **durante todo o ciclo de vida do projeto**. Apenas ter um Plano de Gerenciamento de Riscos não é suficiente — os riscos são dinâmicos:

- **Novos riscos** podem surgir;
- Riscos anteriores podem **desaparecer**;
- A **avaliação** (probabilidade e/ou impacto) de riscos existentes pode **aumentar ou diminuir**.

**Entradas:**
- Plano de gerenciamento do projeto
- Documentos do projeto
- Dados de desempenho do trabalho
- Relatórios de desempenho do trabalho

**Ferramentas e técnicas:**
- Análise de dados
- Auditorias
- Reuniões

**Saídas:**
- **Informações sobre o desempenho do trabalho** *(saída principal)*
- Solicitações de mudança
- Atualizações do plano de gerenciamento do projeto
- Atualizações de documentos do projeto
- Atualizações de ativos de processos organizacionais

---

## 5. Determinação Prática de Riscos

Na prática, a abordagem mais comum para determinar riscos em projetos segue o método proposto por **Prado (2002)**:

### 5.1 Passo a passo

1. **Determinação qualitativa**: classifica cada fator de risco em uma escala — **Não Há / Baixo / Médio / Alto**;
2. Pode-se também atribuir um **valor percentual** e um **peso** a cada fator;
3. Calcula-se o **Fator de Risco Global do Projeto (RGP)** — valor agregado de todos os riscos individuais, representado por uma classe ou número real (0 a 1);
4. Quanto **maior o número**, maior o risco; quanto **menor**, maior o fator de sucesso.

### 5.2 Exemplo de planilha de riscos com contramedidas

| Fonte do Risco | Classificação | Contramedida |
|---|---|---|
| Tecnologia do projeto | Médio — razoável domínio sobre a tecnologia | Treinamento da equipe |
| Disponibilidade de recursos | Médio — alguns recursos não garantidos | Contratação de recursos externos |
| Dificuldade de integração externa | Médio — participação do usuário com dificuldades | Reforçar compromisso com presença e planejamento |
| Cronograma apertado | Médio — prazo pode ser insuficiente | Horas extras e trabalho reforçado |
| Falta de conhecimento técnico | Médio — conhecimentos técnicos limitados | Treinamento da equipe |
| Recursos terceirizados | **Baixo** — fácil contratação | *(sem contramedida — apenas monitorar)* |

> **Resultado da análise**: a tabela termina com uma classificação geral do projeto — Não Há / Baixo / Médio / Alto — usada inclusive em **estudos de viabilidade**.

---

## 6. É Possível Conviver com Riscos?

> "Embora os riscos sejam indesejáveis, eles nem sempre são evitáveis. Geralmente, é possível **minimizar** riscos mas não **eliminá-los** totalmente."

A questão essencial não é eliminar todos os riscos, mas definir conscientemente:

**Quanto será investido em gestão de riscos neste projeto?**

Todo tratamento de risco tem um **custo financeiro**. Nem sempre há recursos disponíveis para enfrentar todos os riscos. A decisão sobre quais riscos tratar e com qual intensidade é uma das tarefas centrais do GP e dos stakeholders do projeto.

---

## 7. Resumo Visual: Os 7 Processos e seus Grupos

```
PLANEJAMENTO                                            EXECUÇÃO   MONITORAMENTO
┌──────────────────────────────────────────────────┐  ┌──────────┐  ┌──────────┐
│ 11.1 Planejar   → Plano de gerenciamento de      │  │ 11.6     │  │ 11.7     │
│                   riscos                         │  │ Implementar  │ Monitorar│
│ 11.2 Identificar → Registro dos riscos           │  │ Respostas│  │ os       │
│                                                  │  │ a Riscos │  │ Riscos   │
│ 11.3 Análise     → Registro c/ prioridades       │  │          │  │          │
│      Qualitativa                                 │  │ Efetiva  │  │ Acompanha│
│                                                  │  │ os planos│  │ novos e  │
│ 11.4 Análise     → Matriz P×I, Curva S,          │  │ acordados│  │ extintos │
│      Quantitativa  Risco Global do Projeto       │  │          │  │          │
│                                                  │  │          │  │          │
│ 11.5 Planejar    → Estratégias: Escalar,         │  │          │  │          │
│      Respostas     Prevenir, Transferir,         │  │          │  │          │
│                    Mitigar, Aceitar              │  │          │  │          │
└──────────────────────────────────────────────────┘  └──────────┘  └──────────┘
```

---

## 8. O que mais cai em prova

- **Definição de risco**: "eventos incertos que, caso aconteçam, poderão provocar consequências aos objetivos do projeto." Palavras-chave: **incerto**, **consequências**, **objetivos**.
- **Duas dimensões do risco**: **probabilidade** de ocorrência e **impacto**. Somente analisando as duas é possível avaliar um risco corretamente. Pontuação = Probabilidade × Impacto.
- **Riscos positivos e negativos**: positivos = oportunidades; negativos = ameaças. O GP foca principalmente nas **ameaças**.
- **Risco × FCS**: FCS (Fatores Críticos de Sucesso) são o oposto dos riscos — enumeram o que leva ao sucesso, não ao fracasso.
- **Maximiano (incerteza × complexidade)**: incerteza = domínio sobre a tecnologia; complexidade = número de variáveis. Projetos com **alta complexidade E alta incerteza** apresentam o **maior risco**.
- **Os 7 processos (11.1 a 11.7)** e seus grupos: 11.1 a 11.5 = Planejamento; 11.6 = Execução; 11.7 = Monitoramento e Controle. **Atenção**: 5 dos 7 processos estão no Planejamento.
- **Definições exatas de cada processo**: 11.1 (definir como conduzir), 11.2 (identificar riscos individuais e documentar), 11.3 (priorizar por probabilidade e impacto), 11.4 (analisar numericamente o efeito combinado), 11.5 (desenvolver alternativas e estratégias), 11.6 (implementar planos acordados), 11.7 (monitorar implementação e identificar novos riscos).
- **11.2 Identificar os Riscos**: é a **primeira etapa** do tratamento de riscos. Organizações maduras possuem listas pré-definidas de riscos.
- **5 estratégias de resposta a ameaças**: **Escalar** (passa para nível superior), **Prevenir** (pró-ativa — evita o risco), **Transferir** (passa a terceiro — ex: seguro), **Mitigar** (reativa — reduz impacto), **Aceitar** (tolera o risco, geralmente quando o impacto é baixo).
- **Aceitar** o risco: atitude válida quando o risco é **baixo** e o custo de tratá-lo supera o benefício; enquanto se mantiver baixo, apenas **monitora-se**.
- **11.6 Implementar Respostas**: surgiu mais recentemente no PMBOK v6; o **PMO (Escritório de Projetos)** pode ajudar bastante nesse processo e no de monitoramento.
- **11.7 Monitorar**: deve ocorrer durante **todo o ciclo de vida** do projeto; riscos são dinâmicos — novos surgem, outros desaparecem, outros mudam de avaliação.
- **Análise qualitativa**: classifica riscos em categorias — Cliente, Equipe de Desenvolvimento, Política Organizacional (Machado, 2002).
- **Análise quantitativa**: determina numericamente probabilidade e impacto; usa **Matriz Probabilidade × Impacto** e **Curva S**; um projeto não deve iniciar sem essa análise.
- **Curva S**: gráfico de probabilidade cumulativa de conclusão vs. custo total do projeto; à medida que o projeto avança, o risco tende a diminuir e a probabilidade de sucesso tende a aumentar.
- **Risco Global do Projeto (RGP)**: valor agregado de todos os riscos individuais; indica se o projeto é de risco baixo, médio ou alto; usado em estudos de viabilidade.
- **Prado (2002) — determinação prática**: classificação por gradação (Não Há / Baixo / Médio / Alto); planilha com Fonte do Risco, Classificação e Contramedida.
- **Gestão de riscos tem custo**: qualquer medida de tratamento exige investimento financeiro; a questão central é decidir **quanto investir** em gestão de riscos em cada projeto.
- **Riscos são minimizáveis, não elimináveis**: todo projeto apresenta algum risco residual; o objetivo é reduzir a incidência e os impactos, tornando os riscos menos relevantes.
