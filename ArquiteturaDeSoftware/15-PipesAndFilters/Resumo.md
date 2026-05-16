# Estilos Arquiteturais: Pipes-and-Filters, Publish/Subscribe e Space-Based

Esta aula apresenta três estilos arquiteturais usados em projetos de software modernos, cada um com foco e objetivos distintos.

## Pipes-and-Filters

Neste estilo, os dados passam por uma sequência de componentes independentes chamados filtros (filters), conectados entre si por canais chamados tubulações (pipes). Cada filtro recebe um dado, processa e passa o resultado adiante. O fluxo começa em uma bomba (pump), fonte dos dados, e termina em um destino (sink). É possível ter pipelines paralelos, onde fluxos distintos correm ao mesmo tempo sem conflito.

Exemplos reais: pipeline de CI/CD (build, backup, deploy), o Logstash na pilha ELK e o pipeline de middlewares do ASP.NET Core.

Vantagens: modularidade, reutilização de filtros, fácil manutenção e processamento paralelo. Desvantagens: cada etapa extra aumenta o overhead e a latência, o gerenciamento de estado global é difícil, e nem todo problema se encaixa em fluxo sequencial.

## Publish/Subscribe (Pub/Sub)

Não é um estilo arquitetural clássico por si só, mas um padrão de comunicação muito usado em arquiteturas orientadas a eventos. Funciona assim: quem publica (publisher) envia mensagens para um canal (topic/broker), e quem assina (subscriber) recebe essas mensagens sem comunicação direta entre as partes. Isso gera baixo acoplamento e comunicação assíncrona. A desvantagem é que rastrear o fluxo e depurar erros se torna mais difícil. Este padrão será aprofundado no módulo de EDA (Event Driven Architecture).

## Space-Based (SBA)

Criado pela Microsoft em 1997, este estilo elimina o banco de dados centralizado como gargalo. A ideia central é distribuir processamento e estado entre múltiplos nós (Processing Units - PUs), mantendo os dados na memória RAM de vários servidores ao mesmo tempo. Isso garante altíssima escalabilidade, baixa latência e alta disponibilidade.

Os principais componentes são: Data Grid (memória RAM distribuída entre os nós, o coração do "espaço de dados"), Messaging Grid (comunicação assíncrona entre PUs), Processing Grid (distribuição da carga de processamento) e Deployment Manager (monitoramento e escala automática).

O SBA segue o modelo AP do teorema CAP: prioriza disponibilidade e tolerância a partições, abrindo mão da consistência imediata. É ideal para e-commerce de alta escala, jogos online, sistemas financeiros e streaming. Não é recomendado para sistemas pequenos, equipes reduzidas ou quando consistência forte (ACID) é obrigatória.

## O que mais cai em prova

Saber identificar os componentes de cada estilo (filtro, pipe, pump, sink; publisher, broker, subscriber; PU, Data Grid, Messaging Grid), seus trade-offs principais (manutenibilidade vs. performance no Pipes-and-Filters; disponibilidade vs. consistência no Space-Based) e quando usar ou evitar cada um. A diferença entre SBA, EDA e Microsserviços também é relevante: SBA distribui estado e processamento, EDA organiza a comunicação por eventos, e Microsserviços separa responsabilidades de negócio.
