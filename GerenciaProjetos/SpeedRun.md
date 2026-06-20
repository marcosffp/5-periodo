# SPEED RUN — Gerência de Projetos

---

## CAP 7 — CUSTOS

**Def PMBOK**: processos de planejamento, estimativa, orçamento, financiamento, gerenciamento e controle dos custos para o projeto ser realizado **dentro do orçamento aprovado**.

**4 processos:**

| # | Processo | Grupo | Saída principal |
|---|---|---|---|
| 7.1 | Planejar Gerenciamento dos Custos | Planejamento | Plano de gerenciamento dos custos |
| 7.2 | Estimar os Custos | Planejamento | Estimativa de custos |
| 7.3 | Determinar o Orçamento | Planejamento | **Linha de base dos custos** |
| 7.4 | Controlar os Custos | Monitoramento e Controle | Informações de desempenho |

**Técnicas de estimativa (7.2):**
- **Análoga**: usa dados de projetos anteriores similares
- **Paramétrica**: fórmulas/modelos estatísticos (ex: custo por ponto de função)
- **Bottom-up**: detalha cada tarefa e soma → **mais precisa**
- **Três pontos**: (O + 4M + P) / 6 — considera otimista, mais provável e pessimista

**Macetes:**
- 7.2 *estima* → 7.3 *agrega e gera a linha de base* (resposta ao "quanto vai custar?")
- Linha de base dos custos = orçamento aprovado = referência para controle
- Recursos humanos = item mais caro em projetos de software
- Orçamento deve ter **valores + datas** (quando cada gasto ocorre)
- Custo ↔ Escopo ↔ Cronograma ↔ Recursos: mudança em um impacta os outros
- Perspectiva cliente > perspectiva fornecedor (inclui lucro e margem de segurança)

---

## CAP 8 — QUALIDADE

**Def PMBOK**: processos para incorporação da política de qualidade da organização com relação ao **planejamento, gerenciamento e controle dos requisitos de qualidade** do projeto e do produto para atender os **objetivos das partes interessadas**.

**4 autores:**

| Autor | Definição |
|---|---|
| Juran | Adequação ao uso |
| Deming | Melhoria contínua (TQM) |
| Crosby | Conformidade com os requisitos |
| Ishikawa | Mais econômico, mais útil, sempre satisfaz o consumidor |

**ISO 9000**: "grau em que um conjunto de características inerentes **atende aos requisitos**."
**Weinberg**: qualidade é relativa — o que é qualidade para um pode não ser para outro.

**3 processos:**

| # | Processo | Grupo | Saída principal |
|---|---|---|---|
| 8.1 | Planejar o Gerenciamento da Qualidade | Planejamento | Plano de gerenciamento da qualidade + Métricas |
| 8.2 | Gerenciar a Qualidade | Execução | Relatórios de qualidade, Solicitações de mudança |
| 8.3 | Controlar a Qualidade | Monitoramento e Controle | **Entregas verificadas** + Medições de controle |

**Técnicas estatísticas:**
- **Prevenção**: mantém erros **fora do processo** (pró-ativa)
- **Inspeção**: mantém erros **fora do cliente** (reativa)
- **Amostragem de atributos**: sim/não (conforme ou não)
- **Amostragem de variáveis**: escala contínua (grau de conformidade)
- **Tolerância**: faixa de resultados aceitáveis

**Macetes:**
- Prevenção > correção (sempre preferível e mais barata)
- Qualidade do produto resulta da qualidade do processo
- **8.2 e 8.3** não são responsabilidade exclusiva do GP (envolve equipe + área de qualidade da org.)
- Ferramenta: **Diagrama de Ishikawa** (espinha de peixe) — 6 categorias: Pessoas, Processo, Equipamento, Material, Ambiente, Gerenciamento → identifica causas raiz

---

## CAP 10 — COMUNICAÇÕES

**Def PMBOK**: processos para assegurar **geração, coleta, distribuição, armazenamento, recuperação e destinação final** das informações de forma oportuna e adequada.

**GP gasta ~80% do tempo em comunicação.**

**Dimensões da comunicação (5 pares):**

| Dimensão | Tipos |
|---|---|
| Alcance | Interna ↔ Externa |
| Formalidade | Formal ↔ Informal |
| Hierarquia | Vertical ↔ Horizontal |
| Oficialidade | Oficial ↔ Não oficial |
| Linguagem | Verbal ↔ Não verbal |

**Métodos de comunicação:**
- **Interativo**: bidirecional, mais eficaz (reuniões, videoconferências)
- **Push** (empurrar): enviado ao receptor, não garante leitura (e-mail, relatórios)
- **Pull** (puxar): receptor acessa quando quiser (intranet, base de conhecimento)

**3 processos:**

| # | Processo | Grupo | Saída principal |
|---|---|---|---|
| 10.1 | Planejar o Gerenciamento das Comunicações | Planejamento | **Plano de comunicação** |
| 10.2 | Gerenciar as Comunicações | Execução | Comunicações do projeto |
| 10.3 | Monitorar as Comunicações | Monitoramento e Controle | Informações de desempenho |

**Macetes:**
- FCS (Fatores Críticos de Sucesso) = **oposto dos riscos** → FCS foca no sucesso; riscos, no fracasso
- **Plano de Ação ≠ Registro de Riscos**: Plano de Ação (Issue Log) = problemas **já ocorridos**; Registro de Riscos = eventos **futuros e incertos**
- Plano de Comunicação (saída 10.1): tabela com Evento / Frequência / Responsável / Audiência
- **Ouvir ativo**: observar linguagem corporal + tom + verificar entendimento + demonstrar interesse + empatia

---

## CAP 11 — RISCOS

**Def**: riscos são **eventos incertos** que, caso aconteçam, provocarão **consequências aos objetivos do projeto**.

**2 dimensões de todo risco**: Probabilidade × Impacto → **Pontuação = P × I**

**2 tipos**: Ameaça (negativo) | Oportunidade (positivo) → GP foca nas **ameaças**

**Maximiano**: Risco = f(Incerteza × Complexidade). Maior complexidade + maior incerteza = maior risco.

**7 processos:**

| # | Processo | Grupo |
|---|---|---|
| 11.1 | Planejar o Gerenciamento dos Riscos | Planejamento |
| 11.2 | Identificar os Riscos | Planejamento |
| 11.3 | Realizar a Análise Qualitativa | Planejamento |
| 11.4 | Realizar a Análise Quantitativa | Planejamento |
| 11.5 | Planejar as Respostas aos Riscos | Planejamento |
| 11.6 | Implementar Respostas a Riscos | **Execução** |
| 11.7 | Monitorar os Riscos | **Monitoramento e Controle** |

> 5 dos 7 processos estão no Planejamento!

**O que cada processo faz:**
- **11.1**: define *como* conduzir os riscos no projeto
- **11.2**: identifica riscos individuais e documenta → **1ª etapa do tratamento**
- **11.3**: categoriza (Cliente / Equipe / Política Organizacional) e prioriza
- **11.4**: determina numericamente P e I → Matriz P×I; **Curva S**; Risco Global do Projeto (RGP)
- **11.5**: define estratégia para cada ameaça (5 opções abaixo)
- **11.6**: efetiva os planos acordados (PMO pode ajudar)
- **11.7**: acompanha durante **todo o ciclo de vida** — novos riscos surgem, outros somem, outros mudam

**5 estratégias de resposta a ameaças:**

| Estratégia | O que faz |
|---|---|
| **Escalar** | Passa para nível hierárquico superior |
| **Prevenir** | Pró-ativa — elimina a causa do risco |
| **Transferir** | Passa o risco a terceiro (ex: seguro, terceirização) |
| **Mitigar** | Reduz impacto quando o risco ocorre |
| **Aceitar** | Tolera — usada quando risco é **baixo** e custo de tratar > benefício |

**Macetes:**
- Riscos são minimizáveis, **nunca** totalmente elimináveis
- Aceitar = apenas monitorar enquanto o risco permanecer baixo
- Análise quantitativa = Matriz P×I (Pontuação = P × I) + Curva S
- RGP (Risco Global do Projeto) = indicador do risco total → usado em estudos de viabilidade
- Prado (2002): classificação prática = Não Há / Baixo / Médio / Alto + contramedidas
- Gestão de risco tem **custo** — definir quanto investir é decisão central

---

## CONTEXTO ORGANIZACIONAL

### FAE — Fatores Ambientais da Empresa
**Def PMBOK**: "condições **fora do controle** da equipe que influenciam, restringem ou direcionam o projeto." Podem ser + ou − para o projeto. São **entradas** nos processos de planejamento.

| Internos | Externos |
|---|---|
| Cultura / estrutura / governança | Condições de mercado |
| Distribuição geográfica | Influências sociais e culturais |
| Infraestrutura | Restrições legais |
| Software de TI | Bancos de dados comerciais |
| Disponibilidade de recursos | Pesquisa acadêmica |
| Capacidade dos funcionários | Padrões governamentais / setoriais |
| | Considerações financeiras |
| | Elementos ambientais físicos |

### APO — Ativos de Processos Organizacionais
**Def PMBOK**: "planos, processos, políticas, procedimentos e bases de conhecimento específicos da organização." 2 categorias:
1. Processos, políticas e procedimentos
2. Bases de conhecimento organizacionais

> **FAE ≠ APO**: FAE = condições do ambiente (a org. não controla); APO = patrimônio acumulado da org.

### 3 Tipos de Estrutura Organizacional

| | Funcional | Por Projeto (Projetizada) | Matricial |
|---|---|---|---|
| **Prioridade** | Rotina | Projeto | Ambos |
| **Bom para projetos?** | NÃO | SIM | SIM |
| **Bom para rotina?** | SIM | NÃO | SIM |
| **Hierarquia** | Muito rígida | Pouco rígida | Bi-dimensional |
| **Comunicação** | Lenta e burocrática | Fluida no projeto | Moderada |
| **Chefia** | Única | Única | **Dupla** |
| **Poder do GP** | Baixo | Alto | Compartilhado |
| **Estabilidade** | Alta | Baixa (papéis temporários) | Média |
| **Frequência no BR** | Muito comum | Rara | Menos comum |
| **Exemplo** | Indústria em geral | Consultoria | Empresas de TI |
| **Problema principal** | Projetos em 2º plano | Equipe fica sem alocação ao fim do projeto | Rotina "atropela" projetos; dupla chefia confunde |
| **Dificuldade de implantar** | Baixa | Média | **Alta** |

**PMO (Escritório de Projetos)**: área de **assessoria** (não de comando) — padroniza processos, define ferramentas, apoia GPs e gerentes funcionais. Aparece na estrutura matricial e ajuda na implementação/monitoramento de riscos.

**5 requisitos do GP no contexto organizacional**: Poder · Influência · Competência · Liderança · Capacidade de negociação

**Estruturas mistas**: combinam os três modelos. Variações matriciais: fraca (→ funcional) / equilibrada / forte (→ projetizada).

---

## MAPA GERAL — PROCESSOS POR GRUPO

```
PLANEJAMENTO                       EXECUÇÃO     MONITORAMENTO E CONTROLE
────────────────────────────────   ──────────   ────────────────────────
7.1 Planejar Custos                             7.4 Controlar Custos
7.2 Estimar Custos
7.3 Determinar Orçamento
────────────────────────────────   ──────────   ────────────────────────
8.1 Planejar Qualidade             8.2 Gerenciar Qualidade
                                                8.3 Controlar Qualidade
────────────────────────────────   ──────────   ────────────────────────
10.1 Planejar Comunicações         10.2 Gerenciar Comunicações
                                                10.3 Monitorar Comunicações
────────────────────────────────   ──────────   ────────────────────────
11.1 Planejar Riscos                            11.7 Monitorar Riscos
11.2 Identificar Riscos            11.6 Implementar
11.3 Análise Qualitativa           Respostas
11.4 Análise Quantitativa
11.5 Planejar Respostas
────────────────────────────────   ──────────   ────────────────────────
CONTAGEM:  Custos 3 | Qual 1 | Com 1 | Riscos 5 = 10 processos no plan.
```
