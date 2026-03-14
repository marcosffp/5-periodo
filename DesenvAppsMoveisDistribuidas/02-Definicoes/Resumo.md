# Resumo — Introdução a Sistemas Distribuídos

-- Link do mateiral: <https://cristianoneto.online/puc/damd/aula03>
---

## 1. O que é um Sistema Distribuído?

Um Sistema Distribuído (SD) é um conjunto de computadores independentes que, para o usuário, aparece como se fosse uma única máquina. Cada nó tem seu próprio processador, memória e sistema operacional, e a comunicação entre eles ocorre exclusivamente por troca de mensagens pela rede (usando protocolos como TCP/UDP), sem memória compartilhada física. Exemplos clássicos: Google Search, Netflix, sistemas bancários online.

---

## 2. Por que usar Sistemas Distribuídos?

- **Economia:** usar hardware comum é mais barato que supercomputadores.
- **Escalabilidade:** cresce adicionando máquinas, não substituindo.
- **Distribuição geográfica:** reduz latência e atende leis locais (ex: LGPD).
- **Confiabilidade:** redundância elimina pontos únicos de falha.
- **Performance:** tarefas paralelas aceleram processamentos pesados (Big Data).

---

## 3. As 4 Metas de Design

**Transparência** — ocultar a complexidade da distribuição do usuário final.
**Abertura** — interfaces bem definidas que permitem interoperabilidade entre sistemas.
**Escalabilidade** — crescer sem perder desempenho.
**Confiabilidade** — continuar funcionando mesmo com falhas parciais.

---

## 4. Os 7 Tipos de Transparência (ponto crítico de prova)

| Tipo | O que esconde |
|---|---|
| Acesso | Diferenças de representação entre sistemas |
| Localização | Onde o recurso está fisicamente |
| Migração | Que um recurso foi movido (enquanto inativo) |
| Relocação | Que um recurso se move durante o uso (ex: roaming) |
| Replicação | Que existem cópias do recurso (ex: CDN) |
| Concorrência | Que múltiplos usuários acessam ao mesmo tempo |
| Falha | Que um componente falhou e se recuperou |

---

## 5. Escalabilidade: 3 Dimensões

- **Tamanho:** mais usuários e dados → solução: sharding e load balancing.
- **Geográfica:** usuários espalhados pelo mundo → solução: CDN e Edge Computing.
- **Administrativa:** múltiplas organizações envolvidas → solução: padrões abertos.

Técnicas principais: **particionamento (sharding)**, **replicação**, **caching** e **filas assíncronas** (RabbitMQ, Kafka).

---

## 6. Modelos Arquiteturais

**Cliente-Servidor:** servidor sempre ativo atende requisições dos clientes. Modelo centralizado. Exemplos: web, e-mail.

**Peer-to-Peer (P2P):** descentralizado; cada nó age como cliente e servidor ao mesmo tempo. Exemplos: BitTorrent, Bitcoin.

**Híbrido:** combina os dois modelos. Exemplo: Spotify (servidor central de metadados + P2P para streaming).

---

## 7. Paradigmas de Comunicação

- **RPC:** chamada de função remota que parece local para o desenvolvedor.
- **REST (HTTP/JSON):** baseado em recursos, sem estado (stateless). O mais usado na web.
- **Filas de mensagens:** comunicação assíncrona e desacoplada (RabbitMQ).
- **Pub/Sub:** orientado a eventos; quem publica não sabe quem consome (Redis, MQTT).
- **Streaming:** fluxo contínuo de dados (WebSockets, gRPC).

---

## 8. O que mais cai em prova

- Saber **citar e explicar os 7 tipos de transparência** com exemplos.
- Diferenciar **Cliente-Servidor vs P2P vs Híbrido**.
- Entender que SD se comunica **apenas por troca de mensagens**, sem memória compartilhada.
- Conhecer as **3 dimensões de escalabilidade** e suas soluções.
- Google Drive salvando em um lugar e acessando em outro = **Transparência de Replicação/Localização**.
- Latência aumentando com carga = **gargalo de escalabilidade**.
- Sistema tolerante a falhas e escalável = **P2P ou Híbrido**.