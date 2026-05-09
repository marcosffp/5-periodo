# RESUMO — Arquitetura Cliente-Servidor e SOA

## Contexto Geral

A aula aborda dois estilos arquiteturais fundamentais: Cliente-Servidor e Orientado a Serviços (SOA). Ambos são amplamente usados na construção de sistemas modernos e costumam aparecer combinados na prática.

## Arquitetura Cliente-Servidor

Nesse modelo, o sistema é dividido em dois papéis: o cliente, que faz requisições, e o servidor, que responde a essas requisições. É a base de praticamente toda aplicação web. Mesmo arquiteturas mais avançadas, como microserviços, utilizam esse modelo como fundamento.

Existem três variações principais. No modelo 2-tier, o cliente acessa o banco de dados diretamente. No 3-tier, há um servidor de aplicação intermediário entre o cliente e o banco. No N-tier, surgem múltiplas camadas, como API, serviços de negócio e banco de dados, o que permite maior escalabilidade.

As vantagens incluem centralização dos dados, facilidade de manutenção e maior controle de segurança. As desvantagens são o ponto único de falha no servidor, possível gargalo de desempenho e limitação de escalabilidade sem mecanismos adicionais como balanceamento de carga e replicação.

## Arquitetura Orientada a Serviços (SOA)

SOA organiza o sistema como um conjunto de serviços independentes, fracamente acoplados e acessíveis por interfaces bem definidas. Seus princípios incluem reusabilidade, baixo acoplamento, autonomia, ausência de estado, abstração e interoperabilidade.

O ESB (Enterprise Service Bus) é o componente central do SOA tradicional. Funciona como um intermediário que roteia mensagens, transforma formatos e aplica regras de segurança entre os serviços, sem tomar decisões de negócio. O problema do ESB clássico é que, quando acumula responsabilidades demais, vira um "God ESB": gargalo, ponto único de falha e difícil de evoluir. Na prática atual, suas funções foram absorvidas por API Gateways e ferramentas de mensageria como Kafka, RabbitMQ e Amazon SQS.

A coordenação entre serviços pode ocorrer de duas formas. Na orquestração, um componente central (orquestrador) controla o fluxo do processo, com controle explícito e mais simples de entender. Na coreografia, os serviços reagem a eventos de forma autônoma, sem um controlador central, com maior autonomia mas mais difícil de rastrear.

Para lidar com transações em sistemas distribuídos, onde o ACID tradicional não se aplica, usa-se o padrão SAGA: cada etapa do processo tem uma ação compensatória em caso de falha, garantindo consistência eventual.

## Diferenças Principais

Cliente-Servidor centraliza lógica em um servidor único. SOA distribui as responsabilidades em serviços independentes que se comunicam por interfaces padronizadas. SOA é mais indicado em ambientes complexos, com múltiplos sistemas legados e forte necessidade de integração, como bancos, hospitais e sistemas governamentais.

## O que mais cai em prova

Saber definir Cliente-Servidor e SOA, conhecer as variações em camadas (2-tier, 3-tier, N-tier), entender o papel do ESB e seus problemas, diferenciar orquestração de coreografia, e compreender o SAGA Pattern para consistência em sistemas distribuídos.
