# Resumo — Requisitos Arquiteturalmente Significativos (RAS) — Aula 9

Ao projetar um sistema de software, o arquiteto recebe uma grande lista de requisitos. No entanto, nem todos eles impactam a estrutura do sistema da mesma forma. Alguns requisitos apenas descrevem funcionalidades simples, enquanto outros obrigam o arquiteto a repensar completamente como o sistema será construído. Esses últimos são chamados de Requisitos Arquiteturalmente Significativos, ou RAS.

Segundo Bass, Clements e Kazman, RAS são os requisitos que exercem influência direta e mensurável sobre a arquitetura do sistema. Em outras palavras, são aqueles que, se ignorados ou mal tratados, comprometem o funcionamento, o desempenho ou a estrutura do sistema como um todo.

Para entender a diferença, considere dois tipos de requisito. Um Requisito Funcional (RF) descreve o que o sistema deve fazer: permitir que alunos enviem mensagens, que usuários façam login ou redefinam senhas. Esses requisitos são importantes, mas raramente mudam a arquitetura. Já um Requisito Não Funcional (RNF) com impacto arquitetural, como suportar 50 mil conexões simultâneas, garantir 99% de disponibilidade em dias úteis, ou prover acesso imediato a dados históricos, força o arquiteto a tomar decisões estruturais: usar balanceadores de carga, replicação de servidores, cache, bancos distribuídos, entre outros.

Portanto, os RAS surgem da combinação de RF, RNF e concerns arquiteturais, que são as preocupações técnicas e de negócio dos stakeholders. O processo para identificá-los segue uma sequência lógica: levantar requisitos com os stakeholders, refiná-los, classificá-los em funcionais e não funcionais, registrar os concerns arquiteturais de cada parte envolvida, e então filtrar quais desses requisitos realmente impactam a qualidade e a estrutura do sistema. O resultado desse processo é justamente a lista de RAS.

A distinção prática é clara: requisitos funcionais comuns podem ser implementados sem alterar a arquitetura, enquanto os RAS determinam tecnologias, padrões, topologia de infraestrutura e decisões de design que afetam o sistema como um todo.

Para a prova, o mais importante é: saber definir RAS e distingui-los de requisitos funcionais comuns; entender que RAS geralmente estão ligados a atributos de qualidade como desempenho, disponibilidade, escalabilidade, segurança e auditabilidade; e conhecer o processo de identificação que vai dos stakeholders até a lista final de RAS. Lembre-se também que RAS podem vir tanto de RNF quanto de RF complexos que impõem restrições estruturais.
