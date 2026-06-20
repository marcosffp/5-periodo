# RESUMO — Gerência das Aquisições do Projeto

## 1. Contexto Geral

A Gerência das Aquisições é a área de conhecimento do PMBOK (Capítulo 12) responsável por garantir que todos os **insumos necessários para o projeto** — produtos, serviços e resultados externos — estejam disponíveis a tempo de serem utilizados.

> Segundo o PMBOK v6: "O gerenciamento das aquisições do projeto inclui os **processos necessários para comprar ou adquirir produtos, serviços ou resultados externos** à equipe do projeto."

Uma distinção crucial que aparece logo na introdução:

> **Só entram na gerência de aquisições os insumos voltados diretamente ao projeto.** Café, papel higiênico, cartuchos de impressora, mobiliário — esses são insumos da *organização*, não do projeto. Eles existiriam independentemente de o projeto existir ou não.

Exemplos do que **entra** na gestão de aquisições de projetos de software:
- Servidores e equipamentos de TI adquiridos para o projeto
- Softwares e licenças específicas
- Contratos de consultoria e assessoria
- Serviços de hospedagem em nuvem
- Contratação de empresa para avaliação de qualidade do produto

---

## 2. Conceito de Aquisição

### 2.1 Definição completa (PMBOK v6)

O gerenciamento das aquisições inclui não apenas a compra em si, mas os **processos de gerenciamento e controle** necessários para desenvolver e administrar acordos como:

| Instrumento | O que é |
|---|---|
| **Contrato** | Instrumento formal de compra e venda — sem ele, não há obrigação de pagamento nem de entrega |
| **Pedido de compra** (Ordem de compra / Solicitação de compra) | Documento que formaliza a intenção de adquirir um item |
| **Memorando de entendimento** | Documento que permite chegar a acordos sobre os termos de uma compra antes do contrato formal |
| **Acordos de Nível de Serviço — ANS** (SLA) | Definem os níveis de serviço que devem ser garantidos — podem ser externos (com fornecedores) ou internos (entre equipes) |

### 2.2 Aquisições dentro da Logística

O processo de aquisição **não é isolado** — ele faz parte de um processo maior suportado pela **logística**. Não basta especificar e comprar: é preciso gerir todo o ciclo:

```
Especificação → Orçamentos → Compra/Contrato → Entrega → Aceite/Utilização
```

A gerência de aquisições acompanha **desde a especificação do que será comprado até o aceite dos produtos entregues**.

### 2.3 Fatores que influenciam as aquisições

As aquisições devem ser **adaptadas ao contexto de cada projeto e organização**. Os principais fatores são:

- **Natureza da organização (pública vs. privada)**: organizações públicas seguem a **Lei de Licitações** (lei nacional + leis complementares), com processo mais burocrático e regulado. Empresas privadas têm mais flexibilidade, mas muitas adotam modelos derivados da licitação pública.
- **Burocracia interna**: o nível de aprovações e procedimentos internos para realizar uma compra varia bastante de empresa para empresa.
- **Disponibilidade de recursos financeiros**: sem recursos, as compras travam — o gerente de projeto precisa considerar isso no planejamento.
- **Modelo de gestão**: em modelos descentralizados, setores têm autonomia para comprar; em modelos centralizados, tudo passa por aprovação central.

---

## 3. Processos da Gerência de Aquisições (PMBOK, Capítulo 12)

O PMBOK v6 define **3 processos** para a área de aquisições, distribuídos em três grupos distintos:

| Processo | Grupo | Descrição resumida |
|---|---|---|
| **12.1** Planejar o Gerenciamento das Aquisições | Planejamento | Define o que será comprado, como e de quais fornecedores |
| **12.2** Conduzir as Aquisições | Execução | Obtém propostas, seleciona o vendedor e formaliza o contrato |
| **12.3** Controlar as Aquisições | Monitoramento e Controle | Monitora contratos ativos, aplica correções e encerra contratos |

---

### 3.1 Processo 12.1 — Planejar o Gerenciamento das Aquisições

> "O processo de **documentação das decisões de compra** do projeto, **especificando a abordagem** e **identificando vendedores em potencial**."
> — PMBOK v6

É neste processo que se decide **o que será comprado** e **como** — incluindo a decisão estratégica de **fazer ou comprar** (*Make or Buy*).

#### A decisão de Fazer ou Comprar (*Make or Buy*)

Uma das saídas mais importantes do 12.1. Ao identificar um item necessário, o gerente avalia:

1. **Fazer internamente** — a equipe produz o item (ex: desenvolver o software internamente).
2. **Comprar** — adquirir de um fornecedor externo (ex: comprar uma solução pronta).
3. **Alugar** — terceira opção frequentemente esquecida, mas válida (ex: alugar um servidor ao invés de comprar).

Essa decisão depende de custo, capacidade da equipe, prazo e estratégia da organização.

**Entradas:**
- Termo de abertura do projeto
- Plano de gerenciamento do projeto
- Documentos do projeto
- Fatores ambientais da empresa
- Ativos de processos organizacionais

**Ferramentas e técnicas:**
- Opinião especializada
- Coleta de dados
- Análise de dados
- **Análise para seleção de fontes** — avalia quais fornecedores estão aptos a entregar o que é necessário
- Reuniões

**Saídas:**
- **Plano de gerenciamento das aquisições** *(saída principal)*
- **Estratégia da aquisição**
- **Documentos de licitação** — pacote que vai a mercado para obter propostas
- **Especificação do trabalho das aquisições** — descreve tecnicamente o que se deseja comprar
- **Análise para seleção de fontes** (critérios de avaliação de fornecedores)
- **Decisões de fazer ou comprar**
- Estimativas de custo independentes
- Solicitações de mudanças
- Atualizações de documentos do projeto

---

### 3.2 Processo 12.2 — Conduzir as Aquisições

> "O processo de **obtenção de respostas de vendedores**, **seleção de um vendedor** e **adjudicação de um contrato**."
> — PMBOK v6

Aqui o planejamento vira ação: vai-se ao mercado, recebem-se propostas, seleciona-se o melhor fornecedor e formaliza-se o contrato.

#### O que é adjudicação de contrato?

**Adjudicação** é o ato formal de **aceitar e dar vigor ao contrato** com o vendedor selecionado. Quando um contrato é adjudicado, significa que foi corroborado por ambas as partes — o fornecedor é oficialmente escolhido e o acordo passa a ter validade.

**Entradas:**
- Plano de gerenciamento do projeto
- Documentos do projeto
- Documentação de aquisições
- **Propostas dos vendedores** — as respostas recebidas do mercado
- Fatores ambientais da empresa
- Ativos de processos organizacionais

**Ferramentas e técnicas:**
- Opinião especializada
- **Publicidade** — divulgar a necessidade de compra para atrair fornecedores
- **Reuniões com licitantes** — interação com os participantes do processo de seleção (*licitantes* = empresas participando de uma licitação)
- Análise de dados
- **Habilidades interpessoais e de equipe** — negociação é central nessa etapa

**Saídas:**
- **Vendedores selecionados** *(saída principal)*
- **Acordos** (contratos formalizados)
- Solicitações de mudança
- Atualizações do plano de gerenciamento do projeto
- Atualizações de documentos do projeto

---

### 3.3 Processo 12.3 — Controlar as Aquisições

> "O processo de **gerenciar relacionamentos de aquisições**, **monitorar o desempenho do contrato**, fazer alterações e correções conforme apropriado e **encerrar contratos**."
> — PMBOK v6

Depois que o contrato está assinado e o fornecedor está entregando, é preciso acompanhar: o serviço de hospedagem contratado está funcionando como esperado? O fornecedor está cumprindo os prazos e padrões do contrato? Se houver desvios, este processo os trata.

#### Administração de reivindicações

Uma ferramenta específica deste processo. Reivindicações podem surgir de ambos os lados:
- O **fornecedor** pode alegar que só consegue entregar em determinada data ou com determinada tecnologia.
- O **contratante** pode ter exigências que o fornecedor questiona.

Ambas precisam ser **negociadas e resolvidas** dentro do processo de controle das aquisições.

**Entradas:**
- Plano de gerenciamento do projeto
- Documentos do projeto
- Acordos (contratos ativos)
- Documentação de aquisições
- Solicitações de mudança aprovadas
- Dados de desempenho do trabalho
- Fatores ambientais da empresa
- Ativos de processos organizacionais

**Ferramentas e técnicas:**
- Opinião especializada
- **Administração de reivindicações**
- Análise de dados
- **Inspeção**
- **Auditorias**

**Saídas:**
- **Encerrar as aquisições** *(encerramento formal dos contratos concluídos)*
- Informações sobre o desempenho do trabalho
- Atualizações na documentação de aquisições
- Solicitações de mudança
- Atualizações do plano de gerenciamento do projeto
- Atualizações de documentos do projeto

---

## 4. Aquisições em Projetos — Artefatos

Para realizar uma aquisição em projeto, são utilizados artefatos específicos em cada etapa do processo:

| Artefato | Papel no processo |
|---|---|
| **Especificação técnica** | Define *o que* se quer comprar — requisitos técnicos do item |
| **Proposta** | Resposta do fornecedor ao processo de seleção — descreve como ele atenderia a demanda |
| **Orçamento** | Apresenta os valores cobrados pelo fornecedor |
| **Coleta de preços** | Comparação de propostas/orçamentos de múltiplos fornecedores para identificar a melhor opção |
| **Licitação** | Processo formal de seleção de fornecedor, obrigatório em empresas públicas — compõe especificação técnica + requisitos jurídicos + administrativos |
| **Contrato** | Instrumento formal que formaliza o acordo entre comprador e fornecedor — garante obrigações de entrega e pagamento |

> **Fluxo típico numa aquisição**: especificação técnica → licitação/coleta de preços → propostas dos vendedores → comparação → adjudicação do contrato → monitoramento → encerramento.

---

## 5. Dimensão Jurídica e Administrativa das Aquisições

A gerência de aquisições envolve fortemente as áreas **administrativa e jurídica** da organização — não é uma atividade puramente técnica. Isso porque:

- Qualquer compra gera um instrumento legal (contrato, pedido de compra, etc.)
- Em empresas públicas, há leis federais, estaduais e municipais que regulam cada etapa
- Mesmo em empresas privadas, contratos têm validade jurídica e precisam ser redigidos com cuidado
- O descumprimento de contratos pode gerar litígios e perdas financeiras ao projeto

---

## 6. Resumo Visual: Os 3 Processos e seus Grupos

```
PLANEJAMENTO              EXECUÇÃO                MONITORAMENTO E CONTROLE
┌────────────────────┐   ┌────────────────────┐   ┌────────────────────────┐
│ 12.1 Planejar o    │   │ 12.2 Conduzir as   │   │ 12.3 Controlar as      │
│ Gerenciamento das  │   │ Aquisições         │   │ Aquisições             │
│ Aquisições         │   │                    │   │                        │
│                    │   │ → Vai ao mercado   │   │ → Monitora contratos   │
│ → Define o que     │   │ → Recebe propostas │   │ → Gerencia desvios     │
│   comprar e como   │   │ → Seleciona        │   │ → Trata reivindicações │
│ → Make or Buy      │   │   fornecedor       │   │ → Encerra contratos    │
│                    │   │ → Adjudica contrato│   │                        │
│ → Plano de         │   │ → Vendedores       │   │ → Encerramento das     │
│   aquisições       │   │   selecionados     │   │   aquisições           │
│ → Docs de licitação│   │ → Acordos          │   │                        │
└────────────────────┘   └────────────────────┘   └────────────────────────┘
```

---

## 7. O que mais cai em prova

- **Definição do PMBOK**: "inclui os processos necessários para comprar ou adquirir produtos, serviços ou resultados externos à equipe do projeto." Decorar com as palavras-chave: **produtos, serviços ou resultados externos**.
- **O que entra e o que não entra**: só insumos diretamente voltados ao projeto. Café, papel, mobiliário geral = insumos da organização, não do projeto.
- **Os 3 processos (12.1, 12.2, 12.3)** e em qual grupo cada um pertence: 12.1 = Planejamento; 12.2 = Execução; 12.3 = Monitoramento e Controle.
- **Definições exatas dos processos pelo PMBOK**: 12.1 (documentação das decisões de compra, especificando a abordagem e identificando vendedores em potencial), 12.2 (obtenção de respostas de vendedores, seleção de um vendedor e adjudicação de um contrato), 12.3 (gerenciar relacionamentos de aquisições, monitorar desempenho, fazer correções e encerrar contratos).
- **Make or Buy (Fazer ou Comprar)**: decisão estratégica do 12.1 — fazer internamente, comprar de terceiros ou **alugar** (terceira opção). Saber que "alugar" é uma possibilidade válida.
- **Adjudicação de contrato**: ato formal de aceitar e dar valor jurídico ao contrato com o fornecedor selecionado — significa que o contrato foi corroborado pelas partes.
- **Administração de reivindicações**: ferramenta do 12.3 — trata disputas e exigências de ambos os lados (fornecedor e contratante) durante a execução do contrato.
- **Licitantes**: participantes de uma licitação — as empresas concorrendo ao fornecimento.
- **Organizações públicas** seguem a **Lei de Licitações** — processo mais burocrático e regulado que o privado.
- **Aquisições estão inseridas na logística**: não basta comprar — é preciso gerir todo o ciclo, da especificação ao aceite.
- **Artefatos das aquisições**: proposta, orçamento, coleta de preços, especificação técnica, licitação e contrato — saber o papel de cada um.
- **Acordos gerenciados**: contratos, pedidos de compra, memorandos de entendimento e ANS (Acordos de Nível de Serviço) internos — todos são instrumentos de aquisição.
