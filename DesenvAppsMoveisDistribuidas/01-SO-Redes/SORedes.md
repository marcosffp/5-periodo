# Desenvolvimento de Aplicações Móveis e Distribuídas — Revisão Técnica
**PUC Minas — ICEI — Aula de Revisão**
**Professor:** Cristiano de Macedo Neto

*O Essencial de SO e Redes para SD*

*"Um sistema distribuído é aquele em que a falha de um computador que você nem sabia que existia pode tornar o seu próprio computador inutilizável."* — Leslie Lamport

---

## Por que Revisar SO e Redes Agora?

Aplicações distribuídas não são mágicas. São múltiplos Sistemas Operacionais comunicando-se via Rede. O objetivo é criar **transparência**: o usuário vê um app único, mas nós (engenheiros) gerenciamos a complexidade subjacente.

---

## Manifesto: IA como Ferramenta de Maestria

*"Não ignoramos a IA Generativa. A forma como você a utiliza definirá se será um engenheiro robusto ou frágil. Quem não sabe escrever código sem IA, é refém dela. Você deve comandar a máquina, não o contrário."*

Três modos de uso correto da IA:
- **Modo 1 — O Espelho:** você faz → IA faz → comparação crítica. Aprenda com as diferenças.
- **Modo 2 — O Gerador:** IA cria o desafio, você resolve. Ferramenta de treino técnico.
- **Modo 3 — O Auditor:** IA gera código com bugs intencionais, você identifica e corrige. Desenvolve pensamento crítico.

---

## 1. Sistema Operacional: Unidades de Execução

### Processos vs Threads

**Processos:** programa em execução com espaço de endereçamento isolado.
- ✅ Falha não propaga entre processos
- ❌ Alto custo: context switch pesado (~1–10μs)
- ❌ Comunicação via IPC (pipes, sockets)

**Threads:** unidade leve dentro do processo, compartilham memória (variáveis globais).
- ✅ Context switch leve (~100ns)
- ✅ Compartilham Code e Data
- ❌ Risco: race conditions

**Essencial em SD:** servidores multithreaded atendem múltiplos clientes simultaneamente.

### Demo: Race Condition em Ação

**O problema:** operações de leitura-modificação-escrita não são atômicas. Entre o momento que Thread A lê o saldo e escreve o novo valor, Thread B pode ler o valor antigo. O resultado final é não-determinístico. Ex: 10 threads debitando R$10 de saldo R$100 podem resultar em R$30, R$50 ou R$70 em vez de R$0.

**A solução:** usar `threading.Lock()` (Mutex) com `with lock:` garante exclusão mútua — apenas 1 thread executa a seção crítica por vez. Resultado: sempre R$0.

**Checkpoint 1:** por que servidores web modernos usam arquitetura multithreaded ou event-driven?
✅ **B — Para evitar bloqueio de I/O e atender múltiplos clientes simultaneamente.**

---

## Sincronização: Mecanismos e Trade-offs

| Mecanismo | Como Funciona | Quando Usar | Trade-off |
|---|---|---|---|
| Mutex | Lock binário (trancado/destrancado) | Proteger seção crítica simples | ✅ Simples ❌ Apenas 1 thread por vez |
| Semáforo | Contador (permite N threads) | Limitar concorrência (ex: pool de conexões) | ✅ Flexível ❌ Mais complexo |
| Monitor | Lock + variável de condição | Espera condicional (produtor-consumidor) | ✅ Alto nível ❌ Overhead maior |
| Read-Write Lock | Múltiplos leitores OU 1 escritor | Dados lidos com frequência | ✅ Performance ❌ Risco de starvation |

**Problemas Clássicos:** Deadlock (threads esperando umas pelas outras indefinidamente), Livelock (threads mudando estado sem progresso), Starvation (thread nunca consegue executar).

**O Desafio em SD:** não existe memória compartilhada física entre máquinas! A solução são algoritmos distribuídos baseados em mensagens (Lamport, Ricart-Agrawala, Token Ring).

---

## Comunicação entre Processos (IPC)

| Mecanismo | Escopo | Características | Caso de Uso |
|---|---|---|---|
| Pipes | Mesma máquina | Unidirecional, buffer no kernel | Comunicação pai-filho (ex: `ls \| grep`) |
| Shared Memory | Mesma máquina | Rápido, requer sincronização | Grande volume de dados entre processos |
| Message Queues | Mesma máquina | Assíncrono, mensagens estruturadas | Desacoplamento temporal |
| Sockets | Qualquer lugar | Abstração de rede, TCP/UDP | **Base para SD** (RPC, REST, gRPC) |

**Insight Fundamental:** em SD, tudo se resume a Sockets no final das contas. REST sobre HTTP? Socket. RPC? Socket. WebSocket? Socket. gRPC? Socket sobre HTTP/2. Sockets tratam a rede como um arquivo: você faz `read()` e `write()`.

### Socket Programming — Hello World

Fluxo do **servidor TCP:** criar socket → bind na porta → listen → accept cliente → recv dados → send resposta → close.

Fluxo do **cliente TCP:** criar socket → connect ao servidor → send dados → recv resposta → close.

**Exercício:** modifique o servidor para atender múltiplos clientes usando threads. Dica: `threading.Thread(target=handle_client, args=(client,))`.

---

## 2. Redes: Foco nas Camadas 4 e 7

Em SD, vivemos principalmente em duas camadas do modelo OSI:

- **Camada 7 — Aplicação:** aqui definimos protocolos de aplicação — como estruturar requisições, rotas, autenticação, formato de dados. Ex: REST/HTTP, GraphQL, Protobuf, Thrift.
- **Camada 4 — Transporte:** decisão crítica entre TCP e UDP. Define confiabilidade, latência e controle de fluxo. Esta escolha impacta toda a arquitetura do sistema.

### O Grande Dilema: TCP ou UDP?

**TCP (Transmission Control Protocol):**
- ✅ Confiável: garante entrega e ordem
- ✅ Controle de fluxo e congestionamento
- ❌ Maior overhead: handshake (3-way) + ACKs, ~1.5 RTT para estabelecer conexão
- **Casos de uso:** HTTP/REST, bancos de dados, transações financeiras, e-mail, transferência de arquivos

**UDP (User Datagram Protocol):**
- ✅ Rápido: zero overhead de conexão, baixa latência
- ❌ Não confiável: pode perder pacotes, sem ordem garantida, sem controle de fluxo
- **Casos de uso:** streaming (vídeo/áudio), jogos online, DNS, VoIP, IoT sensores

**Trade-off Fundamental:** não existe "melhor" protocolo. TCP = confiança ao custo de latência. UDP = velocidade ao custo de garantias. A escolha depende dos requisitos da aplicação.

**Checkpoint 2:** sistema de videoconferência (voz + vídeo ao vivo). Qual protocolo para o stream de vídeo?
✅ **B — UDP**, porque baixa latência é mais importante que perder alguns frames.

---

## Modelos de I/O: Escalabilidade

**Como seu servidor lida com 10 mil conexões simultâneas?**

**Blocking I/O (Tradicional):** thread faz requisição de I/O e fica parada esperando. Modelo: 1 thread por cliente. ❌ Com 10k clientes = 10k threads = ~10GB RAM + context switching excessivo.

**Non-Blocking I/O (Assíncrono):** thread dispara requisição e continua trabalhando. Callback executado quando I/O completa. Modelo: poucos threads + Event Loop. ✅ Alta escalabilidade com poucos recursos. Ex: Node.js, Python asyncio, Java Netty.

Com `asyncio.gather()` em Python, é possível disparar 100 requisições HTTP simultaneamente com apenas 1 thread — o event loop gerencia tudo.

---

## As 8 Falácias da Computação Distribuída

Erros clássicos de quem vem do desenvolvimento monolítico. Nunca assuma:

1. A rede é confiável
2. A latência é zero
3. A largura de banda é infinita
4. A rede é segura
5. A topologia não muda
6. Existe um único administrador
7. O custo de transporte é zero
8. A rede é homogênea

### Casos Reais de Violações

**AWS S3 Outage (2017) — "A rede é confiável":** erro humano derrubou servidores na região us-east-1. Mais de 150 mil sites fora do ar por ~4 horas. Lição: implemente retry logic, circuit breaker e multi-region redundancy.

**Knight Capital (2012) — "A latência é zero":** bug em sistema de trading causou race conditions. Perda de US$ 440 milhões em 45 minutos. Lição: ordem de operações importa; use timestamps lógicos (Lamport) e considere latência de rede.

**GitHub Outage (2018) — "A topologia não muda":** falha de rede causou split-brain entre datacenters. Sistema ficou 24h degradado com dados inconsistentes. Lição: implemente detecção de partição e escolha entre CAP (Consistência, Disponibilidade, Tolerância a Partição).

### Padrões de Design para Lidar com Falácias
- **Retry com Exponential Backoff:** tente novamente com atraso crescente
- **Circuit Breaker:** pare de chamar serviço que está falhando
- **Timeout Agressivo:** nunca espere indefinidamente
- **Bulkhead Pattern:** isole falhas (1 serviço caindo não derruba tudo)
- **Idempotência:** operações podem ser repetidas com segurança

**Checkpoint 3:** sistema de pagamentos com timeout no débito. O que fazer?
✅ **B — Implementar operação idempotente + retry com exponential backoff + verificar se já foi processado.**

---

## Resumo: Os 6 Pilares de SD

1. **Abstração de Comunicação:** sockets são a porta de entrada. Toda comunicação distribuída usa sockets. Domine socket programming.
2. **Escolha de Transporte:** TCP (confiança + latência) vs UDP (velocidade − garantias). Não há "melhor", há trade-offs.
3. **Modelo de I/O:** para escalar, use Non-Blocking I/O (Event Loop, async/await). 1 thread pode gerenciar milhares de conexões.
4. **Realidade da Rede:** a rede vai falhar. Latência existe. Projete assumindo o erro. Implemente retry, timeout, circuit breaker.
5. **Sincronização:** memória compartilhada em SD é impossível. Use algoritmos baseados em mensagens. Exclusão mútua distribuída é complexa.
6. **Concorrência:** threads permitem paralelismo, mas exigem sincronização (Mutex, Semáforos). Race conditions são reais e perigosas.

**Próxima Aula:** Introdução a Sistemas Distribuídos — Definição Formal · Metas de Design · Transparência · Escalabilidade · Modelos Arquiteturais.

---

**📚 Bibliografia Principal:**
- TANENBAUM, A.S.; VAN STEEN, M. *Distributed Systems: Principles and Paradigms*. 3. ed. Pearson, 2017.
- COULOURIS, G. et al. *Distributed Systems: Concepts and Design*. 5. ed. Addison-Wesley, 2011.
- KUROSE, J.F.; ROSS, K.W. *Computer Networking: A Top-Down Approach*. 7. ed. Pearson, 2016.
- DEUTSCH, L.P. *The Eight Fallacies of Distributed Computing*. Sun Microsystems, 1994.

**Material Complementar:** RFC 793 (TCP), RFC 768 (UDP), Wireshark, Kleppmann — *Designing Data-Intensive Applications*, Cap. 8.