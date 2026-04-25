# PERT/CPM: Gerência de Projetos

## Contexto geral

PERT/CPM são técnicas usadas para planejar, organizar e controlar projetos complexos por meio de redes de tarefas. O CPM (Critical Path Method) surgiu em 1957 e o PERT (Program Evaluation and Review Technique) em 1958. Apesar de desenvolvidos separadamente, são complementares e hoje usados em conjunto. A motivação veio de projetos militares de grande escala, como o submarino Polaris da Marinha americana.

## Componentes básicos da rede

A rede é formada por dois elementos: atividades (tarefas que consomem tempo e recursos, como "instalar equipamentos") e eventos (marcos que indicam início ou fim de uma atividade, sem consumir tempo). Há dois métodos de representação: o Método Americano, onde atividades são setas e eventos são círculos; e o Método Francês, onde atividades são blocos e setas indicam a ordem de execução.

## Atividade Fantasma

Criada artificialmente com duração zero, serve para representar uma dependência entre tarefas sem criar uma relação real inexistente. É usada quando duas tarefas compartilham um evento, mas não dependem exatamente das mesmas predecessoras.

## Tipos de dependência entre tarefas

Existem quatro tipos: Início-Início (II), quando duas tarefas devem começar juntas; Início-Término (IT), rara na prática; Término-Início (TI), o mais comum — uma tarefa só começa quando outra termina; e Término-Término (TT), quando duas tarefas devem terminar juntas.

## Conceitos de datas e folgas

PDI (Primeira Data de Início): data mais cedo em que uma tarefa pode começar. PDT (Primeira Data de Término): quando termina se iniciada na PDI. UDI (Última Data de Início): data limite para iniciar sem atrasar o projeto. UDT (Última Data de Término): quando termina se iniciada na UDI. Folga Livre (FL): atraso possível sem prejudicar o início das sucessoras. Folga Total (FT): atraso possível sem prejudicar a conclusão do projeto.

## Etapas para construir a rede

Etapa 0: montar o diagrama de setas. Etapa 1: calcular o Tempo de Término mais Cedo (TC) dos eventos, do início ao fim. Etapa 2: calcular o Tempo de Término mais Tarde (TT), do fim para o início. Etapa 3: calcular PDI, PDT e Folga Livre. Etapa 4: calcular UDI, UDT e Folga Total.

**Fórmulas principais:** PDI = TCi + 1 | PDT = PDI + D − 1 | FL = TCj − PDT | UDT = TT | UDI = UDT − D + 1 | FT = UDT − PDT

## Caminho Crítico

É o caminho do início ao fim da rede com maior duração total. As atividades nesse caminho têm Folga Total igual a zero — qualquer atraso nelas atrasa o projeto inteiro. Pode haver mais de um caminho crítico. Identificado também pela menor diferença TT − TC em cada evento.

## O que mais cai em prova

Definição e diferença entre PERT e CPM; conceito de atividade fantasma e quando usá-la; os quatro tipos de dependência (especialmente TI); as seis datas (PDI, PDT, UDI, UDT, FL, FT) e suas fórmulas; identificação do caminho crítico (FT = 0 e maior duração); erros comuns na construção da rede (dependências falsas, atividades fictícias desnecessárias, numeração errada de eventos).
