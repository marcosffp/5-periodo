**RESUMO — Trade-offs Arquiteturais**

**Introdução**

Em arquitetura de software, nenhum sistema consegue ser ao mesmo tempo extremamente rápido, seguro, sempre disponível e totalmente consistente. Assim como escolher um meio de transporte envolve ceder em algum aspecto (conforto, custo, velocidade), projetar um sistema exige escolhas conscientes entre atributos que competem entre si. Essas escolhas têm nome: trade-offs arquiteturais.

**O que são Trade-offs**

Trade-offs são as consequências inevitáveis das decisões arquiteturais. Ao melhorar um atributo de qualidade do sistema (como desempenho), outro atributo tende a ser prejudicado (como consistência). Não existe arquitetura perfeita — existem apenas escolhas bem justificadas. O erro não está em ter trade-offs, mas em ignorá-los ou não documentá-los.

**Principais tipos de Trade-offs**

Os mais comuns são: Desempenho vs Consistência (usar cache acelera o sistema, mas pode servir dados desatualizados), Disponibilidade vs Consistência (manter o sistema no ar pode significar retornar dados inconsistentes), Segurança vs Usabilidade (autenticação em dois fatores protege, mas reduz praticidade), e Disponibilidade vs Custo.

**Teorema CAP**

Proposto por Eric Brewer, o CAP afirma que sistemas distribuídos só conseguem garantir dois dos três atributos simultaneamente: Consistência (C), Disponibilidade (A) e Tolerância a Partições (P). Na prática, como partições de rede sempre podem ocorrer, a escolha real é entre CP (dados consistentes, mas pode negar requisições em falhas) e AP (sistema sempre responde, mas pode retornar dados temporariamente desatualizados — consistência eventual). A combinação CA não existe em sistemas distribuídos reais sob falha de rede.

**Teorema PACELC**

O PACELC, proposto por Daniel Abadi, complementa o CAP ao cobrir o que ele ignora: o comportamento do sistema em condições normais, sem falha de rede. Na ausência de partição, o trade-off passa a ser entre Latência (L) e Consistência (C). Ou seja, mesmo quando tudo funciona bem, o arquiteto precisa decidir: responder rápido com dados locais (possível inconsistência) ou esperar todos os nós sincronizarem (dados corretos, porém mais lento).

**O papel do Arquiteto**

O arquiteto não elimina trade-offs, mas os gerencia. Para isso, precisa avaliar impactos, justificar escolhas com base nos requisitos do negócio e assumir as consequências. O fluxo típico vai dos RAS (Requisitos Arquiteturalmente Significativos) para os trade-offs, depois para as decisões, e finalmente para a arquitetura do sistema.

**O que mais cai em prova**

Definição de trade-off arquitetural, os três atributos do CAP e o que cada combinação (CP, AP, CA) representa, a limitação do CAP e como o PACELC a resolve (latência vs consistência em operação normal), os principais tipos de trade-off (desempenho, consistência, disponibilidade, segurança), e o papel do arquiteto como mediador de escolhas justificadas.