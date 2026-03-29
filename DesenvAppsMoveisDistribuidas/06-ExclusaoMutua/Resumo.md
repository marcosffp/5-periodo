# RESUMO PARA PROVA — Exclusão Mútua Distribuída e Eleição de Líder

## CONTEXTO GERAL

Em sistemas centralizados, mutex e semáforos controlam o acesso exclusivo a recursos usando memória compartilhada. Em sistemas distribuídos (SD), cada processo roda em máquinas diferentes — não há memória compartilhada. Toda coordenação é feita por mensagens. O objetivo é garantir que apenas um processo por vez acesse a seção crítica (CS), sem deadlock e sem starvation.

## AS TRES FAMILIAS

Permissão: o processo pede autorização a todos antes de entrar na CS. Custo: 2(N-1) mensagens. Uma falha qualquer trava tudo.

Token: um token circula pelo anel lógico; só quem o possui entra na CS. Custo: 1 a N-1 mensagens. Risco: token perdido em falha.

Coordenador central: um líder eleito decide quem entra. Simples, mas ponto único de falha.

## RICART-AGRAWALA (Permissão)

Cada processo tem um relógio lógico (timestamp). Para entrar na CS, envia REQUEST com seu timestamp a todos. Ao receber REQUEST, responde REPLY imediatamente se não quer a CS ou se o outro tem timestamp menor (prioridade maior). Caso contrário, enfileira e responde ao sair. Entra na CS ao receber REPLY de todos. Empate: menor PID vence. Custo: 2(N-1) mensagens.

## TOKEN RING

Token passa continuamente pelo anel. Quem tem o token e quer a CS entra, executa e repassa. Quem não quer apenas repassa. Detectar e regenerar token perdido é o principal desafio.

## ELEIÇÃO DE LIDER

- Quando o coordenador falha, é preciso eleger outro. Dois nós acreditando ser líderes ao mesmo tempo causam split brain — inconsistência grave.

- Bully (1982): quem detecta a falha envia ELECTION a todos com ID maior. Quem responder repete o processo acima. Sem resposta, declara-se líder e notifica todos. Vence sempre o maior ID ativo. Custo: O(N²) no pior caso. Exige sistema síncrono.

- Ring Election (1979): processos em anel. Cada nó encaminha o ID recebido se for maior que o seu, descarta se for menor. Ao receber o próprio ID, declara-se líder. Custo médio: O(N log N).

- Raft (2014): cada nó é Follower, Candidate ou Leader. Se o Follower não recebe heartbeat dentro de um timeout aleatório (150-300 ms), vira Candidate, incrementa o term (mandato), vota em si mesmo e solicita votos. Maioria absoluta (>N/2) elege o Leader. Timers aleatórios evitam que vários nós se candidatem juntos (split vote). Cada nó vota uma vez por term. Em empate, novo term começa. Custo: O(N). Tolera N/2-1 falhas. Usado em etcd (Kubernetes), CockroachDB e Consul.

## O QUE MAIS CAI EM PROVA

No Ricart-Agrawala: menor timestamp entra primeiro; empate pelo menor PID. Contar mensagens: 2(N-1) por acesso. No Bully: O(N²) pior caso; maior ID vence. No Raft: maioria garante unicidade do líder; timers aleatórios evitam split vote; é o mais tolerante a falhas e dominante na prática.