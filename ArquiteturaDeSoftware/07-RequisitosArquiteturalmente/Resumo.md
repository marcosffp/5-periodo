# Resumo — Requisitos Arquiteturalmente Significativos (RAS) — Aula 9

Ao projetar um sistema de software, o arquiteto recebe uma grande lista de requisitos. No entanto, nem todos eles impactam a estrutura do sistema da mesma forma. Alguns requisitos apenas descrevem funcionalidades simples, enquanto outros obrigam o arquiteto a repensar completamente como o sistema será construído. Esses últimos são chamados de Requisitos Arquiteturalmente Significativos, ou RAS.

Segundo Bass, Clements e Kazman, RAS são os requisitos que exercem influência direta e mensurável sobre a arquitetura do sistema. Em outras palavras, são aqueles que, se ignorados ou mal tratados, comprometem o funcionamento, o desempenho ou a estrutura do sistema como um todo.

Para entender a diferença, considere dois tipos de requisito. Um Requisito Funcional (RF) descreve o que o sistema deve fazer: permitir que alunos enviem mensagens, que usuários façam login ou redefinam senhas. Esses requisitos são importantes, mas raramente mudam a arquitetura. Já um Requisito Não Funcional (RNF) com impacto arquitetural, como suportar 50 mil conexões simultâneas, garantir 99% de disponibilidade em dias úteis, ou prover acesso imediato a dados históricos, força o arquiteto a tomar decisões estruturais: usar balanceadores de carga, replicação de servidores, cache, bancos distribuídos, entre outros.

Portanto, os RAS surgem da combinação de RF, RNF e concerns arquiteturais, que são as preocupações técnicas e de negócio dos stakeholders. O processo para identificá-los segue uma sequência lógica: levantar requisitos com os stakeholders, refiná-los, classificá-los em funcionais e não funcionais, registrar os concerns arquiteturais de cada parte envolvida, e então filtrar quais desses requisitos realmente impactam a qualidade e a estrutura do sistema. O resultado desse processo é justamente a lista de RAS.

A distinção prática é clara: requisitos funcionais comuns podem ser implementados sem alterar a arquitetura, enquanto os RAS determinam tecnologias, padrões, topologia de infraestrutura e decisões de design que afetam o sistema como um todo.

Para a prova, o mais importante é: saber definir RAS e distingui-los de requisitos funcionais comuns; entender que RAS geralmente estão ligados a atributos de qualidade como desempenho, disponibilidade, escalabilidade, segurança e auditabilidade; e conhecer o processo de identificação que vai dos stakeholders até a lista final de RAS. Lembre-se também que RAS podem vir tanto de RNF quanto de RF complexos que impõem restrições estruturais.



## Requisitos Arquiteturalmente Significativos (RAS) — Alugue Fácil

---

**RAS 01 — Escalabilidade horizontal**
O sistema deve suportar até 1 milhão de usuários ativos com crescimento nacional, obrigando o uso de arquitetura distribuída, balanceamento de carga e escalonamento independente por serviço.

---

**RAS 02 — Baixo acoplamento entre módulos**
Os módulos devem ser deployados de forma independente e crescer gradualmente, forçando uma separação clara de responsabilidades — orientando para microserviços ou SOA com contratos bem definidos.

---

**RAS 03 — Integração com múltiplos provedores externos**
A plataforma depende de serviços externos de pagamento, antifraude, validação documental, geolocalização, e-mail e push notification, exigindo uma camada de integração desacoplada (ex: API Gateway, adaptadores) para não criar dependência direta com fornecedores.

---

**RAS 04 — Comunicação assíncrona para notificações e eventos**
Eventos como confirmação de aluguel, pagamento e disputa precisam disparar notificações e atualizar múltiplos módulos, exigindo uso de filas ou mensageria (ex: RabbitMQ, Kafka) para garantir desacoplamento e resiliência.

---

**RAS 05 — Busca por localização**
A busca de itens disponíveis por localização geográfica exige tecnologia especializada de indexação geoespacial, impactando diretamente a escolha do banco de dados e da infraestrutura de busca.

---

**RAS 06 — Segurança, privacidade e conformidade com a LGPD**
Dados pessoais, documentos e reconhecimento facial exigem criptografia em repouso e em trânsito, controle de acesso granular e mecanismo de exclusão de dados, impactando a estrutura de armazenamento e os fluxos de identidade.

---

**RAS 07 — Verificação documental e reconhecimento facial**
O cadastro depende de integrações com serviços externos de validação de identidade, exigindo um fluxo assíncrono tolerante a latência e com tratamento de falhas, pois o tempo de resposta desses provedores pode ser imprevisível.

---

**RAS 08 — Pagamentos dentro da plataforma**
O processamento de pagamentos exige atomicidade nas transações, rastreabilidade financeira e integração segura com gateway externo, impactando o modelo de dados e exigindo mecanismos de idempotência para evitar cobranças duplicadas.

---

**RAS 09 — Gestão de disputas e seguros**
Disputas e acionamento de seguro envolvem múltiplos módulos (aluguel, pagamento, usuário) e estados complexos, exigindo um fluxo de orquestração ou coreografia de eventos com rastreabilidade de cada etapa.

---

**RAS 10 — Disponibilidade multi-plataforma**
A plataforma deve funcionar em Web, Android e iOS, exigindo APIs padronizadas (REST/GraphQL) consumíveis por qualquer cliente, sem lógica acoplada a um canal específico.