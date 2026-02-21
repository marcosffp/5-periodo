# Resumo: Arquitetura de Software e o Papel do Arquiteto

## Contexto Geral

A diferença entre um software que funciona bem e um frustrante quase nunca está na aparência visual. Ela está na arquitetura: um conjunto de decisões tomadas antes mesmo de qualquer código ser escrito. Essa "fundação invisível" define como o sistema será organizado, como suas partes se comunicam e como ele vai evoluir ao longo do tempo.

## O que é Arquitetura de Software?

Diferentes autores definem arquitetura de formas complementares. Martin Fowler a resume como decisões difíceis de mudar no futuro. Roger Pressman fala em organização fundamental do sistema. A definição mais usada na prova é a de Bass, Clements e Kazman: arquitetura são as estruturas de um sistema, compostas por elementos de software, suas propriedades visíveis externamente e as relações entre eles.

Três conceitos-chave emergem dessa definição:

Estrutura é a organização geral do sistema — como ele é dividido. Um sistema pode ser monolítico (tudo junto em um bloco) ou baseado em microsserviços (partes independentes e especializadas). A estrutura responde à pergunta: "Como este sistema é organizado?"

Elementos são os blocos principais do sistema: componentes, serviços, APIs, bancos de dados, filas de mensagens. Não são classes de código — são unidades arquiteturais. Respondem à pergunta: "Quais são as partes principais?"

Relações definem como os elementos se comunicam ou dependem uns dos outros, por exemplo via requisições síncronas, mensagens assíncronas ou fluxo de dados. Respondem à pergunta: "Como essas partes se conectam?"

Atenção: estrutura não é código, elementos não são apenas classes, e relações não são apenas chamadas de método.

## O Papel do Arquiteto de Software

Existem três tipos de arquiteto: o corporativo (foco estratégico, olha a organização toda), o de soluções (foco em resolver um problema de negócio específico) e o de software (foco técnico, define como o sistema será construído). A aula trata principalmente deste último.

O arquiteto de software toma decisões que moldam o sistema antes que o código exista. Ele identifica atributos de qualidade prioritários — desempenho, segurança, confiabilidade, manutenibilidade — e entende que esses atributos não são resolvidos apenas no código, mas nas decisões estruturais.

Seu trabalho central é mediar trade-offs: escolhas onde ganhar em um aspecto implica abrir mão de outro. Mais velocidade de desenvolvimento pode significar menos confiabilidade futura; mais segurança pode reduzir desempenho. Não existe arquitetura perfeita nem receita universal — cada projeto exige equilíbrio diferente.

O arquiteto também comunica suas decisões para desenvolvedores, gestores e stakeholders por meio de modelos, diagramas e documentação. Arquitetura não comunicada não existe para a equipe.

Desmistificando: o arquiteto não é apenas desenhista de diagramas, não é gerente de projetos e não trabalha isolado — ele colabora com a equipe ao longo de todo o ciclo de desenvolvimento.

## Princípios Fundamentais

Os três pilares da definição de Bass: estrutura, elementos e relações. Saiba diferenciar os três tipos de arquiteto e seus focos. Entenda que arquitetura trata do "como o sistema é organizado", não do "o que ele faz". Tenha claro o conceito de trade-off e que não existe solução arquitetural universalmente superior. Por fim, lembre que decisões arquiteturais ruins impactam qualidade, escalabilidade e evolução do sistema ao longo do tempo.