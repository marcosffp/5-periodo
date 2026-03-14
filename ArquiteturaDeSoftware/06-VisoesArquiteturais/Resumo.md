# Resumo — Visões Arquiteturais (Aula 8)

Um sistema de software complexo precisa ser compreendido por pessoas muito diferentes: desenvolvedores, gestores, equipe de infraestrutura, analistas de negócio. O problema é que cada um desses grupos tem interesses distintos, chamados de *concerns*. Por isso, um único diagrama tentando mostrar tudo ao mesmo tempo acaba confundindo todos: uns veem informação de mais, outros de menos. A solução é usar visões arquiteturais, ou seja, representações diferentes do mesmo sistema, cada uma focada em um público específico.

O modelo mais conhecido para organizar essas visões é o 4+1, criado por Philippe Kruchten. Ele propõe cinco perspectivas simultâneas do mesmo sistema.

A Visão Lógica mostra a estrutura interna do software, ou seja, como seus componentes e módulos estão organizados e se relacionam. Não mostra funcionalidades diretamente, mas a estrutura que as sustenta. É representada por diagramas de pacotes e de componentes, e interessa a usuários, analistas, arquitetos e desenvolvedores.

A Visão de Processo trata do comportamento dinâmico do sistema: como as partes se comunicam em tempo de execução, como lidam com concorrência, desempenho e escalabilidade. É representada por diagramas de sequência, atividades e máquina de estados. Interessa a arquitetos e desenvolvedores.

A Visão de Desenvolvimento mostra a organização estática do código: como as classes, interfaces e pacotes estão estruturados no projeto. O diagrama de classes é o principal representante. É voltada para a equipe de desenvolvimento.

A Visão Física descreve como o software é distribuído no hardware: quais servidores, onde cada serviço roda, como a infraestrutura está configurada. Usa o diagrama de implantação e é essencial para equipes de DevOps e operações.

A Visão de Cenários, o "+1", é o ponto de integração de todas as outras. Ela usa diagramas de casos de uso para mostrar como o sistema se comporta do ponto de vista do usuário, validando se as demais visões fazem sentido juntas. É compreensível por todos os envolvidos.

Para representar essas visões na prática, utiliza-se a UML (Unified Modeling Language), uma linguagem de modelagem padronizada que oferece dois grandes grupos de diagramas: estruturais, como classes, componentes e pacotes, e comportamentais, como sequência, casos de uso e atividades.

Para a prova, o mais importante é: saber o que é cada visão, para qual público ela se destina, qual diagrama UML a representa e entender que a Visão Lógica mostra estrutura, não funcionalidades. O modelo 4+1 é o framework central da aula.
