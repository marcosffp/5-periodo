# Transparência em Sistemas Distribuídos — Código na Prática
**PUC Minas — ICEI — Aplicações Móveis e Distribuídas — Aula Prática**
**Professor:** Cristiano de Macedo Neto

*Exemplos em Python · Aula de Laboratório*

**🎯 Objetivos:**
- Identificar cada um dos 7 tipos de transparência da ISO/RM-ODP em código real
- Compreender o antes e depois de cada implementação
- Reconhecer quando não aplicar transparência (anti-patterns)
- Conectar teoria e prática usando Python como linguagem veículo

---

## O que é Transparência em SD?

*"Transparência é a propriedade de um sistema distribuído de ocultar do usuário e do programador de aplicações a separação entre seus componentes."*
— ISO/IEC 10746 · Reference Model of Open Distributed Processing (RM-ODP)

A norma ISO/RM-ODP define 7 tipos de transparência:

| Tipo | O que o sistema esconde? | Quem se beneficia? |
|---|---|---|
| Acesso | Como o recurso é acessado (local vs. remoto) | Desenvolvedor |
| Localização | Onde o recurso está fisicamente | Usuário / Dev |
| Migração | Que o recurso foi movido para outro nó | Usuário |
| Relocação | Que o recurso se move enquanto está em uso | Usuário |
| Replicação | Que existem múltiplas cópias do recurso | Usuário / Dev |
| Concorrência | Que múltiplos clientes compartilham o recurso | Usuário |
| Falha | Que um componente falhou e foi substituído | Usuário |

**Metodologia desta aula:** Para cada tipo: (1) definição → (2) código SEM transparência (o problema) → (3) código COM transparência (a solução) → (4) análise do trade-off.

---

## 1. Transparência de Acesso

Oculta as diferenças de representação e mecanismo de acesso entre recursos locais e remotos. O código do cliente deve ser idêntico independentemente de onde o recurso está.

**❌ SEM Transparência:** o cliente precisa saber onde está o dado e usar APIs completamente diferentes (arquivo local, HTTP, S3), com acoplamento forte à origem.

**✅ COM Transparência:** contrato único `ConfigRepository` (ABC) com implementações `LocalConfig`, `RemoteConfig` e `S3Config`. O cliente usa sempre `repo.get("database")` — não sabe onde está o dado.

**Padrão de projeto utilizado: Repository + Strategy.** A abstração é o contrato. As implementações concretas encapsulam os detalhes. O cliente não muda quando a origem muda — apenas a injeção de dependência muda.

---

## 2. Transparência de Localização

Oculta onde o recurso está fisicamente. O cliente usa um nome lógico; o sistema resolve a localização real sem expô-la.

**Analogia — DNS:** você acessa `www.pucminas.br` sem saber o IP. O DNS resolve em tempo real.

**❌ SEM Transparência:** IPs e portas hardcoded no código (`http://192.168.10.42:8080`). Se o servidor mudar de IP → reimplanta tudo.

**✅ COM Transparência — Service Discovery + nomes lógicos:** classe `ServiceLocator` consulta o Consul e devolve instâncias saudáveis em tempo real. O cliente usa apenas `locator.resolve("user-service")` — nunca IPs.

Fluxo: **Cliente usa "user-service"** → **Service Registry (Consul/Eureka/k8s DNS)** → **192.168.10.42:8080 (IP resolvido em runtime)**

---

## 3. Transparência de Migração

Oculta que um recurso foi movido de um nó para outro. Clientes continuam acessando pelo mesmo endereço lógico, sem qualquer alteração no código.

**Cenário Real — Live Migration de VMs:** o AWS EC2 move instâncias entre hosts físicos para manutenção. A aplicação não percebe.

**❌ SEM Transparência:** sessão armazenada com localização física (`host: "10.0.1.5"`). Se o serviço migrar de host, a referência fica inválida e a conexão falha.

**✅ COM Transparência:** sessão armazenada no Redis (`session:{user_id}`) com TTL. A instância do app pode ser migrada a qualquer momento — a sessão persiste no Redis, independente do host.

**Princípio chave: Stateless Services.** Serviços que não guardam estado localmente podem ser migrados, replicados e terminados sem perda de dados. Todo o estado vai para armazenamento externo (Redis, banco, S3).

---

## 4. Transparência de Relocação

Oculta que um recurso está sendo movido **enquanto ainda está em uso**. É mais sutil que a Migração: a sessão é ativa, a transferência é ao vivo.

### Migração vs. Relocação

| Aspecto | Migração | Relocação |
|---|---|---|
| Quando ocorre | Recurso inativo | Recurso em uso |
| Interrupção | Pode ter pausa | Deve ser zero |
| Exemplo físico | Mover arquivo | Roaming celular |

**Casos Reais:** Mobile IP (mover entre torres WiFi sem perder chamada), Kubernetes (rebalancear pods durante pico sem derrubar conexões), CDN prefetch (antecipar mudança de PoP).

**Implementação:** classe `TransparentWSClient` com estados `CONNECTED`, `MIGRATING` e `RECONNECTING`. Durante a relocação, mensagens são bufferizadas silenciosamente e drenadas ao reconectar. O código de negócio nunca sabe que a relocação ocorreu.

---

## 5. Transparência de Replicação

Oculta que múltiplas cópias do recurso existem. O cliente acessa "o banco de dados" — não sabe que são 3 réplicas em regiões distintas.

**Por que replicar?** Alta disponibilidade + escalabilidade de leitura. Se uma réplica cair, as outras continuam. Se a carga crescer, adiciona-se réplica.

**✅ COM Transparência — `ReplicaPool`:** encapsula master e réplicas. O cliente usa apenas `.query()`, nunca sabe quantas réplicas existem. Leituras vão para réplicas (round-robin), escritas vão para o master. Se uma réplica falha, é removida do pool e o fallback vai para o master — tudo transparente.

```python
# Leitura: vai para uma réplica (transparente)
users = db.query("SELECT * FROM users WHERE active=true")

# Escrita: vai para o master (transparente)
db.query("INSERT INTO orders VALUES (...)", write=True)
```

---

## 6. Transparência de Concorrência

Oculta que múltiplos clientes compartilham e modificam o mesmo recurso simultaneamente. Cada cliente deve ter a ilusão de acesso exclusivo.

**❌ SEM Transparência:** variável global `saldo` com operação não-atômica (leitura → operação → escrita). Race condition: dois clientes leem o mesmo saldo e sobrescrevem um ao outro. Saldo deveria ser R$500, mas fica R$700 ou R$800.

**✅ COM Transparência — lock distribuído via Redis:** `distributed_lock(resource)` usa `SET NX EX` (atômico). A seção crítica é protegida, o lock é liberado automaticamente com `finally`. Exclusão mútua garantida entre processos distribuídos.

**Por que não usar `threading.Lock()` em SD?** O lock local funciona apenas dentro do mesmo processo. Em SD, processos estão em máquinas diferentes sem memória compartilhada. O lock precisa ser implementado sobre comunicação de rede (Redis, ZooKeeper, etcd).

---

## 7. Transparência de Falha

Considerada a **mais difícil** de implementar: ocultar que um componente falhou e foi substituído ou recuperado.

**Implementação — Circuit Breaker + Retry com backoff exponencial:**

Estados do Circuit Breaker:
- **CLOSED:** normal — requisições passam
- **OPEN:** falhas detectadas — rejeita requisições (falha rápida)
- **HALF_OPEN:** testando se o serviço voltou

Lógica: ao atingir `failure_threshold` falhas, o CB abre. Após `recovery_timeout` segundos, vai para HALF_OPEN e testa. Enquanto aberto, retorna fallback do cache.

**Decorator de retry com backoff exponencial:** tenta até 3 vezes com delays 0.5s, 1s, 2s antes de desistir.

Fluxo: **Cliente** → **Circuit Breaker (CLOSED/OPEN/HALF)** → **Serviço (pode falhar)** → **Cache/Fallback (resposta degradada)**

---

## 🎯 Checkpoint: Classificação de Transparências

**Q1.** Spotify move stream de servidor sobrecarregado para outro sem pausar a música. Que transparência é essa?
✅ **C — Transparência de Relocação** — recurso em uso sendo movido sem interrupção.

**Q2.** Google Docs: João e Maria editam o mesmo parágrafo simultaneamente sem perceber conflitos internos. Qual transparência?
✅ **B — Transparência de Concorrência** — múltiplos clientes compartilham o mesmo recurso.

**💬 Questão Reflexiva — Pix:** A transparência de Falha seria o ideal, mas a mensagem "Pix em análise" indica que não foi totalmente alcançada. O sistema optou por informar ao usuário em vez de fingir sucesso — legítimo quando a semântica da operação exige confirmação explícita. Exemplo dos **limites da transparência** em transações financeiras.

---

## ⚠️ Os Limites da Transparência

Transparência total é **impossível e às vezes prejudicial**. Waldo et al. (1994) demonstraram que ocultar completamente a distribuição gera bugs silenciosos difíceis de diagnosticar.

**❌ Anti-Pattern — Transparência Excessiva:** função `get_user()` que parece local, mas pode levar 800ms, lançar `TimeoutError`, retornar dado desatualizado ou falhar silenciosamente.

**✅ Padrão Correto — Expor Custos Explicitamente:** `async/await` deixa explícito que é operação cara; nome `remote` sinaliza chamada de rede; timeout configurável; falha explícita (retorna `None` ou cache), nunca silenciosa.

### Quando aplicar (ou não) cada transparência

| Tipo | Aplicar quando... | Evitar quando... |
|---|---|---|
| Acesso | Múltiplos backends precisam ser trocados sem afetar cliente | Backends têm semânticas muito diferentes (SQL vs. objeto) |
| Localização | Serviços podem ser implantados em diferentes hosts/regiões | Latência geográfica é crítica e o cliente precisa escolher a réplica mais próxima |
| Replicação | Alta disponibilidade e escalabilidade de leitura são necessárias | Consistência forte é mandatória (ex: saldo bancário real-time) |
| Falha | Operações idempotentes que toleram resposta degradada | Transações financeiras onde confirmação explícita é obrigatória |

---

## Síntese: Os 7 Tipos em uma Visão Só

| Tipo | O Que Esconde | Padrão/Técnica | Exemplo Real |
|---|---|---|---|
| Acesso | Como o recurso é acessado | Repository, Adapter, Strategy | NFS: arquivo local e remoto com mesma API POSIX |
| Localização | Onde o recurso está | Service Discovery (Consul, Eureka, k8s DNS) | URL `https://api.empresa.com` sem revelar datacenter |
| Migração | Que o recurso foi movido | Stateless Services + External State (Redis) | VM live migration no AWS EC2 durante manutenção |
| Relocação | Que o recurso se move em uso | Buffering, Mobile IP, WebSocket reconnect | Mover entre redes WiFi sem cair a chamada VoIP |
| Replicação | Que existem múltiplas cópias | Read Replica Pool, CDN, cache em camadas | Netflix CDN: vídeo replicado em centenas de PoPs |
| Concorrência | Que múltiplos clientes compartilham | Distributed Lock (Redis/ZooKeeper), MVCC | Google Docs: edição colaborativa em tempo real |
| Falha | Que um componente falhou | Circuit Breaker, Retry + Backoff, Fallback Cache | Netflix Hystrix: degradação graciosa de serviços |

**Próxima Aula:** Comunicação em SD — RPC em Detalhes · Message-Oriented Middleware · Serialização · gRPC na Prática

---

**📚 Bibliografia:**
- COULOURIS, G. et al. *Distributed Systems: Concepts and Design*. 5. ed. Cap. 2.
- TANENBAUM, A. S.; VAN STEEN, M. *Distributed Systems*. 3. ed. Cap. 1.
- KLEPPMANN, M. *Designing Data-Intensive Applications*. Caps. 5 e 8.
- WALDO, J. et al. *A Note on Distributed Computing*. Sun Microsystems, 1994.
- ISO/IEC 10746-1. *Reference Model of Open Distributed Processing*. 1998.