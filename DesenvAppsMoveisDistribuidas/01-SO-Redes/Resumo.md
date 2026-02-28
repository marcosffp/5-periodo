# O Essencial de SO e Redes para Sistemas Distribuídos

## Fundamentos de SO e Redes para Sistemas Distribuídos

-- Link do mateiral: <https://cristianoneto.online/puc/damd/aula02>
> *"Um sistema distribuído é aquele em que a falha de um computador que você nem sabia que existia pode tornar o seu próprio computador inutilizável."* — Leslie Lamport

---

### Por que revisar SO e Redes?

Aplicações distribuídas não são mágica: são múltiplos Sistemas Operacionais se comunicando via rede. A estrutura fundamental de um SD é composta por **Nós** (com CPU, memória e threads), interligados por uma **Rede** (TCP/UDP, sujeita a latência e falhas), e coordenados por uma camada de **Middleware** que cria a ilusão de um sistema único para o usuário.

---

### 1. Processos vs. Threads

Cada nó em um sistema distribuído roda um SO que gerencia unidades de execução. Entender a diferença entre processo e thread é fundamental:

**Processo** é um programa em execução com espaço de endereçamento **isolado**. A vantagem é que falhas não se propagam entre processos. A desvantagem é o alto custo de troca de contexto (context switch), que leva entre 1 e 10 microssegundos, e a necessidade de IPC para comunicação.

**Thread** (fio de execução) é uma unidade leve dentro de um processo. Threads compartilham código e dados (heap), mas cada uma tem sua própria pilha (stack). O context switch é muito mais rápido (~100ns). São essenciais em SD porque servidores multithreaded conseguem atender múltiplos clientes simultaneamente. O risco é a ocorrência de **race conditions** quando múltiplas threads acessam a mesma memória sem sincronização.

---

### 2. Race Condition e Sincronização

O problema de race condition ocorre quando operações de leitura-modificação-escrita não são atômicas. O exemplo mostrado é um saldo bancário de R$100 sendo debitado por 10 threads simultaneamente: sem proteção, o saldo final pode ser R$30, R$50 ou qualquer valor imprevisível em vez de R$0.

A solução é usar um **Mutex (Lock)**, que garante exclusão mútua, permitindo que apenas uma thread execute a seção crítica por vez.

Os principais mecanismos de sincronização são:

- **Mutex:** lock binário para proteger seção crítica simples. Simples, mas limita a 1 thread por vez.
- **Semáforo:** contador que permite até N threads simultâneas. Útil para pool de conexões.
- **Monitor:** lock com variável de condição para espera condicional (padrão produtor-consumidor). Alto nível, mas com overhead maior.
- **Read-Write Lock:** permite múltiplos leitores simultâneos ou 1 único escritor. Bom para dados muito lidos, com risco de starvation.

Os problemas clássicos de sincronização são **Deadlock** (threads esperando umas pelas outras indefinidamente), **Livelock** (threads mudam de estado mas sem progresso real) e **Starvation** (uma thread nunca consegue executar).

Em SD, o desafio é ainda maior: como fazer exclusão mútua entre processos em **máquinas diferentes**, sem memória compartilhada? A solução são algoritmos distribuídos baseados em mensagens (Lamport, Ricart-Agrawala, Token Ring).

---

### 3. Comunicação entre Processos (IPC)

Existem diferentes mecanismos de IPC, dependendo se os processos estão na mesma máquina ou não:

- **Pipes:** mesma máquina, unidirecional, buffer no kernel. Usado em comandos como `ls | grep`.
- **Shared Memory:** mesma máquina, muito rápido, mas exige sincronização.
- **Message Queues:** mesma máquina, assíncrono, com mensagens estruturadas.
- **Sockets:** funcionam em **qualquer lugar** (mesma máquina ou rede), sendo a **base de toda comunicação em SD**. REST, RPC, gRPC, WebSocket — tudo usa sockets por baixo.

A abstração do socket trata a rede como um arquivo: você faz `read()` e `write()` nele.

A aula apresenta um exemplo completo de servidor e cliente TCP em Python demonstrando o fluxo: criar socket → bind na porta → escutar → aceitar cliente → receber dados → enviar resposta → fechar.

---

### 4. Redes — Foco nas Camadas 4 e 7

Em SD, o trabalho se concentra em duas camadas do modelo OSI:

**Camada 7 (Aplicação):** onde se definem os protocolos de aplicação — como estruturar requisições, rotas, autenticação e formato de dados. Exemplos: HTTP/REST, GraphQL, gRPC, Protobuf.

**Camada 4 (Transporte):** onde se decide entre **TCP** e **UDP** — decisão que impacta toda a arquitetura.

---

### 5. TCP vs. UDP — O Grande Dilema

**TCP** realiza um handshake de 3 vias (SYN → SYN-ACK → ACK) antes de qualquer troca de dados, adicionando latência de ~1.5 RTT. Em troca, garante entrega, ordem e controle de fluxo/congestionamento. Ideal para: HTTP/REST, bancos de dados, transações financeiras, e-mail e transferência de arquivos.

**UDP** não tem handshake, não garante entrega nem ordem, mas é extremamente rápido e de baixíssima latência. Ideal para: streaming de vídeo/áudio, jogos online, DNS, VoIP, IoT e alguns protocolos de consenso.

A regra é: **não existe protocolo melhor**, existe o protocolo certo para cada requisito. TCP = Confiança com custo de latência. UDP = Velocidade sem garantias.

---

### 6. Modelos de I/O — Escalabilidade

Como um servidor lida com 10 mil conexões simultâneas?

**Blocking I/O (tradicional):** cada thread faz uma requisição de I/O e fica bloqueada esperando. O modelo é 1 thread por cliente. Com 10 mil clientes seriam necessárias 10 mil threads, consumindo ~10GB de RAM e causando context switching excessivo — inviável.

**Non-Blocking I/O (assíncrono):** a thread dispara a requisição e continua trabalhando. Quando o I/O é concluído, um callback é executado. O Event Loop gerencia milhares de conexões com poucas threads. Exemplos: Node.js, Python asyncio, Java Netty. O exemplo da aula mostra 100 requisições HTTP feitas de forma assíncrona com `asyncio.gather()`, todas executadas "simultaneamente" com 1 única thread.

---

### 7. As 8 Falácias da Computação Distribuída

Erros clássicos de quem vem do desenvolvimento monolítico. Nunca assuma:

1. A rede é confiável
2. A latência é zero
3. A largura de banda é infinita
4. A rede é segura
5. A topologia não muda
6. Existe um único administrador
7. O custo de transporte é zero
8. A rede é homogênea

A aula ilustra cada falácia com casos reais:

- **AWS S3 Outage (2017):** erro humano derrubou servidores, deixando mais de 150 mil sites fora do ar por ~4 horas. Lição: use retry logic, circuit breaker e redundância multi-região.
- **Knight Capital (2012):** bug com race condition causou perda de US$ 440 milhões em 45 minutos. Lição: ordem de operações importa em SD — use timestamps lógicos (Lamport) e considere latência.
- **GitHub Outage (2018):** falha de rede causou split-brain entre datacenters. Lição: implemente detecção de partição e entenda o teorema CAP (Consistência, Disponibilidade, Tolerância a Partição).

Os **padrões de design** para lidar com essas falhas são: Retry com Exponential Backoff, Circuit Breaker, Timeout Agressivo, Bulkhead Pattern e Idempotência.

---

### Resumo — Os 6 Pilares de SD

1. **Abstração de Comunicação:** Sockets são a base de tudo. Domine socket programming.
2. **Escolha de Transporte:** TCP vs. UDP — não há melhor, há trade-offs.
3. **Modelo de I/O:** Non-Blocking I/O (Event Loop / async) permite alta escalabilidade.
4. **Realidade da Rede:** A rede vai falhar. Projete assumindo o erro. Use retry, timeout e circuit breaker.
5. **Sincronização Distribuída:** Memória compartilhada é impossível entre máquinas. Use algoritmos baseados em mensagens.
6. **Concorrência:** Threads exigem sincronização. Race conditions são reais e perigosas.

---

### Sobre a Revisão (aula02/revisao)

A página de revisão é um **quiz interativo com 15 questões de múltipla escolha** sobre os temas da aula: Threads, Sockets, TCP/UDP e as Falácias da Computação Distribuída. Cada resposta correta vale 10 pontos, com feedback imediato e explicação fundamentada. Ao final, é exibida uma classificação de desempenho.