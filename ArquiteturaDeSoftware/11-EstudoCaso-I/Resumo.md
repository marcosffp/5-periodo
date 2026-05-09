# Avaliação de Arquitetura: Estudo de Caso JackieStore

O estudo de caso usa um e-commerce com problemas reais para ensinar como um arquiteto deve pensar antes de propor soluções. A lição central é: diagnostique antes de agir, e considere o negócio, não só a tecnologia.

## O Problema

A JackieStore rodava num monolito em uma única VM: aplicação e banco na mesma máquina. Os sintomas eram lentidão nos picos, indisponibilidade ocasional e inconsistência no pagamento (cobrado no cartão mas pedido ficava pendente). Tudo piorava nos finais de semana.

## A Armadilha Comum

Alunos tenderam a propor reescritas completas ou microservices do zero. O professor aponta que isso ignora o contexto: o sistema está em produção com clientes reais, e uma reescrita leva meses. Enquanto isso, o problema continua. Essa é a diferença entre trade-off técnico e trade-off de negócio.

## A Abordagem Correta: Evolução Incremental

Ação 1 - Separar banco de dados em VM dedicada, liberar cores do SQL e coletar estatísticas. Ação 2 - Escalar horizontalmente a aplicação com um pool de VMs e colocar um Load Balancer na frente. Ação 3 - Implantar monitoramento (DataDog, Application Insights) para identificar gargalos reais. Ação 4 - Aplicar cache com Redis via padrão Cache-Aside nas leituras frequentes. Ação 5 - Converter fluxos lentos e síncronos em assíncronos via fila de mensagens, especialmente pagamento e emissão de documentos. Ação 6 - Para suportar os apps mobile, construir uma REST API nova em paralelo ao monolito, estrangulando o legado gradualmente sem derrubá-lo.

## As Três Lições

Lição 1: existem trade-offs de negócio além dos técnicos. Toda decisão arquitetural é uma decisão de negócio. Lição 2: pense como dono da empresa, considerando reputação, clientes e saúde financeira. Lição 3: diagnosticar corretamente já é metade da solução. Trocar peças sem entender o problema gasta tempo e dinheiro sem resolver nada.

## O que mais cai em prova

Saber identificar qual problema exige qual ação (lentidão = escala + cache; inconsistência = processamento assíncrono; indisponibilidade = redundância + load balancer). Entender que monolito não é necessariamente o problema, e que a resposta raramente é "reescrever tudo". O conceito de estrangulamento de legado e a diferença entre trade-off técnico e trade-off de negócio são pontos centrais do material.
