# APIs RESTful, OpenAPI, Docker e Kubernetes — Resumo para Prova

## REST — O que é e de onde vem

REST (Representational State Transfer) é um **estilo arquitetural**, não um protocolo nem um padrão certificado. Foi definido por Roy Fielding em sua dissertação de doutorado em 2000 na UC Irvine. A ideia central: usar o próprio HTTP como linguagem universal de integração, aproveitando o que a Web já fazia naturalmente.

Antes do REST, a integração entre sistemas era feita via **SOAP/WSDL** (anos 1990–2000): envelopes XML enormes, schemas complexos, stubs gerados — e qualquer mudança no contrato quebrava todos os clientes. O custo de escalar integrações crescia quadraticamente com o número de sistemas.

**REST não é certificado.** Não há organização que ateste que uma API é "REST". É um conjunto de constraints que, quando satisfeitos, produzem escalabilidade, visibilidade e modificabilidade.

---

## Os 6 Constraints de Fielding

Estes são os pilares do REST. Dominar cada um é essencial para qualquer questão conceitual.

### 1. Client–Server
Separação de responsabilidades: o cliente gerencia a interface; o servidor gerencia dados e lógica de negócio. Cada lado pode evoluir de forma independente — o app mobile não precisa saber nada sobre o banco de dados do servidor.

### 2. Stateless
Cada requisição deve conter **todas** as informações necessárias para ser processada. O servidor **não mantém estado de sessão** entre requisições. Consequência direta: qualquer instância do servidor pode atender qualquer requisição, tornando o escalonamento horizontal trivial. É por isso que JWT é preferido a sessões no servidor.

### 3. Cache
Respostas devem declarar explicitamente se são cacheáveis (`Cache-Control`, `ETag`, `Last-Modified`). Clientes ou intermediários que cacheiam reduzem latência e carga sobre o servidor.

### 4. Uniform Interface (o mais importante)
Quatro sub-constraints:
- **(a) Identificação de recursos por URI** — o recurso é identificado por sua URI, não por sua localização física.
- **(b) Manipulação via representações** — o cliente manipula o recurso através de representações (JSON, XML), não diretamente.
- **(c) Mensagens auto-descritivas** — cada mensagem contém informação suficiente para ser processada (Content-Type, status code). Retornar `200 OK` com `{"status": "error"}` viola esse constraint.
- **(d) HATEOAS** — hipermídia como motor do estado da aplicação (ver seção abaixo).

### 5. Layered System
O cliente não sabe se está falando com o servidor final ou com um intermediário (API Gateway, proxy reverso, CDN, load balancer). Cada camada vê apenas a adjacente.

### 6. Code-on-Demand (opcional)
O único constraint opcional. O servidor pode enviar código executável ao cliente (ex: JavaScript). Raro em APIs de sistemas distribuídos.

**Para a prova:** Stateless e Uniform Interface são os mais cobrados. Saiba que a maioria das APIs chamadas de "REST" na prática viola pelo menos Stateless ou Uniform Interface.

---

## Design de Recursos, Verbos HTTP e Versionamento

### URIs identificam recursos, não ações

**Errado:** `POST /cancelOrder/123`
**Correto:** `POST /orders/123/cancellations`

A ação é o verbo HTTP; o recurso é o substantivo na URI. Nunca verbo na URI.

### Tabela de verbos e URIs

| Ação | URI | Verbo | Resposta |
|---|---|---|---|
| Listar | `/v1/orders` | GET | 200 + array |
| Criar | `/v1/orders` | POST | 201 + Location |
| Buscar | `/v1/orders/{id}` | GET | 200 ou 404 |
| Substituir completo | `/v1/orders/{id}` | PUT | 200 ou 204 |
| Atualizar parcial | `/v1/orders/{id}` | PATCH | 200 ou 204 |
| Remover | `/v1/orders/{id}` | DELETE | 204 |
| Sub-recurso | `/v1/orders/{id}/items` | GET | 200 + array |

### Versionamento

**URI Versioning** (mais adotado): `/v1/orders` → `/v2/orders`. Fácil de testar no browser. Usado por Stripe, GitHub, Twitter. Tecnicamente viola REST (a versão é detalhe de implementação, não parte do recurso).

**Header Versioning**: `Accept: application/vnd.app.v2+json`. Mais fiel ao REST. Difícil de testar e cachear. Usado pelo GitHub API v3 e Microsoft Graph.

Regra prática: versione desde a primeira release pública. Nunca quebre contratos sem período de transição documentado.

---

## Idempotência e Segurança dos Verbos HTTP

Dois conceitos fundamentais para entender como clientes e intermediários tratam requisições:

**Método Seguro (Safe):** não altera o estado do servidor. O cliente pode chamar quantas vezes quiser sem efeitos colaterais.

**Método Idempotente:** chamar N vezes produz o mesmo estado que chamar uma vez. A propriedade é sobre o **estado resultante**, não sobre a resposta HTTP.

| Método | Seguro? | Idempotente? | Consequência de repetir |
|---|---|---|---|
| GET | Sim | Sim | Idêntico — seguro para retry automático |
| HEAD | Sim | Sim | Idêntico |
| OPTIONS | Sim | Sim | Idêntico |
| PUT | Não | **Sim** | Estado final idêntico — seguro para retry |
| DELETE | Não | **Sim** | 1º retorna 204; 2º retorna 404 — **estado idêntico** |
| PATCH | Não | Depende | Depende do payload |
| POST | Não | **Não** | Cria novo recurso a cada chamada — **nunca retry automático** |

**Por que isso importa:** se um cliente perde a resposta de um POST (timeout), não pode repetir automaticamente — criaria pedido duplicado. Solução: `Idempotency-Key` header (adotado pelo Stripe) — o servidor detecta e ignora réplicas.

**DELETE é idempotente mesmo que o segundo retorne 404:** o estado resultante é o mesmo (recurso não existe). Isso é frequentemente cobrado em prova.

---

## Códigos de Status HTTP — Semântica Correta

Anti-padrão comum: retornar `200 OK` com `{"status": "error"}`. Isso viola o Uniform Interface, impede que API Gateways tratem erros automaticamente e engana clientes.

### 2xx — Sucesso

| Código | Quando usar |
|---|---|
| 200 OK | GET, PUT, PATCH bem-sucedidos com corpo |
| 201 Created | POST que criou recurso. **Incluir header `Location`** |
| 204 No Content | DELETE ou PUT/PATCH sem corpo de resposta |
| 202 Accepted | Operação aceita mas processada de forma assíncrona |

### 4xx — Erro do Cliente

| Código | Quando usar |
|---|---|
| 400 Bad Request | Payload malformado ou parâmetros inválidos |
| 401 Unauthorized | Sem autenticação ou token inválido |
| 403 Forbidden | Autenticado mas sem permissão |
| 404 Not Found | Recurso não existe (também usado para esconder 403) |
| 409 Conflict | Conflito de estado (ex: e-mail já cadastrado) |
| 422 Unprocessable | Payload bem formado mas semanticamente inválido |
| 429 Too Many Requests | Rate limiting. Incluir `Retry-After` |

### 5xx — Erro do Servidor

| Código | Quando usar |
|---|---|
| 500 Internal Server Error | Falha genérica. **Nunca expor stack trace** |
| 503 Service Unavailable | Servidor sobrecarregado ou em manutenção |

---

## HATEOAS — O Constraint Mais Ignorado

HATEOAS (Hypermedia As The Engine Of Application State) é o quarto sub-constraint do Uniform Interface. A ideia: a API guia o cliente pelas ações disponíveis **através de links na própria resposta**, assim como uma página web guia o usuário por hiperlinks.

**Sem HATEOAS:** o cliente precisa conhecer antecipadamente todas as URIs e quando cada ação está disponível. Qualquer mudança no servidor exige atualização coordenada do cliente — acoplamento.

**Com HATEOAS:** o cliente começa de um ponto de entrada e descobre as ações disponíveis nas respostas. URIs podem mudar sem quebrar clientes que seguem links.

```json
// GET /v1/orders/42 — pedido aguardando pagamento
{
  "id": "42", "status": "pending_payment",
  "_links": {
    "self":   { "href": "/v1/orders/42", "method": "GET" },
    "pay":    { "href": "/v1/orders/42/payments", "method": "POST" },
    "cancel": { "href": "/v1/orders/42/cancellations", "method": "POST" }
  }
}

// GET /v1/orders/42 — após pagamento confirmado
{
  "status": "paid",
  "_links": {
    "self":  { "href": "/v1/orders/42" },
    "track": { "href": "/v1/orders/42/tracking" }
    // "cancel" e "pay" não aparecem — ação indisponível nesse estado
  }
}
```

O servidor controla quais transições de estado são válidas. Se a regra de negócio mudar (ex: janela de cancelamento cai de 24h para 12h), apenas o servidor muda — o cliente se adapta automaticamente.

**HAL** usa `_links`; **JSON:API** usa `links` e `relationships` — dois formatos populares para expressar HATEOAS.

---

## Autenticação — Bearer Token e OAuth2

### Bearer Token (JWT)

O mecanismo mais comum. O cliente envia o token no header de cada requisição:

```
Authorization: Bearer eyJhbGciOiJSUzI1NiJ9...
```

O JWT tem 3 partes separadas por ponto:
- **Header:** algoritmo e tipo (`{"alg":"RS256","typ":"JWT"}`)
- **Payload:** claims (`{"sub":"user:u42","roles":["admin"],"exp":1750000000}`)
- **Signature:** assinatura com chave privada do servidor

**JWT não é criptografia — é assinatura.** O payload é apenas codificado em Base64. Nunca armazene dados sensíveis (senhas, cartões) no JWT. Use JWE se precisar de confidencialidade.

O servidor valida o token **sem consultar banco de dados** (respeita Stateless).

### OAuth 2.0 — Dois Fluxos Principais

**Client Credentials (M2M — Machine to Machine):**
1. Serviço A envia `client_id` + `client_secret` para o Authorization Server
2. Recebe `access_token` (JWT) com escopo definido
3. Usa o token como Bearer em chamadas ao Serviço B

**Authorization Code (usuário final):**
1. App redireciona usuário ao Authorization Server ("Login com Google")
2. Usuário autentica e consente → Authorization Server retorna um `code`
3. App troca o `code` por `access_token` + `refresh_token` (server-to-server)
4. Usa `access_token` como Bearer. Quando expira, usa `refresh_token` para renovar sem novo login

---

## OpenAPI + Swagger UI

OpenAPI Specification (OAS) é um formato YAML/JSON que descreve completamente uma API REST. Evolução do Swagger Specification, hoje mantido pela Linux Foundation. Versão atual: 3.1, alinhada com JSON Schema 2020-12.

**Por que documentação como código:** sem contrato formal, mudanças de tipo em campos só aparecem como bug em produção. Com OpenAPI versionado no repositório, a mudança é visível em PR antes de ir ao ar.

### Composição de schemas

```yaml
Order:
  allOf:           # herança — inclui tudo de BaseOrder
    - $ref: '#/components/schemas/BaseOrder'
    - properties:
        status: { type: string, enum: [pending, paid, shipped] }

PaymentMethod:
  oneOf:           # polimorfismo — é UM dos tipos abaixo
    - $ref: '#/components/schemas/CreditCard'
    - $ref: '#/components/schemas/Pix'
    discriminator:
      propertyName: type
```

O **Swagger UI** renderiza o documento como interface navegável e executável no browser — cada endpoint pode ser testado com autenticação OAuth2 integrada sem precisar de curl ou Postman.

**FastAPI (Python), Spring Boot com Springdoc (Java) e NestJS (Node.js)** geram o documento OpenAPI automaticamente a partir de anotações no código e sobem o Swagger UI em `/docs`.

---

## Docker — Contêineres e Boas Práticas

### Contêiner ≠ Máquina Virtual

VMs virtualizam hardware completo com kernel próprio. Contêineres compartilham o kernel do host, isolando apenas processo e filesystem. Inicializam em milissegundos, consomem megabytes. Um contêiner empacota código, runtime e dependências em uma unidade portável que roda identicamente em qualquer máquina com Docker Engine.

### Dockerfile com Multi-Stage Build

```dockerfile
# Estágio 1: builder — compila e testa
FROM node:22-alpine AS builder
WORKDIR /app
COPY package*.json .
RUN npm ci
COPY . .
RUN npm run build && npm test

# Estágio 2: produção — só o necessário
FROM node:22-alpine AS production
WORKDIR /app
COPY package*.json .
RUN npm ci --omit=dev
COPY --from=builder /app/dist ./dist
USER node           # nunca rodar como root
EXPOSE 3000
CMD ["node", "dist/main.js"]
```

**Benefício do multi-stage:** a imagem final de produção contém **apenas o artefato compilado e dependências de runtime** — sem compilador, ferramentas de build, código-fonte ou dependências de dev. Isso reduz a superfície de ataque e o tamanho da imagem (pode cair de centenas para dezenas de MB), acelerando pulls no deploy.

### Docker Compose

Orquestra múltiplos serviços em uma única máquina (dev/QA). Destaque para o `depends_on` com `condition: service_healthy`:

```yaml
services:
  api:
    build: { context: ./api, target: production }
    ports: ["3000:3000"]
    depends_on:
      db: { condition: service_healthy }   # espera o banco estar pronto

  db:
    image: postgres:16-alpine
    healthcheck:
      test: ["CMD", "pg_isready"]
      interval: 5s
```

O `healthcheck` no Compose evita que a API suba antes do banco estar pronto — erro comum sem ele.

---

## Kubernetes — Do Pod ao Auto-scaling

### Por que K8s em produção (e não Compose)

Compose não reinicia contêineres com backoff inteligente, não escala com base em métricas, não distribui carga entre servidores, não faz rolling updates sem downtime.

### Pod, Deployment e Service

**Pod:** a menor unidade do K8s. Um ou mais contêineres que compartilham rede e storage.

**Deployment:** declara quantas réplicas do Pod devem rodar e gerencia rolling updates.

**Service:** expõe os Pods como um endpoint estável. O tráfego é distribuído entre os Pods saudáveis:
```
Cliente → Service :80 → Pod 1 | Pod 2 | Pod 3
```

### Liveness vs. Readiness Probes

Essa distinção é a mais cobrada em provas sobre Kubernetes.

**Liveness Probe:** "o contêiner está travado?" — se falhar N vezes, K8s **reinicia** o Pod.

**Readiness Probe:** "o contêiner está pronto para receber tráfego?" — se falhar, K8s **remove o Pod do Service** (para de enviar tráfego) sem matar o contêiner.

```yaml
livenessProbe:
  httpGet: { path: /health/live, port: 3000 }
  initialDelaySeconds: 15    # aguarda inicialização
  failureThreshold: 3        # 3 falhas = restart

readinessProbe:
  httpGet: { path: /health/ready, port: 3000 }
  periodSeconds: 5
  failureThreshold: 2        # 2 falhas = sai do load balancer
```

**Cenário de prova:** o banco de dados ficou temporariamente indisponível. A readinessProbe falha; a livenessProbe passa. O K8s remove o Pod do balanceamento **mas não reinicia** — correto, pois reiniciar não resolveria (o banco continuaria indisponível). Quando o banco volta, a readinessProbe passa e o Pod retorna ao pool.

O endpoint `/health/ready` deve verificar dependências externas (banco, cache). O `/health/live` verifica só se o processo está vivo.

### HPA — HorizontalPodAutoscaler

Escala automaticamente o número de Pods com base em métricas:

```yaml
kind: HorizontalPodAutoscaler
spec:
  scaleTargetRef:
    kind: Deployment
    name: orders-api
  minReplicas: 2
  maxReplicas: 12
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 60
```

A cada 15 segundos o HPA consulta a utilização média de CPU. Se superar 60%, calcula:

```
réplicas = ceil(atual × utilização / alvo)
```

Com 3 Pods a 90% de CPU: `ceil(3 × 90/60) = 5` réplicas.

**Pré-requisito crítico:** o HPA só funciona se o Deployment tiver `resources.requests.cpu` definido. Sem isso o K8s não tem base para calcular o percentual.

---

## Docker vs. Compose vs. Kubernetes

| Aspecto | Docker | Compose | Kubernetes |
|---|---|---|---|
| Escopo | 1 contêiner | 1 máquina | N máquinas |
| Auto-restart | Básico | Básico | Com backoff |
| Auto-scaling | Não | Não | HPA |
| Rolling update | Não | Parcial | Nativo |
| Liveness/Readiness | Não | Healthcheck | Probes nativas |
| Segredos | Env vars | `.env` files | Secrets API |
| Uso ideal | Dev local | Dev/QA | Produção |

---

## Mapa conceitual da aula (para revisar antes da prova)

```
API RESTful
  6 constraints · idempotência · status codes · HATEOAS · OAuth2
          ↓ contrato descrito por
OpenAPI + Swagger UI
  schemas · allOf/oneOf · securitySchemes · Idempotency-Key
          ↓ empacotada em
Docker + Compose
  multi-stage build · healthcheck · ambiente reproduzível
          ↓ operada por
Kubernetes
  Pod · Deployment · Service · Probes · HPA
```

---

## O que gravar para a prova

- **REST é estilo arquitetural, não padrão certificado.** Seis constraints; Stateless e Uniform Interface são os mais violados na prática.
- **Idempotência é contrato:** GET e DELETE podem ser repetidos com segurança. POST nunca pode. `Idempotency-Key` protege operações críticas não idempotentes.
- **HATEOAS:** o servidor controla transições de estado via links na resposta. Cliente não precisa conhecer regras de negócio — se mudarem, só o servidor muda.
- **JWT:** assinatura, não criptografia. Payload em Base64. Nunca dados sensíveis.
- **Multi-stage build Docker:** imagem de produção não carrega ferramentas de build. Menor superfície de ataque.
- **Liveness ≠ Readiness:** Liveness = reinicia o Pod. Readiness = tira do load balancer sem matar. Banco indisponível → Readiness falha, Liveness passa, Pod sai do LB mas continua vivo.
- **HPA requer `resources.requests.cpu`** definido no Deployment para funcionar.
