# Resumo — Introdução à Modelagem Arquitetural

## Contexto Geral

Modelagem arquitetural é o processo de representar a estrutura de um sistema por meio de elementos, relações e decisões de projeto. Não se trata apenas de um diagrama bonito: é o conjunto de escolhas estruturais que garantem que o sistema funcione dentro dos requisitos exigidos. Uma boa arquitetura comunica decisões, não apenas componentes.

## Principais Conceitos

A aula usou o sistema **PUC Connect** como estudo de caso — uma plataforma de comunicação entre alunos e professores, acessível por web e mobile.

A estrutura básica de qualquer sistema web segue três camadas: **Front-end** (interface com o usuário), **Back-end** (lógica de negócio) e **Database** (persistência dos dados). Cada camada tem opções tecnológicas próprias. No front-end, exemplos são React, Angular e Flutter. No back-end, Node, Java, .NET, Python, entre outros. No banco de dados, há opções relacionais (PostgreSQL, MySQL) e não relacionais (MongoDB, Cassandra).

Os **Requisitos Não Funcionais (RNFs)** são os que mais impactam a arquitetura. No PUC Connect, os principais foram: suportar 10.000 usuários simultâneos, garantir tempo de resposta máximo de 3 segundos em 95% das requisições, disponibilidade mínima de 99% e permitir expansão futura.

## Decisões Arquiteturais e Estratégias

Para atender aos RNFs, a arquitetura adotou o padrão **Monolito Modular** no back-end: todos os módulos (Usuário, Disciplina, Comunicação, Arquivos) ficam na mesma solução, mas com fronteiras bem definidas. Isso reduz a latência entre módulos (sem overhead de rede) e simplifica o controle de transações. O risco é o aumento de acoplamento se o design for mal feito.

Para **desempenho e escalabilidade**, foram aplicadas diversas estratégias: uso de **cache-aside** (lazy loading) para evitar leituras desnecessárias no banco; queries otimizadas com índices e desnormalização estratégica; processamento assíncrono (async/await); e **balanceamento de carga** com múltiplas réplicas do back-end gerenciadas por um **API Gateway + Load Balancer**. O front-end web usa **CDN** para distribuição de conteúdo.

Para **armazenamento de arquivos**, foi utilizado o **Azure Storage**, separando arquivos binários do banco de dados relacional.

## Diferença: Monolito Modular vs. Expansão

Um monolito modular bem estruturado permite tanto expansão de escopo (novos módulos como Fórum e Testes) quanto separação futura de módulos críticos em serviços independentes, caso um deles se torne gargalo. Isso equilibra simplicidade inicial com flexibilidade futura.

## O que mais cai em prova

- Definição de modelagem arquitetural (Bass, Clements e Kazman).
- RNFs são os que definem a arquitetura; sem eles, a arquitetura é apenas opinião.
- Monolito Modular: vantagens (baixa latência, menor complexidade) e riscos (acoplamento).
- Estratégias para desempenho: cache, load balancer, queries otimizadas, async.
- Arquitetura sem teste de carga é opinião — arquitetura validada é engenharia.
- O objetivo não é a arquitetura mais complexa, mas a mais coerente com os requisitos.
