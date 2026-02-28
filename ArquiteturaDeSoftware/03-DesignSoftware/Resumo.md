# Resumo - Arquitetura vs Design de Software

## Contexto Geral

No desenvolvimento de software, dois conceitos frequentemente confundidos são Arquitetura de Software e Design de Software. Entender a diferença entre eles é essencial para construir sistemas bem organizados, sustentáveis e de sucesso.

---

## Arquitetura de Software

Segundo Bass, Clements e Kazman, Arquitetura de Software é a estrutura de um sistema composta por elementos de software, suas propriedades visíveis externamente e as relações entre eles. Em termos simples, é o "mapa geral" do sistema: define os grandes blocos que o compõem e como eles se comunicam, sem se preocupar com os detalhes internos de cada bloco.

Três mitos comuns devem ser evitados: a estrutura do sistema não é o código em si; os elementos da arquitetura não são apenas classes, mas sim serviços, camadas e bancos de dados inteiros; e as relações entre componentes vão muito além de chamadas de método, incluindo comunicação via APIs, mensageria e eventos.

A arquitetura responde à pergunta: como o sistema está organizado?

---

## Design de Software

Design de Software é o processo de definir os componentes internos de cada parte do sistema, suas interfaces, dados e comportamento, com o objetivo de implementar os requisitos conforme a arquitetura planejada. É o "zoom" dentro de cada bloco arquitetural: define classes, métodos, padrões de projeto e detalhes de implementação.

O design responde à pergunta: como o sistema será implementado?

---

## Diferença Prática

A hierarquia de processos segue a ordem: Requisitos, Arquitetura, Design e Código. Cada etapa depende da anterior.

Exemplo prático com um e-commerce: a decisão arquitetural é separar o sistema em três serviços independentes (pedidos, pagamentos e produtos) com um API Gateway na frente. O design, por sua vez, entra dentro do serviço de pedidos e define as tabelas do banco de dados, as classes como OrderServiceAPI e OrderServiceController, os casos de uso e a estrutura interna de funcionamento.

Arquitetura atua em alto nível; design atua em nível médio, detalhando a implementação.

---

## O que ambos têm em comum

Martin Fowler define tanto Arquitetura quanto Design como conjuntos de decisões difíceis de mudar no futuro. Escolher microserviços é uma decisão arquitetural. Modelar o banco de dados de certa forma é uma decisão de design. Ambas impactam custo, velocidade e manutenção do projeto ao longo do tempo. Por isso, devem ser tomadas com cuidado.

---

## O papel contínuo do Arquiteto

Definir a arquitetura é apenas o início. O arquiteto não pode "relaxar" após criar os diagramas. Ele deve comunicar a arquitetura à equipe, garantir sua correta aplicação, apoiar as decisões de design e manter a integridade arquitetural ao longo de todo o projeto. Quando isso não acontece, o resultado final pode divergir completamente do que foi planejado.

---

## O que mais cai em prova

Saber classificar itens entre Arquitetura (A) e Design (D) é fundamental. São exemplos de Arquitetura: microserviços, sistema distribuído, API Gateway, mensageria, uso de cache Redis, infraestrutura em nuvem, load balancer, autenticação OAuth e comunicação assíncrona. São exemplos de Design: classes e interfaces específicas, métodos, padrões de projeto como Adapter, algoritmos, tratamento de exceções e SOLID. Lembre-se: Arquitetura organiza o sistema em macro; Design detalha como cada parte será construída internamente.