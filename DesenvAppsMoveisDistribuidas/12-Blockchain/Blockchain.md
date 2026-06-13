# Blockchain e Aplicações Descentralizadas
### Do problema de confiança ao livro-razão distribuído

**Disciplina:** Engenharia de Software — Aplicações Móveis e Distribuídas · PUC Minas / ICEI · 2026/1  
**Professor:** Cristiano de Macedo Neto  
**Aula 13** — Primeiro contato com o tema (sem pré-requisito técnico)

---

## Mapa da Aula

| Bloco | Tema | Slides |
|---|---|---|
| I | O Problema e a Solução | 2–4 |
| II | Smart Contracts e DApps | 5–6 |
| III | Blockchain Empresarial | 7–8 |
| IV | Trilemma e Limitações | 9–11 |

---

## Objetivos de Aprendizagem

Ao final desta aula você vai conseguir:

- Explicar o que é o duplo gasto e por que ele tornava dinheiro digital impossível antes de 2009
- Descrever como o encadeamento de blocos por hash garante imutabilidade sem autoridade central
- Distinguir o que um smart contract faz do que um banco de dados convencional faz
- Identificar o trilemma e o trade-off que cada blockchain importante faz

> **"Bitcoin is a purely peer-to-peer version of electronic cash [that] would allow online payments to be sent directly from one party to another without going through a financial institution."**
>
> NAKAMOTO, S. *Bitcoin: A Peer-to-Peer Electronic Cash System.* bitcoin.org/bitcoin.pdf, 31 out. 2008. p. 1.

---

## I. O Problema — Por que dinheiro digital era impossível antes de 2009

Antes de entender blockchain, precisamos entender o problema que ela resolve.

### O Duplo Gasto (*double-spending problem*)

Um arquivo digital pode ser copiado. Se eu tenho R$100 como arquivo, nada me impede tecnicamente de **enviar o mesmo arquivo para duas pessoas** ao mesmo tempo. Diferente de uma nota de dinheiro física, que só pode estar num lugar ao mesmo tempo.

**Cenário:** Maria tem um arquivo `R$100.dat`. Ela envia para João e, simultaneamente, para Pedro. Quem decide quem recebeu o dinheiro?

A solução clássica: um banco central guarda o registro de saldos e arbitra. Confiamos no banco para não mentir.

### Cronologia do Problema e da Solução

| Data | Evento |
|---|---|
| **Set/2008** | Lehman Brothers faliu. Crise financeira global. Desconfiança em bancos atinge pico histórico. |
| **31/out/2008** | Nakamoto publica whitepaper de 9 páginas: sistema de pagamento peer-to-peer sem intermediário. |
| **3/jan/2009** | Primeiro bloco minerado (Genesis Block). Embarcado: manchete do jornal sobre segundo resgate a bancos. |
| **2015** | Ethereum lança: blockchain programável com smart contracts. |
| **2017** | Hyperledger Fabric v1.0: blockchain permissionada para empresas. |

### A Pergunta de Nakamoto

> E se não precisássemos confiar em ninguém? Se o próprio sistema matemático garantisse que uma moeda só pode ser gasta uma vez, sem depender de nenhuma instituição?

Esta questão levou a uma das inovações mais citadas da computação distribuída: o blockchain como solução para o consenso Byzantine em redes abertas.

**Referências:** NAKAMOTO (2008); BANO et al., IEEE S&P 2019; LAMPORT, SHOSTAK, PEASE. *The Byzantine Generals Problem.* ACM TPLS, v.4, n.3, 1982.

---

## I. Como a Blockchain Resolve: Blocos, Hashes e Cadeia

A solução de Nakamoto tem três ingredientes simples: um **registro compartilhado**, um **mecanismo de impressão digital** (hash) e um **encadeamento** que torna qualquer alteração detectável.

### Passo 1 — O que é um Hash

> **Analogia: impressão digital dos dados**  
> Uma função de hash pega qualquer texto, qualquer tamanho, e produz uma "impressão digital" de tamanho fixo. Qualquer mudança mínima nos dados, mesmo uma vírgula, gera uma impressão digital completamente diferente.

**Exemplo:**

| Entrada | SHA-256 |
|---|---|
| `"Alice paga 1 BTC a Bob"` | `0000a3f8b2c9...` |
| `"Alice paga 2 BTC a Bob"` | `9f2e7c1a4b8d...` |

→ Hash completamente diferente mesmo com mudança mínima.

### Passo 2 — A Estrutura de um Bloco

```
BLOCO #820.001
├── Hash do bloco anterior: 0000a3f8b2c9d1e4f7...
├── Timestamp & Nonce:      2024-01-03 18:15:05 | nonce: 2083236893
├── Transações (Merkle Tree):
│     Tx1: Alice → Bob: 0.5 BTC
│     Tx2: Carol → Dave: 1.2 BTC
│     ...
└── Meu hash: 00007b2c4e9f... (calculado de tudo acima)
```

### Passo 3 — O Encadeamento que Garante Imutabilidade

Cada bloco guarda o **hash do bloco anterior**. Isso cria uma corrente. Se alguém tentar alterar uma transação antiga, o hash daquele bloco muda, tornando inválida a referência do próximo, que precisa ser recalculado, e assim por diante até o bloco mais recente.

```
[BLOCO #1]──→[BLOCO #2]──→[BLOCO #3]
 hash: 0000a3f8  prev: 0000a3f8   prev: 00007b2c
                 hash: 00007b2c   hash: 000034ef

 ↓ FRAUDE no bloco #1
[BLOCO #1*]──→ invalida #2 ──→ invalida #3
 hash: DIFERENTE!
```

> Alterar um bloco antigo exigiria recalcular o PoW de todos os blocos seguintes, **mais rápido do que a rede honesta continua a adicionar novos blocos**. Com milhares de mineradores no mundo, isso exigiria mais de 50% do poder computacional total da rede. Custo estimado de um ataque ao Bitcoin em 2024: bilhões de dólares por hora.

**Referências:** NAKAMOTO (2008), seções 4 e 5. MERKLE, R. *CRYPTO 1987.* NIST FIPS PUB 180-4, 2015. ZHENG et al. *IEEE BigData Congress 2017.*

---

## I. Quem Decide qual Bloco é Válido? Mecanismos de Consenso

Se várias pessoas tentam adicionar blocos ao mesmo tempo, quem "ganha"? Precisa haver uma regra que **todos sigam sem precisar confiar uns nos outros**.

### ⚙️ Proof of Work (PoW) — Bitcoin

Mineradores tentam trilhões de combinações de *nonce* por segundo. Quem primeiro encontrar um hash com N zeros iniciais "ganha" o direito de adicionar o próximo bloco e recebe a recompensa (atualmente **3,125 BTC**).

**Condição:** `SHA-256(cabeçalho + nonce)` deve começar com N zeros — N define a dificuldade, ajustada a cada 2.016 blocos para manter média de 10 min por bloco.

**Custo energético:** ~150 TWh/ano (Cambridge CBECI, 2024) — comparável ao consumo da Argentina. Ineficiência intencional: é o custo que torna o ataque inviável.

### Proof of Stake (PoS) — Ethereum (desde set/2022)

Validadores depositam capital (ETH como *stake*) proporcional à probabilidade de ser escolhido para propor o próximo bloco. Comportamento desonesto = perda do stake (*slashing*).

**The Merge (15/set/2022):** Ethereum migrou de PoW para PoS, reduzindo o consumo de energia em aproximadamente **99,95%**, de ~23 MW para ~0,01 MW.

### Comparativo PoW vs PoS

| Critério | PoW (Bitcoin) | PoS (Ethereum) |
|---|---|---|
| Como "compra" direito de votar | Gasta energia (CPU) | Deposita capital (ETH) |
| Penalidade por fraude | Gasto de energia perdido | Perda do stake (slashing) |
| Energia por ano | ~150 TWh ❌ | ~0,01 MW contínuo ✅ |
| Quem pode participar | Quem tem hardware ASIC | Quem tem 32+ ETH |
| Em uso desde | 2009 | Set. 2022 (The Merge) |

### A lógica por trás dos dois modelos

Ambos criam um custo para participar. No PoW, o custo é energia real gasta — um atacante precisaria gastar mais energia do que toda a rede honesta. No PoS, o custo é capital bloqueado — um atacante que tenta fraudar perde seu próprio dinheiro investido. Lógicas diferentes, mesma garantia fundamental.

### Por que Bitcoin não migrou para PoS?

A governança do Bitcoin é altamente descentralizada e conservadora. Mudar o mecanismo de consenso exigiria concordância de mineradores (que perderiam seus investimentos em hardware), detentores de moeda e desenvolvedores — grupos com interesses conflitantes. No Ethereum, a Ethereum Foundation tem mais influência sobre decisões de protocolo.

**Referências:** NAKAMOTO (2008), sec. 5. Cambridge CBECI 2024. ethereum.org/roadmap/merge. YAN, S. et al. *arXiv:2209.11545*, 2022. BANO et al. *ACM AFT '19.*

---

## II. Smart Contracts — Contratos que se Executam Sozinhos

O Bitcoin provou que é possível transferir valor sem intermediário. Em 2014, Vitalik Buterin perguntou: e se a blockchain pudesse executar *qualquer regra*, não apenas transferências de moeda?

### O que é um Smart Contract

> **Analogia: a máquina de refrigerante**  
> Nick Szabo cunhou o conceito em 1994. Você insere moedas (condição), a máquina libera o produto (execução automática). Sem vendedor, sem confiança necessária. O contrato está embutido na máquina.

Um smart contract é a versão digital: cláusulas contratuais codificadas em software, executadas automaticamente quando as condições são atendidas, sem possibilidade de intervenção de terceiros.

### Transferência via Banco vs Smart Contract

| | Banco Tradicional | Smart Contract (Ethereum) |
|---|---|---|
| Fluxo | Alice → Banco (valida, cobra, registra) → Bob | Alice → `if (condição) { pagar; }` → Bob |
| Confiança | Confia no banco. Banco pode congelar, cobrar taxa, negar. | Código público, imutável. Executado por ~10.000 nós. Ninguém pode bloquear ou alterar. |

### Casos de Uso

- **Escrow automático** — Pagamento liberado só após confirmação
- **Seguro automático** — Paga se chover. Dados do oráculo.
- **NFT de diploma** — Universidade emite, qualquer um verifica
- **DAO (governança)** — Votação on-chain, tesouro autônomo

### EVM — Ethereum Virtual Machine

O Ethereum adicionou ao Bitcoin uma **máquina virtual Turing-completa**: pode executar qualquer programa. O código do smart contract roda identicamente em todos os ~10.000 nós da rede. Nenhum nó pode ser subornado ou coagido a executar diferente.

**Gas:** cada operação tem um custo em "gas" (pago em ETH). Limita computação e evita loops infinitos — quem quiser travar a rede precisaria pagar para isso.

### ⚠️ O hack do The DAO (jun/2016) — lição fundamental

Um smart contract com US$150M em ETH tinha um bug de reentrada. O atacante drenava ETH recursivamente antes do saldo ser atualizado. Resultado: **US$60M drenados**.

Como o contrato era imutável, a única solução foi um **hard fork** polêmico (bloco #1.920.000, 20/jul/2016) que reverteu as transações. A comunidade se dividiu: **Ethereum (ETH) vs Ethereum Classic (ETC)**.

> **Lição:** imutabilidade é a propriedade central da segurança e também a limitação mais crítica. Bugs não têm patch. Auditorias de código são essenciais antes do deploy.

### O Problema do Oráculo

Smart contracts não conseguem acessar dados externos por conta própria. Um contrato de seguro agrícola que paga se chover precisa de um *oráculo* — entidade externa que fornece dados. Se o oráculo mentir, o contrato age com dados errados.

Toda a descentralização da blockchain depende de uma fonte de dados potencialmente centralizada. O **Chainlink** é a tentativa de resolver com múltiplos oráculos independentes.

**Referências:** SZABO (1994/1996). BUTERIN (2014), ethereum.org/whitepaper. WOOD, G. *Ethereum Yellow Paper*, 2014. MEHAR et al. *J. Cases on Information Technology*, v.21, n.1, 2019.

---

## III. Blockchain Empresarial — Pública vs Privada

Bitcoin e Ethereum são públicas: qualquer pessoa do mundo pode participar anonimamente. Mas e se os participantes já são conhecidos? E se os dados precisam ser confidenciais?

### Comparativo: Pública vs Privada

| Critério | Blockchain Pública (Bitcoin/Ethereum) | Blockchain Privada (Hyperledger Fabric) |
|---|---|---|
| Participantes | Anônimos, desconhecidos | Identificados via X.509 |
| Dados | Todos públicos | Canais privados (sigilosos) |
| Consenso | Lento (PoW/PoS) | Rápido (BFT) |
| Throughput | 7–30 TPS | ~3.500 TPS |
| Criptomoeda | Obrigatória | Não necessária |

**Como decidir qual usar:**
- Os participantes **não** são conhecidos → **Blockchain Pública**
- Os participantes **são** conhecidos e há relação jurídica → **Blockchain Privada**

> **A pergunta mais importante antes de adotar blockchain:** se todos os participantes já confiassem uns nos outros, um banco de dados relacional comum resolveria o problema de forma mais simples, rápida e barata. Blockchain faz sentido quando há múltiplas partes que precisam de um registro comum *sem* confiar num único controlador.

### Hyperledger Fabric — Blockchain para Negócios

Framework de blockchain permissionada mantido pela Linux Foundation, com forte participação da IBM. Lançado em 2015, versão 1.0 em 2017. Smart contracts se chamam **chaincode** e podem ser escritos em Go, Java ou Node.js.

**Diferenciais técnicos:**
- Identidade via certificados X.509
- Canais privados (dados sigilosos)
- Consenso BFT: ~3.500 TPS
- Sem criptomoeda obrigatória

### Caso Real: IBM Food Trust (Walmart)

```
Fazenda → Transporte → Distribuidor → Supermercado
  (lote,    (temperatura,  (recepção,   (cliente escaneia
   data,     GPS,           rastreio)    QR code)
   insumos)  chegada)

          [Hyperledger Fabric — registro imutável compartilhado]
```

| | Antes | Depois |
|---|---|---|
| Rastrear origem de um alimento | **7 dias** (ligações telefônicas, papéis) | **2 segundos** (consulta ao ledger) |

**Outros casos reais:**
- **Trade finance:** carta de crédito entre bancos internacionais
- **Saúde:** prontuário compartilhado entre hospitais
- **Diplomas:** emissão verificável sem repositório central

**Referências:** ANDROULAKI et al. *EuroSys '18.* arXiv:1801.10228. IBM Food Trust: ibm.com/food-trust. CACHIN & VUKOLIC. *arXiv:1707.01873*, 2017.

---

## IV. O Trilemma da Blockchain

Buterin formalizou em 2015: das três propriedades desejadas, **qualquer melhora significativa em uma tende a prejudicar ao menos outra**. Não é um bug — é uma restrição estrutural.

### As Três Propriedades em Tensão

```
         Descentralização
         (muitos nós, sem dono)
               △
              /|\
             / | \
            /  |  \
           / escolhe\
          /   2 de 3 \
Segurança ────────── Escalabilidade
(resistência         (alto TPS)
a ataques)
```

### Posicionamento de cada Blockchain

| Blockchain | TPS | Trade-off |
|---|---|---|
| **Bitcoin** | 7 TPS | Maximiza segurança + descentralização. Sacrifica escalabilidade. |
| **Ethereum** | ~30 TPS | Equilíbrio. Layer 2 para escalar. |
| **Solana** | 65k TPS* | Maximiza escalabilidade. Críticas sobre centralização e 5 outages em 2022. |
| **Hyperledger Fabric** | 3.500 TPS | Abandona anonimato. Ganha velocidade e privacidade. |

*Solana: 65k TPS teórico; média real ~2.000–5.000 TPS na mainnet*

### Por que é uma Restrição Estrutural

**Descentralização vs Escalabilidade:** Mais nós significa mais descentralização — mas cada transação precisa ser validada por todos os nós. Aumentar o tamanho dos blocos processa mais transações, mas encarece operar um nó, reduzindo o número de participantes e concentrando o poder.

**Segurança vs Escalabilidade:** Validação mais rápida aumenta o TPS, mas reduz o tempo para detectar nós maliciosos. Reduzir o número de validadores (menos nós = mais rápido) facilita ataques de conluio.

### Estado do Trilemma em 2026

Em janeiro de 2026, Buterin afirmou que o Ethereum "resolveu" o trilemma com **PeerDAS** (amostragem de disponibilidade de dados) e **ZK-EVMs**. A comunidade técnica reconhece o progresso real mas aguarda verificação sob carga de longo prazo em produção.

ZK-EVM em produção: Polygon zkEVM (mar. 2023), zkSync Era (mar. 2023), Scroll (out. 2023).

**Referências:** BUTERIN. "Ethereum Sharding FAQ", 2015. MENDES et al. *arXiv:2005.06665*, 2020. ZHOU et al. *IEEE Access*, v.8, 2020.

---

## IV. O que o Mercado Não Conta sobre Blockchain

A maioria das apresentações de blockchain foca nos benefícios. Para tomar decisões técnicas honestas, é necessário conhecer os limites reais.

### Diagrama de Decisão: Usar ou Não Blockchain?

```
Múltiplos participantes sem confiança mútua?
├── NÃO → Use banco de dados convencional
└── SIM
    Dados precisam ser públicos ou participantes são anônimos?
    ├── SIM → Blockchain Pública (Bitcoin, Ethereum)
    └── NÃO
        Participantes conhecidos, dados confidenciais?
        └── SIM → Blockchain Privada / Consórcio (HLF, Quorum)
```

### Limitações Reais que Iniciantes Ignoram

**⚠️ Imutabilidade é faca de dois gumes**  
Bugs em smart contracts são permanentes. The DAO (2016): US$60M roubados, solução só via hard fork polêmico. GDPR/LGPD exige direito ao esquecimento — como apagar dados de um registro imutável?

**⚠️ UX e a barreira real de adoção**  
Criar uma carteira exige entender chaves públicas, chaves privadas e seed phrases. Perder a chave privada = perder todos os ativos, para sempre. Estima-se que 3–4 milhões de BTC (~20% do total) estejam inacessíveis por chaves perdidas.

**⚠️ Performance: Blockchain vs Banco de Dados**

| | TPS |
|---|---|
| Bitcoin | 7 TPS |
| PostgreSQL / Visa | 100.000+ TPS |

### Quando Blockchain NÃO é a Resposta

- Banco de dados com único controlador confiável
- Dados que precisam ser deletados (LGPD Art. 18)
- Performance crítica (>1.000 TPS)
- Participantes que já confiam uns nos outros
- Regulação que exige identificação (KYC/AML)

**Referências:** POLITOU et al. *J. Cybersecurity*, v.4, n.1, 2018. Chainalysis Crypto Crime Report 2024. WUST & GERVAIS. *IEEE CVCBT 2018.* DOI: 10.1109/CVCBT.2018.00011.

---

## Checkpoint — Questões de Verificação

### Questão 1 — A cadeia de hashes

O bloco #5.000 de uma blockchain contém a transação "Alice envia 10 BTC a Bob". Um atacante altera essa transação para "Alice envia 0 BTC a Bob". O que acontece com os blocos #5.001, #5.002 e todos os seguintes?

- A) Nada. Cada bloco é independente e armazena apenas suas próprias transações.
- B) ✅ **CORRETA** — O hash do bloco #5.000 muda. O bloco #5.001 guarda o hash antigo do #5.000, que agora está incorreto. Para consertar o #5.001 seria preciso recalculá-lo, o que muda seu hash, invalidando o #5.002, e assim em cascata até o bloco mais recente.
- C) O bloco alterado é automaticamente rejeitado pela rede antes de qualquer efeito.
- D) Apenas o bloco seguinte (#5.001) é afetado.

> **Explicação:** O encadeamento por hash garante que qualquer alteração produz efeito em cascata. Alterar o bloco #5.000 muda seu hash, tornando inválido o campo "hash anterior" do #5.001. Para corrigir o #5.001 seria necessário recalculá-lo, gerando um novo hash que invalida o #5.002, e assim por diante até o bloco mais recente. Refazer todo esse trabalho computacional em PoW exigiria mais poder que a rede inteira — economicamente inviável.

---

### Questão 2 — Smart contract e o oráculo

Uma seguradora cria um smart contract no Ethereum que paga R$10.000 automaticamente se a temperatura numa cidade cair abaixo de -5°C por três dias consecutivos. Qual é o ponto central de vulnerabilidade?

- A) O contrato pode ser alterado após o deploy se a empresa detectar um bug.
- B) O Solidity não suporta lógica de datas e temperatura.
- C) ✅ **CORRETA** — O smart contract não consegue acessar dados meteorológicos por conta própria. Ele depende de um oráculo externo. Se esse oráculo falhar, for hackeado ou reportar dados incorretos, o contrato executa com base em informações falsas, destruindo a lógica descentralizada.
- D) O gas necessário excederia o limite de bloco do Ethereum.

> **Explicação:** O Solidity suporta plenamente lógica de datas e comparações. Gas limits afetam complexidade computacional, não tipo de lógica. Smart contracts não podem ser alterados após o deploy (imutabilidade é propriedade, não vulnerabilidade). O problema central é que o contrato não consegue acessar dados do mundo real por conta própria — precisa de um oráculo externo que reintroduz uma dependência centralizada na arquitetura descentralizada. Chainlink tenta mitigar com múltiplos oráculos independentes.

---

### Questão 3 — Escolha arquitetural

Três bancos brasileiros querem criar um sistema para liquidar transferências entre si com registro auditável permanente, sem depender de um banco central como intermediário. Os três bancos são conhecidos, possuem relação jurídica e os dados das transações são confidenciais. Qual é a arquitetura mais adequada?

- A) Bitcoin, por ser a blockchain mais segura e descentralizada.
- B) Ethereum público com smart contracts.
- C) ✅ **CORRETA** — Hyperledger Fabric ou blockchain de consórcio: participantes identificados por X.509, canais privados para dados confidenciais, consenso BFT com ~3.500 TPS, sem necessidade de criptomoeda ou mineração.
- D) Um banco de dados relacional distribuído com replicação síncrona entre os três bancos.

> **Explicação:** Bitcoin e Ethereum público expõem todas as transações a qualquer pessoa — incompatível com dados confidenciais entre bancos concorrentes. Bitcoin tem 7 TPS e Ethereum ~30 TPS — insuficiente para liquidação bancária. Um banco de dados distribuído resolve o problema técnico, mas sem eliminar a necessidade de confiar em quem controla o banco. O Hyperledger Fabric foi criado exatamente para participantes conhecidos que precisam de registro compartilhado imutável com dados confidenciais e sem autoridade central.

---

## Síntese — O Fio Condutor

```
Problema: duplo gasto
(dinheiro digital pode ser copiado)
               ↓
Blockchain: ledger distribuído
(hash encadeado + consenso = imutabilidade sem banco central)
          ↙                    ↘
PoW (Bitcoin)              PoS (Ethereum)
gasta energia, 7 TPS       deposita capital, 30 TPS
     ↓                              ↓
Blockchain Privada          Smart Contracts (Ethereum)
HLF: BFT, 3.500 TPS, X.509  código que executa sem intermediário
          ↓
Trilemma: escolha 2 de 3
(descentralização + segurança + escalabilidade)
          ↓
Use quando: múltiplos participantes sem confiança mútua
Não use quando: banco de dados convencional resolve
```

### Conexão com o Semestre

Blockchain é um sistema distribuído como os estudados no início do semestre — com consenso Byzantine (Aula 07), consistência eventual e tolerância a falhas. A diferença: opera em redes abertas com participantes anônimos e sem confiança prévia, exigindo incentivos econômicos (criptomoeda) como substituto da confiança institucional.

---

## Referências Fundamentais

- **Nakamoto (2008):** bitcoin.org/bitcoin.pdf — o whitepaper original, 9 páginas
- **Buterin (2014):** ethereum.org/whitepaper — blockchain programável
- **Szabo (1994/1996):** Smart contracts — precursor conceitual
- **Androulaki et al. (2018):** Hyperledger Fabric. *EuroSys '18.* arXiv:1801.10228
- **Bano et al. (2019):** SoK: Consensus. *ACM AFT '19.* DOI: 10.1145/3318041.3355458
- **Zhou et al. (2020):** Scalability solutions. *IEEE Access.* DOI: 10.1109/ACCESS.2020.2967218
- **Wüst & Gervais (2018):** Do You Need a Blockchain? *IEEE CVCBT.* DOI: 10.1109/CVCBT.2018.00011

---

*Esta é a última aula de conteúdo da disciplina. As próximas aulas serão de revisão geral e avaliação final. Consulte o cronograma no AVA.*