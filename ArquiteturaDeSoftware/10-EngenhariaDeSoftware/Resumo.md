# Arquitetura de Software — Avaliação e Estudo de Caso

A Engenharia de Software é a disciplina que cuida do ciclo de vida de um sistema: projetar, implementar, entregar, monitorar e sustentar. Dentro dela, a Arquitetura de Software é a camada mais estratégica — ela define as decisões estruturais de um sistema, aquelas que são difíceis de mudar depois que o produto já está rodando. Martin Fowler resume bem: arquitetura é justamente o conjunto dessas decisões de alto impacto.

O arquiteto de software não escreve só código; ele pensa em como os componentes do sistema se encaixam e como esse conjunto vai se comportar ao longo do tempo. Diferente da engenharia de software em geral, que foca no processo de construção, a arquitetura foca nas decisões estruturais que afetam todo o resto.

Os principais desafios que um arquiteto enfrenta são técnicos e de negócio. Do lado técnico: performance (velocidade de resposta), escalabilidade (suportar mais carga), disponibilidade (sistema no ar), confiabilidade (funcionar corretamente), segurança, observabilidade (monitorar o que está acontecendo), integração com outros sistemas e lidar com aplicações legadas. Do lado de negócio: custos, prazos, legislação, inovação e comprometimento das partes envolvidas.

Empresas como iFood, Nubank, Mercado Livre, Netflix e Uber têm em comum dois pontos críticos: alta complexidade e alta volumetria. Ou seja, muitas funcionalidades interligadas e milhões de acessos simultâneos. Qualquer sistema, grande ou pequeno, sofre as consequências de decisões ruins tomadas na sua concepção.

O estudo de caso prático envolve três cenários reais de arquitetura a serem reformulados:

Problema 1 — JackieStore: e-commerce de roupas rodando em uma única máquina virtual com ASP.NET MVC e SQL Server. Os sintomas são lentidão em horários de pico, inconsistência no status de pagamentos e indisponibilidade ocasional. Esses problemas apontam para falta de escalabilidade horizontal, ausência de cache, banco de dados sobrecarregado e ausência de filas para processar pagamentos de forma assíncrona e confiável.

Problema 2 — JackieStore mobile: o mesmo e-commerce precisa lançar apps para Android e iOS. A arquitetura atual, monolítica e baseada em renderização server-side, não suporta nativamente APIs para consumo mobile. A solução passa por separar o backend em uma API REST ou GraphQL, desacoplando a interface web dos apps móveis.

Problema 3 — SAEWEB notificações: plataforma escolar que precisa enviar notificações push unidirecionais para professores, gestores, pais e alunos, com confirmação de leitura e entrega mesmo com o app fechado. A solução envolve integração com serviços de push notification (como FCM do Google e APNs da Apple), um sistema de filas para envio assíncrono e armazenamento do histórico de notificações com status de leitura por usuário.

Para a prova, foque em: definição de arquitetura de software (decisões difíceis de mudar), diferença entre Engenharia e Arquitetura de Software, os atributos de qualidade (escalabilidade, disponibilidade, confiabilidade, observabilidade, segurança), os problemas típicos de sistemas legados monolíticos e como cada cenário do estudo de caso exemplifica falhas arquiteturais concretas.
