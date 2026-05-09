# Estilo Arquitetural em Camadas (Layered Architecture)

## O que é

Organiza o sistema em camadas com responsabilidades bem definidas, onde cada camada fornece serviços para a camada acima e consome serviços da camada abaixo. A ideia central é dividir para organizar, reduzindo a complexidade geral do sistema.

---

## Ideia principal

Cada camada tem uma responsabilidade específica, fica isolada das demais e só se comunica com as camadas diretamente adjacentes. Isso evita o "spaghetti code" — código desorganizado, frágil e difícil de mudar.

---

## Camadas típicas

| Camada | Responsabilidade |
| --- | --- |
| Apresentação (Presentation) | Interface com o usuário |
| Negócio (Business Logic) | Regras e lógica da aplicação |
| Acesso a Dados (Data Access) | Comunicação com o banco |
| Banco de Dados | Armazenamento dos dados |

---

## Cross-Cutting

Quando uma funcionalidade precisa servir a mais de uma camada ao mesmo tempo (ex: logging, autenticação, tratamento de erros), ela não pode "pular" camadas. A solução é o conceito de **cross-cutting concern**: um componente transversal que atende todas as camadas de forma controlada, sem quebrar a hierarquia.

---

## Relação com MVC

O padrão MVC é uma aplicação direta do estilo em camadas:

- **View** → Camada de Apresentação
- **Controller** → Camada de Negócio
- **Model** → Camada de Acesso a Dados

---

## Exemplos reais do estilo em camadas

- Modelo OSI (7 camadas de rede)
- Arquitetura TCP/IP (4 camadas)
- Sistemas Operacionais (Aplicações → Kernel → Hardware)

---

## Onde é usado

Sistemas corporativos como ERPs, CRMs e sistemas acadêmicos. Praticamente todos os frameworks web modernos são baseados nesse estilo:

| Tecnologia | Framework |
| --- | --- |
| .NET | ASP.NET MVC / Core |
| Java | Spring MVC |
| Python | Django |
| PHP | Laravel |
| Ruby | Ruby on Rails |
| Node.js | Express |

---

## Vantagens

- Organização clara do código
- Facilidade de manutenção
- Reutilização de camadas
- Separação de responsabilidades
- Facilidade de testes

## Desvantagens

- Mais camadas = mais complexidade
- Overhead de comunicação entre camadas
- Excesso de abstração
- Performance pode ser impactada

---

## Trade-offs principais

**Organização vs. Complexidade:** quanto maior a separação de responsabilidades, maior o número de componentes e abstrações.

**Manutenibilidade vs. Performance:** a separação facilita a manutenção, mas cada requisição atravessa todas as camadas, adicionando overhead de processamento.

---

## Conclusão

A arquitetura em camadas é um dos estilos mais usados em sistemas corporativos por promover organização e manutenibilidade. Porém, como todo estilo, envolve trade-offs. Cabe ao arquiteto avaliar os RAS e os atributos de qualidade desejados para decidir se esse estilo é o mais adequado ao contexto.

**Para a prova, foque em:** definição do estilo, as camadas típicas, o conceito de cross-cutting, a relação com MVC, vantagens, desvantagens e os dois trade-offs principais (organização vs. complexidade / manutenibilidade vs. performance).
