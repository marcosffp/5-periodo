# Resumo — Aula 7: Stakeholders e Concerns Arquiteturais

## Introdução

A Arquitetura de Software não existe por si mesma. Segundo Bass, Clements e Kazman, ela existe para satisfazer os interesses das pessoas e organizações envolvidas em um sistema. Sem essas pessoas — os chamados **stakeholders** — não há razão para falar em atributos de qualidade, preocupações arquiteturais ou trade-offs.

---

## Stakeholders

**Stakeholder** é qualquer indivíduo, grupo ou organização que tem interesse no sistema ou é afetado por ele. Exemplos práticos: alunos, professores, equipe de TI, gestores, acionistas, parceiros e a própria universidade.

O ponto central é que stakeholders **influenciam diretamente as decisões arquiteturais**. Ignorá-los é um dos principais motivos de fracasso em projetos de software.

---

## Concerns (Preocupações Arquiteturais)

**Concern** é um interesse, requisito ou consideração que um stakeholder possui em relação ao sistema. É a forma como cada parte envolvida expressa o que espera do software — em linguagem do dia a dia, não técnica.

Exemplos práticos por perfil:

- **Aluno**: acesso rápido, organização das disciplinas, comunicação fácil com professores.
- **Professor**: publicação simples de conteúdo, gestão centralizada de avaliações.
- **TI**: fácil manutenção, integração com outros sistemas.
- **Universidade**: redução de custos, disponibilidade contínua, segurança.

---

## Concerns x Atributos de Qualidade — Diferença Importante

Concerns **não são** a mesma coisa que Atributos de Qualidade. Essa é uma pegadinha clássica de prova.

O concern é a preocupação do stakeholder, expressa de forma natural. O **Atributo de Qualidade** é a interpretação técnica do arquiteto a partir desse concern:

| Concern do Stakeholder | Atributo de Qualidade |
| --- | --- |
| "Quero acesso rápido" | Desempenho |
| "Precisa estar sempre no ar" | Disponibilidade |
| "Fácil de usar" | Usabilidade |
| "Fácil de manter" | Manutenibilidade |

---

## Impacto na Arquitetura e Trade-offs

Cada concern gera decisões arquiteturais concretas. Exemplos:

- **Desempenho** → uso de cache, CDN, load balancer.
- **Disponibilidade** → réplicas de banco de dados, elasticidade.
- **Custo reduzido** → escalabilidade elástica, tecnologias open-source.
- **Usabilidade** → design responsivo, acessibilidade, apps nativos.

Nem sempre todos os concerns podem ser atendidos ao mesmo tempo. Quando há conflito entre preocupações, o arquiteto precisa tomar decisões de **trade-off** — ou seja, equilibrar interesses opostos. Exemplo clássico: segurança alta (autenticação multifator) versus facilidade de uso. A solução é encontrar um equilíbrio negociado com os stakeholders.

---

## Conclusão — O que mais cai em prova

- Arquitetura existe para satisfazer os interesses dos stakeholders.
- Concern é a preocupação do stakeholder; Atributo de Qualidade é a tradução técnica feita pelo arquiteto — são conceitos **diferentes**.
- Concerns influenciam decisões arquiteturais, que por sua vez definem os atributos de qualidade do sistema.
- O arquiteto precisa identificar stakeholders, mapear seus concerns e equilibrar trade-offs quando houver conflito entre preocupações.
- Ignorar stakeholders pode comprometer todo o projeto.
