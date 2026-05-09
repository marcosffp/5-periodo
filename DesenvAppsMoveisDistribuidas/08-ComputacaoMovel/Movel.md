# Computação Móvel: Fundamentos, Mobile IP e Middleware

**PUC Minas — ICEI — Aplicações Móveis e Distribuídas**  
**Aula 09 · Engenharia de Software · 2026/1**  
**Professor:** Cristiano de Macedo Neto

---

## Sumário

- [I. Fundamentos](#i-fundamentos) — Slides 2–4
- [II. Mobile IP](#ii-mobile-ip) — Slides 5–7
- [III. Limitações](#iii-limitações) — Slides 8–9
- [IV. Middleware](#iv-middleware) — Slides 10–13

---

> *"The most profound technologies are those that disappear. They weave themselves into the fabric of everyday life until they are indistinguishable from it."*  
> — Mark Weiser · *Scientific American*, vol. 265, n. 3, pp. 94–104, 1991

---

## 🎯 Objetivos da Aula

- Entender as características e desafios intrínsecos da mobilidade
- Compreender como dispositivos mantêm conectividade ao se mover entre redes (Mobile IP)
- Reconhecer as limitações que nenhuma geração tecnológica elimina completamente
- Comparar frameworks de middleware para tomar decisões de projeto fundamentadas

---

## I. Fundamentos

### O que é Computação Móvel?

Computação móvel é a capacidade de **acessar recursos computacionais e de rede enquanto em movimento**, independente de localização física. Não é apenas "software que roda em celular" — é um paradigma onde a mobilidade **muda a natureza do problema de distribuição**.

### Três Pilares Conceituais

| Pilar | Descrição | Implicação de Projeto |
|---|---|---|
| 📱 **Portabilidade** | O dispositivo acompanha o usuário fisicamente | Recursos limitados são inevitáveis — bateria, CPU, tela |
| 📡 **Conectividade sem fio** | O link de comunicação é inerentemente variável | Sinal varia com distância, obstáculos, interferências e densidade de usuários |
| 🗺️ **Mobilidade** | O usuário troca de ponto de acesso ao se mover | O endereço IP precisa ser mantido ou atualizado de forma transparente |

### Diferença Crucial: Móvel ≠ Sem Fio

- **📶 Sem fio, mas NÃO móvel:** Um servidor Wi-Fi em ambiente fixo de escritório. Usa link wireless, mas nunca muda de rede — não há desafio de mobilidade.
- **🚶 Móvel e sem fio (o caso real):** Smartphone em movimento entre células de operadora. Muda de antena, de endereço IP, de qualidade de sinal — **todos os desafios de mobilidade aparecem aqui**.
- **🏗️ Implicação para o projeto:** Sistemas móveis precisam ser **projetados para a variabilidade**, não para a estabilidade. A exceção (queda de conexão) é a regra.

> **Referências:** WEISER, M. *Scientific American*, v. 265, n. 3, pp. 94–104, 1991. | SATYANARAYANAN, M. *ACM PODC '96*, pp. 1–7. DOI: 10.1145/248052.248053. | FORMAN, G. H.; ZAHORJAN, J. *IEEE Computer*, v. 27, n. 4, pp. 38–47, 1994. DOI: 10.1109/2.274999.

---

### Evolução Histórica: de 1G ao 5G

| Geração | Característica | Início | Destaques |
|---|---|---|---|
| **1G** | Analógico | NMT-450: set/1981 · AMPS: out/1983 | Voz apenas · ~2,4 kbps |
| **2G** | Digital (GSM) | Finlândia, dez/1991 | SMS + dados lentos · GPRS: ~56 kbps · EDGE: ~384 kbps |
| **3G** | Banda larga móvel | UMTS: out/2001 (JP) | Internet no celular · HSPA: ~7,2 Mbps · Era dos smartphones |
| **4G LTE** | IP nativo | TeliaSonera: dez/2009 | Video streaming · LTE-A: ~100 Mbps · App economy |
| **5G NR** | Convergência | KT/SK/LGU+: abr/2019 | eMBB · URLLC · mMTC · ≤1 ms latência · 20 Gbps teórico · IoT massivo |

### Dispositivos — Marcos Históricos

| Ano | Marco |
|---|---|
| **1992** | PDA — John Sculley (Apple) cunha o termo. Newton MessagePad lançado em ago/1993. PalmPilot em mar/1996. |
| **1994** | IBM Simon Personal Communicator — primeiro smartphone comercial: touchscreen, e-mail, fax. Lançado em 16/ago/1994. |
| **2007** | Apple iPhone (29/jun/2007) — redefiniu o paradigma: interface multi-touch, App Store (2008), plataforma de computação. |
| **2008** | HTC Dream — primeiro Android (22/out/2008). Inaugurou o ecossistema Android. Hoje: >70% do mercado global de smartphones. |

### Por que isso importa para o desenvolvedor?

- **⚡ 5G não resolve tudo:** O 5G oferece 20 Gbps teórico em condições ideais. Na prática, coberturas são heterogêneas, handoffs ainda ocorrem e dispositivos restritos operam em mMTC a kilobits por segundo. A **variabilidade permanece**.
- **🏗️ Evolução das plataformas:** De C nativo (feature phones) → Java ME → iOS SDK / Android → React Native / Flutter → Web APIs. Cada geração eleva o nível de abstração, mas os desafios da mobilidade persistem abaixo da superfície.
- **📊 Escala atual (2025):** ≈ 6,9 bilhões de assinantes móveis únicos no mundo (GSMA 2025). Tráfego de dados móveis cresce ~30% ao ano.

---

### As Quatro Restrições Intrínsecas de Satyanarayanan

Em 1996, Mahadev Satyanarayanan (CMU) publicou um resultado fundamental: as principais limitações da computação móvel **não são acidentes tecnológicos — são consequências necessárias da mobilidade em si**. Tecnologias futuras as atenuarão, mas não as eliminarão.

#### 1. Pobreza de Recursos
Dispositivos móveis sempre serão **menos potentes** do que equivalentes fixos contemporâneos (CPU, RAM, armazenamento, bateria). A miniaturização tem custo físico irredutível.  
→ *Para o dev: design offline-first, caching agressivo, offloading seletivo de computação para a nuvem.*

#### 2. Vulnerabilidade de Segurança
Dispositivos móveis são **fisicamente roubáveis** e operam em meios de transmissão compartilhados (Wi-Fi público). O canal wireless é inerentemente mais vulnerável a eavesdropping.  
→ *Para o dev: TLS obrigatório, autenticação forte, dados sensíveis nunca em plaintext local.*

#### 3. Conectividade Altamente Variável
Largura de banda, latência e disponibilidade **mudam continuamente**. Um cliente pode passar de 4G (50 Mbps) a Wi-Fi (500 Mbps) a túnel/elevador (0 kbps) em segundos.  
→ *Para o dev: adaptive bitrate streaming, retries com backoff exponencial, UX degradado gracioso.*

#### 4. Energia Finita de Bateria
Toda operação tem custo energético: rádio transmitindo consome **10–100× mais** que a CPU em idle. Polling frequente drena bateria; push é mais eficiente.  
→ *Para o dev: prefira WebSocket/SSE a polling; MQTT com QoS 0 para telemetria não-crítica; batch de operações em background.*

### 💡 Application-Aware Adaptation

A melhor estratégia não é esconder as limitações do aplicativo — é uma **parceria entre SO e aplicação** onde o sistema informa as condições atuais e a aplicação ajusta sua fidelidade de resposta. O sistema de arquivos **Coda** (CMU) implementou esse conceito: modo online, weakly-connected e desconectado, cada um com comportamento diferente. Esse princípio permanece a base de padrões modernos como offline-first.

> **Referências:** SATYANARAYANAN, M. *Proc. 15th ACM Symposium on Principles of Distributed Computing (PODC '96)*, pp. 1–7, 1996. DOI: 10.1145/248052.248053. | KISTLER, J. J.; SATYANARAYANAN, M. *ACM TOCS*, v. 10, n. 1, pp. 3–25, 1992.

---

## II. Mobile IP

### O Problema Fundamental do Endereçamento Móvel

No IP convencional, o **endereço IP identifica tanto o dispositivo quanto sua localização** na rede. Um pacote destinado a `192.168.1.5` vai necessariamente para a sub-rede que contém esse prefixo. Isso funciona perfeitamente para hosts fixos — e **quebra completamente quando o host se move**.

**🔴 O problema: IP assume localização fixa**  
Se João conecta ao Wi-Fi da PUC e obtém o IP `200.130.10.45`, então vai ao metrô e conecta ao 4G da operadora, recebe `179.52.33.77`.

- Sessões TCP são quebradas (o IP mudou)
- O servidor não sabe que João "mudou de endereço"
- Pacotes em trânsito para o IP antigo se perdem
- Toda autenticação baseada em IP falha

**🟢 A solução: separar identidade de localização**  
Mobile IP (RFC 5944, 2010) resolve isso com dois conceitos:

- **Home Address:** IP permanente — identidade de João na rede. Nunca muda.
- **Care-of Address (CoA):** IP temporário na rede visitada — localização atual.

Um agente na rede doméstica redireciona os pacotes transparentemente. Para o servidor (Correspondent Node), João sempre tem o mesmo IP.

### Os Três Agentes do Mobile IP

| Agente | Papel |
|---|---|
| 🏠 **Home Agent (HA)** | Roteador na **rede doméstica** de João. Mantém o binding (Home Address ↔ CoA). Recebe pacotes e os encapsula (tunneling) para o CoA atual. |
| 🌍 **Foreign Agent (FA)** | Roteador na **rede visitada**. Fornece o CoA a João. Desencapsula os pacotes recebidos pelo túnel e os entrega. *(Eliminado no Mobile IPv6.)* |
| 💬 **Correspondent Node (CN)** | O servidor que se comunica com João. **Não precisa saber nada** do Mobile IP — envia pacotes ao Home Address normalmente. O HA cuida do redirecionamento. |

> **Referências:** PERKINS, C. E. (Ed.) *IP Mobility Support for IPv4, Revised. RFC 5944*, IETF, novembro 2010. | RFC 2002 (out 1996) → RFC 3344 (ago 2002) → RFC 5944 (nov 2010). Para IPv6: RFC 6275 (jul 2011). | PERKINS, C. E. *Mobile IP: Design Principles and Practices*. Addison-Wesley, 1997. ISBN 978-0201634693.

---

### Fluxo de Comunicação e Roteamento Triangular

Fluxo com João (Mobile Node) fora de sua rede doméstica:

```
Correspondent Node (CN)
        │
        ① dst: 200.130.10.45
        ▼
   Home Agent (HA)       ← Rede Doméstica 200.130.10.0/24
        │
        ② Túnel IP-in-IP (dst: CoA 179.52.33.x)
        ▼
  Foreign Agent (FA)     ← Rede Visitada 179.52.33.0/24
        │
        ③ Desencapsula e entrega a João
        ▼
  João (Mobile Node)
  Home: 200.130.10.45
        │
        ④ Resposta: MN → CN direta (src = Home Address)
        ▼
Correspondent Node (CN)
```

> ⚠️ **Roteamento Triangular:** todo tráfego de entrada passa pelo HA, mesmo que o CN esteja geograficamente próximo ao MN — isso aumenta a latência.

---

### Handoff e Roaming: quando o dispositivo se move

**Handoff** (ou handover) é a transferência da conexão ativa de uma estação base para outra. É o momento mais crítico para aplicações móveis — é quando sessões podem ser interrompidas.

#### Tipos de Handoff

| Tipo | Descrição | Tecnologias |
|---|---|---|
| **Hard Handoff** (break-before-make) | Desconecta da célula A **antes** de conectar à célula B. Há um gap de conectividade. Mais simples, mas interrompe brevemente a transmissão. | GSM, LTE, Wi-Fi (802.11) |
| **Soft Handoff** (make-before-break) | Mantém conexões simultâneas com ambas as células durante a transição. Sem gap, mais suave. Requer mais recursos de rádio. | CDMA (IS-95, CDMA2000), WCDMA/UMTS |
| **Handoff Vertical** (heterogêneo) | Troca entre **tecnologias diferentes**: Wi-Fi → 4G, 4G → 5G. Muito mais complexo: QoS diferente, autenticação diferente, latência diferente. | IEEE 802.21 |

#### 🌍 Roaming vs. Handoff

- **Handoff:** troca de estação base dentro da **mesma rede** — automático, transparente ao usuário.
- **Roaming:** acesso a uma rede de **operadora diferente** (incluindo outro país) via acordos comerciais. Envolve AAA (Authentication, Authorization, Accounting) inter-domínio (RFC 2977).

> **Referências:** Handoff vertical: IEEE 802.21-2008. Roaming inter-domínio: GLASS, S. et al. *RFC 2977*, IETF, outubro 2000. Fast Handovers: *RFC 5568* (jul 2009). HMIPv6: *RFC 5380* (out 2008).

---

## III. Limitações

### Largura de Banda, Latência e Energia

Forman & Zahorjan (1994) demonstraram que a **lacuna entre wireless e cabeado persiste ao longo das gerações** — tipicamente 1 a 3 ordens de magnitude. O 5G reduz a diferença, mas não a elimina.

#### Largura de Banda — Comparativo Real (condições não-ideais)

| Tecnologia | Throughput |
|---|---|
| Ethernet Gbps (cabeado) | 1000 Mbps |
| Wi-Fi 6 (ambiente real) | 600 Mbps |
| 5G mmWave (teórico) | 20 Gbps* |
| 5G Sub-6GHz (prático) | 200 Mbps |
| 4G LTE (médio) | 30 Mbps |
| IoT / LPWAN (mMTC) | 0,25 kbps |

#### Latência — O Inimigo Silencioso

| Tecnologia | Latência |
|---|---|
| Ethernet local | ~0,3 ms |
| Wi-Fi | 1–10 ms |
| 4G LTE | 30–50 ms |
| 5G URLLC (lab) | ≤1 ms |
| 5G Sub-6 (rede real) | 10–20 ms |
| Handoff hard (Wi-Fi) | 50–200 ms |

> ⚡ **Impacto real:** Jogo online competitivo precisa de <50 ms. Videochamada: <150 ms. Um único hard handoff Wi-Fi pode ultrapassar esse limite.

#### Energia — O Recurso Mais Escasso

| Operação | Consumo Aprox. |
|---|---|
| CPU em idle | ~100 mW |
| Wi-Fi transmitindo | ~800 mW |
| 4G transmitindo | ~1,3 W |
| GPS ativo | ~1,1 W |

> Rádio consome **10–13×** mais que CPU em idle.

#### 💡 Estratégia: push é mais eficiente que poll

Polling a cada 30s mantém o rádio ativo continuamente. WebSocket, Server-Sent Events ou MQTT mantêm uma conexão persistente de baixo overhead e o rádio pode hibernar entre mensagens.

> **Referências:** FORMAN, G. H.; ZAHORJAN, J. *IEEE Computer*, v. 27, n. 4, pp. 38–47, 1994. | FLINN, J.; SATYANARAYANAN, M. *ACM SOSP '99*, pp. 48–63, 1999. DOI: 10.1145/319151.319155.

---

### Desconexão, Segurança e Heterogeneidade

#### Operação Desconectada — Coda e Bayou

Satyanarayanan (1996) propôs tratar a desconexão como **estado esperado, não exceção**. O sistema de arquivos **Coda** (CMU) implementou três fases:

1. **Hoarding** — Pré-carrega cache antes de desconectar
2. **Emulation** — Serve requisições apenas do cache local
3. **Reintegration** — Propaga mudanças ao reconectar

O sistema **Bayou** (Xerox PARC, 1995) estendeu esse paradigma com **eventual consistency** e resolução de conflitos específica por aplicação — precursor de CRDTs e bases modernas como Firestore e Realm.

> 📱 **Hoje: offline-first** — Firebase Firestore, AWS Amplify DataStore, WatermelonDB e Realm implementam variantes desse paradigma: persistência local + sincronização quando a rede retorna. O Google Maps funciona sem internet com mapas pré-baixados — exatamente o padrão Coda.

#### Segurança em Redes Sem Fio

| Período | Padrão | Status |
|---|---|---|
| **1997** | WEP | **Quebrado** — RC4 com IV de 24 bits. Recuperação de chave por análise passiva de tráfego demonstrada em 2001 (Fluhrer, Mantin & Shamir). |
| **2003–2004** | WPA / WPA2 (IEEE 802.11i) | WPA: TKIP como correção temporária. WPA2: AES-CCMP. Atacado pelo KRACK (Vanhoef & Piessens, 2017) — patchável. |
| **2018** | WPA3 (Wi-Fi Alliance) | SAE (Dragonfly handshake), forward secrecy, Protected Management Frames obrigatórios. Resistente a ataques offline de dicionário. |

> 🔴 **Princípio para projetos:** Nunca assuma que o canal wireless é seguro, mesmo com WPA3. TLS 1.3 é obrigatório. Certificate pinning para apps críticos. Nunca armazene segredos em SharedPreferences ou UserDefaults sem criptografia.

#### Heterogeneidade de Redes

Um smartphone pode estar simultaneamente: Wi-Fi 6 + 4G (fallback) + Bluetooth (wearable) + GPS. APIs modernas (Android ConnectivityManager, iOS Network.framework) abstraem essa complexidade, mas o desenvolvedor precisa tratar **qualidade de conexão**, não apenas presença/ausência.

> **Referências:** KISTLER, J.J.; SATYANARAYANAN, M. *ACM TOCS*, v. 10, n. 1, pp. 3–25, 1992. | TERRY, D.B. et al. *ACM SOSP '95*, pp. 172–183. | FLUHRER, S.; MANTIN, I.; SHAMIR, A. *SAC 2001*, LNCS 2259, pp. 1–24, 2001. | WPA3: Wi-Fi Alliance, 2018.

---

## IV. Middleware

### O que é Middleware para Computação Móvel?

Middleware é a **camada de software entre o sistema operacional e as aplicações** que provê serviços de comunicação, coordenação e gerenciamento de contexto — escondendo a complexidade da distribuição e da mobilidade.

| Middleware SD Convencional (presume) | Middleware Móvel (precisa lidar com) |
|---|---|
| Conectividade estável e permanente | Desconexões frequentes e esperadas |
| Latência baixa e previsível | Largura de banda variável em ordens de magnitude |
| Recursos abundantes (CPU, memória) | Restrição severa de bateria e CPU |
| Localização dos participantes fixa | Mobilidade física — contexto muda o comportamento |
| — | Heterogeneidade de tecnologias de rede |

### Pilha de Camadas de Middleware Móvel

```
┌─────────────────────────────────────────┐
│          Aplicação Mobile               │  Business logic, UI, UX
├─────────────────────────────────────────┤
│      Middleware de Aplicação            │  Firebase, AWS Amplify, Supabase
├─────────────────────────────────────────┤
│      Middleware de Comunicação          │  MQTT, gRPC, REST, WebSocket
├─────────────────────────────────────────┤
│      Middleware de Contexto             │  Context-awareness, Location, Sensors
├─────────────────────────────────────────┤
│         Serviços de OS                  │  Android / iOS Network, Battery, Background Tasks
├─────────────────────────────────────────┤
│      Infraestrutura de Rede             │  Wi-Fi, 4G/5G, Bluetooth, Mobile IP
└─────────────────────────────────────────┘
```

> **Referências:** MASCOLO, C.; CAPRA, L.; EMMERICH, W. *Advanced Lectures on Networking*, LNCS v. 2497, Springer, 2002. | ISSARNY, V. et al. *4th WICSA*, Oslo, pp. 201–210, 2004. | DEY, A. K. *Personal and Ubiquitous Computing*, v. 5, n. 1, pp. 4–7, 2001.

---

### MQTT — Message Queuing Telemetry Transport

**OASIS Standard, 2014 | ISO/IEC 20922:2016**

- **Origem:** IBM (Andy Stanford-Clark) + Arcom (Arlen Nipper), **1999**, para monitoramento de oleodutos via satélite.
- **Modelo:** Publish/Subscribe via broker. Publisher não conhece subscribers — desacoplamento total no tempo e espaço.

**Por que é ideal para mobile/IoT:**
- Header mínimo: **2 bytes** — menor overhead de qualquer protocolo de mensageria
- Opera sobre TCP — atravessa NAT e firewalls
- 3 níveis de QoS: 0 (at most once), 1 (at least once), 2 (exactly once)
- Sessions persistentes: broker retém mensagens se cliente desconectar
- Last Will & Testament: notifica desconexão inesperada

```
Sensor (pub) → Broker → App (sub 1)
                     → API (sub 2)
```

**Usado por:** AWS IoT Core, Azure IoT Hub, HiveMQ, Mosquitto. Padrão de facto para IoT.

---

### gRPC — Google Remote Procedure Call

**Open source, fev/2015 | CNCF**

- **Origem:** Google, baseado no Stubby interno (~2001). Open-sourced em 26 fev 2015. Gerenciado pela CNCF.
- **Modelo:** RPC binário. Cliente chama métodos do servidor como funções locais — stubs gerados automaticamente a partir de um arquivo `.proto`.

**Por que é relevante para mobile:**
- **HTTP/2:** multiplexação — múltiplos streams numa única conexão TCP
- **Protocol Buffers:** 3–10× mais compacto que JSON
- Compressão de headers HPACK: ~85% de redução
- Streaming bidirecional nativo — ideal para tempo real
- Tipagem forte: contratos de API verificados em tempo de compilação

```
App Mobile ⇄ gRPC Channel (HTTP/2 + TLS) ⇄ Microserviço
```

**Usado por:** Netflix, Spotify, Slack. SDK oficial para Swift, Kotlin, Flutter.

> **Referências:** MQTT: STANFORD-CLARK, A.; NIPPER, A. (IBM/Arcom), 1999. Padrão OASIS MQTT v3.1.1, 29 out 2014. ISO/IEC 20922:2016. MQTT 5.0: mar 2019. | gRPC: Google, open source fev 2015. Protocol Buffers: Google. RFC 7540 (HTTP/2, IETF, 2015).

---

### Firebase e AWS Amplify — BaaS para Mobile

Backend-as-a-Service elimina a necessidade de construir e operar infraestrutura backend do zero.

#### 🔥 Firebase (Google)
*Fundado 2011 · Adquirido Google: out/2014*

| Serviço | Descrição |
|---|---|
| **Realtime Database** | NoSQL JSON em tempo real. Sincronização instantânea via WebSocket. |
| **Cloud Firestore** | NoSQL documental mais escalável. **Offline persistence** embutida. |
| **Authentication** | OAuth 2.0, OIDC, SAML, anônimo, telefone. Integrado com Social Login. |
| **Cloud Messaging (FCM)** | Push notifications para Android, iOS e Web. |
| **Cloud Functions** | Serverless triggers em resposta a eventos do Firestore, Auth, etc. |

- ✅ **Ideal quando:** MVP rápido; apps com real-time colaborativo; ecossistema Google (Android first); projetos onde offline sync é requisito central.
- ⚠️ **Atenção:** Vendor lock-in: migração difícil. Consultas NoSQL limitadas (sem JOINs complexos). Custos podem escalar abruptamente com leitura/escrita intensa.

#### ☁️ AWS Amplify (Amazon)
*Lançado: nov/2017 · Gen 2: nov/2023*

| Serviço | Descrição |
|---|---|
| **Amazon Cognito** | Auth com MFA, SAML, OIDC, User Pools. Controle fino de políticas. |
| **AWS AppSync (GraphQL)** | API GraphQL gerenciada com real-time subscriptions e **offline sync** via DataStore. |
| **Amplify DataStore** | Armazenamento local com sincronização automática. Modelo de dados definido em schema GraphQL. |
| **Amplify Gen 2 (2023)** | Abordagem TypeScript-first, code-first. Infraestrutura declarada em código. |

- ✅ **Ideal quando:** Organização já usa AWS; necessidade de integração com serviços AWS (S3, Lambda, SQS); compliance e controle de dados em regiões específicas.
- ⚠️ **Atenção:** Curva de aprendizado maior. Complexidade de configuração IAM. SDK frequentemente muda (Gen 1 → Gen 2). Também com vendor lock-in.

---

### Análise Comparativa — Como Escolher?

| Critério | MQTT | gRPC | Apache Thrift | Firebase | AWS Amplify |
|---|---|---|---|---|---|
| **Modelo arquitetural** | Pub/Sub via broker | RPC binário (HTTP/2) | RPC cross-language | BaaS (sync em tempo real) | BaaS (GraphQL/REST) |
| **Overhead de rede** | Mínimo (2 bytes header) | Baixo (protobuf binário) | Baixo (binário) | Moderado (JSON/WebSocket) | Moderado (GraphQL JSON) |
| **Suporte a offline** | Sessões persistentes + QoS | Sem suporte nativo | Sem suporte nativo | Firestore offline nativo | DataStore offline-first |
| **Segurança** | TLS (porta 8883) | TLS + JWT nativo | TLS plugável | Auth + Security Rules | Cognito (OAuth/SAML/MFA) |
| **Curva de aprendizado** | Baixa | Média (requer .proto) | Média (IDL) | Baixa | Alta |
| **Vendor lock-in** | Open standard (OASIS/ISO) | Open source (CNCF) | Open source (Apache) | Alto (Google) | Alto (Amazon) |
| **Caso de uso primário** | IoT, telemetria, dispositivos restritos | Microserviços, backend mobile | Serviços backend polyglot | Apps real-time, MVPs rápidos | Apps no ecossistema AWS |
| **Origem / Maturidade** | IBM/Arcom, 1999. OASIS 2014. | Google, 2015. CNCF. | Facebook, 2007. Apache TLP 2010. | Fundado 2011, Google 2014. | Amazon, nov 2017. |

### 🏗️ Guia Rápido de Decisão para Projetos

- **Dispositivos IoT / telemetria:** 👉 MQTT (baixo overhead, bateria, conectividade intermitente)
- **App móvel com backend próprio:** 👉 gRPC (eficiência + tipagem) ou REST (interoperabilidade)
- **MVP / startup / equipe pequena:** 👉 Firebase (velocidade de desenvolvimento + offline pronto)

> **Referências:** Apache Thrift: SLEE, M.; AGARWAL, A.; KWIATKOWSKI, M. Facebook, 2007. Apache TLP: out 2010. | ENDLER, M. et al. *Handbook of Mobile Middleware*, Taylor & Francis/CRC Press, 2006. | ISSARNY, V.; CAPORUSCIO, M.; GEORGANTAS, N. FOSE/ICSE 2007.

---

## Síntese — O que Levar para a Prática

### Mapa Conceitual

```
Computação Móvel
(mobilidade + sem fio + portabilidade)
        ↓ gera
4 Restrições Intrínsecas (Satyanarayanan, 1996)
+ Problema de Endereçamento → Mobile IP (RFC 5944)
        ↓ exige
Middleware Especializado
(MQTT · gRPC · Firebase · Amplify · Thrift)
        ↓ viabiliza
Soluções Móveis Robustas
(offline-first · adaptativas · seguras)
```

### Princípios para Projetos

1. **Projete para a variabilidade, não para a estabilidade** — Trate desconexão como estado normal. Implemente offline-first e degradação graciosa da UX.
2. **Minimize overhead de rede** — JSON verboso custa bateria e banda. Considere MQTT para IoT, protobuf para APIs de alta frequência.
3. **Bateria é o recurso mais escasso** — Push > Poll sempre que possível. Agrupe operações de rede. Evite wakeups desnecessários do rádio.
4. **TLS é obrigatório, não opcional** — Canal wireless é fisicamente exposto. Nunca trafegue dados sensíveis sem criptografia de transporte.

---

## Referências-Chave

- WEISER, M. *Scientific American*, 1991 — visão fundadora
- SATYANARAYANAN, M. *ACM PODC '96* — restrições intrínsecas
- FORMAN & ZAHORJAN. *IEEE Computer*, 1994 — desafios
- RFC 5944 (IPv4) / RFC 6275 (IPv6) — Mobile IP
- KISTLER & SATYANARAYANAN. *ACM TOCS*, 1992 — Coda / offline
- MASCOLO et al. *LNCS 2497*, Springer, 2002 — middleware survey

---

> 🚀 **Próxima Aula:** Consistência e Replicação em Sistemas Distribuídos — modelos de consistência, replicação ativa/passiva, quorum e protocolos de consenso.

---
*PUC Minas / ICEI — Aplicações Móveis e Distribuídas · 2026/1*