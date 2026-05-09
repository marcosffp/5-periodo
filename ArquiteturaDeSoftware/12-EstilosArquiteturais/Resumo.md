# Estilos Arquiteturais: Conceitos e Fundamentos

## O que é Arquitetura de Software

É a organização estrutural de um sistema: define seus elementos, como se relacionam e quais propriedades expõem. Importante: estrutura não é código, elementos não são só classes, e relacionamentos vão além de chamadas de método.

---

## O que é um Estilo Arquitetural

É um modelo de organização que define os tipos de componentes usados, as formas de comunicação entre eles, as restrições de uso e a organização geral do sistema. Em outras palavras: o "padrão" que guia como o sistema será montado.

> **Estilo não é tecnologia.** Microserviços não é Docker; monolito não é .NET. O estilo define a estrutura — a tecnologia é só o meio de implementá-la.
> **Estilo não é arquitetura completa.** A arquitetura envolve requisitos, decisões, trade-offs e pode combinar mais de um estilo.

---

## O que um Estilo Arquitetural define

- Tipos de componentes
- Formas de comunicação
- Restrições de uso
- Organização do sistema

---

## Os 10 Estilos Arquiteturais

### 1. Monolítica

Todo o sistema é desenvolvido e implantado como uma única unidade. Use quando o projeto é pequeno ou simples e velocidade de entrega é prioridade. Evite quando o sistema precisar escalar partes independentes.

### 2. Microsserviços

O sistema é dividido em serviços pequenos, independentes e especializados que se comunicam via rede. Use quando há necessidade de alta escalabilidade e times autônomos. Evite quando a equipe é pequena ou o sistema ainda está sendo definido.

### 3. Model-View-Controller (MVC)

Separa a aplicação em três camadas: Model (dados), View (interface) e Controller (lógica de controle). Use em aplicações web e desktop que precisam separar interface de negócio. Evite quando a lógica de negócio for muito complexa e exigir uma separação mais robusta.

### 4. Orientada a Serviços (SOA)

Organiza o sistema em serviços reutilizáveis que se comunicam por um barramento central (ESB). Use em ambientes corporativos com múltiplos sistemas legados que precisam se integrar. Evite quando a simplicidade for prioridade, pois o barramento central pode virar gargalo.

### 5. Camadas

Organiza o sistema em camadas hierárquicas (ex: apresentação, negócio, dados), onde cada camada só se comunica com a imediatamente abaixo. Use em sistemas corporativos tradicionais com separação clara de responsabilidades. Evite quando o desempenho for crítico, pois cada requisição atravessa todas as camadas.

### 6. Cliente-Servidor

Divide o sistema em clientes (que fazem requisições) e servidores (que respondem). Use em praticamente qualquer sistema web ou aplicação com interface separada do back-end. Evite quando não houver necessidade de comunicação em rede ou quando a latência for inaceitável.

Processa dados passando-os por uma sequência de filtros conectados por tubos (pipes), onde cada filtro transforma a entrada e repassa a saída. Use em pipelines de processamento de dados, compiladores e ETL. Evite quando o processamento exigir compartilhamento de estado entre etapas.

### 8. Orientada a Eventos

Os componentes se comunicam produzindo e consumindo eventos de forma assíncrona, sem depender diretamente uns dos outros. Use quando desacoplamento e reatividade forem essenciais, como em sistemas de notificação e IoT. Evite quando a rastreabilidade e o debugging do fluxo forem críticos, pois eventos assíncronos dificultam o acompanhamento.

### 9. Baseada em Espaço

Distribui o processamento e o armazenamento em um espaço de memória compartilhada (tuple space), sem ponto central. Use quando alta escalabilidade e tolerância a falhas forem necessárias, como em sistemas de alta carga. Evite quando consistência forte dos dados for um requisito.

### 10. Publicador-Inscrito (Pub/Sub)

Os componentes publicam mensagens em tópicos e outros componentes se inscrevem para recebê-las, sem comunicação direta entre si. Use em sistemas de notificação, streaming de eventos e integrações assíncronas. Evite quando a ordem garantida das mensagens ou a resposta síncrona forem obrigatórias.

---

## Como escolher um estilo: o processo RAS

A escolha parte dos **RAS (Requisitos Arquiteturalmente Significativos)**: requisitos funcionais e não funcionais que impactam diretamente a qualidade do sistema.

O processo é:

1. Levantar requisitos com os stakeholders
2. Refinar os requisitos
3. Classificar em RF (funcionais) e RNF (não funcionais)
4. Registrar os concerns arquiteturais
5. Filtrar os que impactam atributos de qualidade
6. Chegar à lista de RAS → escolher o estilo adequado

| Estilo | Benefício principal |
| --- | --- |
| Monolito | Simplicidade |
| Monolito Modular | Simplicidade + flexibilidade |
| Microsserviços | Escalabilidade |
| Orientada a Eventos | Desacoplamento |

---

## Conclusão

Não existe estilo melhor — existe o mais adequado ao contexto. Cabe ao arquiteto conhecer o catálogo, entender o problema e escolher ou combinar estilos que atendam às necessidades do sistema e dos stakeholders.

**Para a prova, foque em:** definição de estilo arquitetural, diferença entre estilo/tecnologia/arquitetura completa, os 10 estilos e seus benefícios, conceito de RAS e o processo de escolha do estilo adequado ao contexto.
