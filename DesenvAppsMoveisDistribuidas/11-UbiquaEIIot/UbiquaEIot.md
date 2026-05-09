# Computação Ubíqua, Contexto e IoT — PUC Minas

**Disciplina:** Engenharia de Software · ICEI — Aplicações Móveis e Distribuídas · 2026/1  
**Professor:** Cristiano de Macedo Neto  
**Aula:** 12 — Da visão de 1991 ao mundo conectado de hoje

---

## Mapa da Aula

| Bloco | Conteúdo | Slides |
|---|---|---|
| I | Computação Ubíqua | 2–3 |
| II | Sistemas Cientes de Contexto | 4–5 |
| III | Internet das Coisas | 6–8 |
| IV | Edge & Cloud | 9–11 |

### 🎯 Nesta aula você vai:

- Compreender a visão de Weiser (1991) e por que ela é mais relevante hoje do que era em 1991
- Entender o que torna um sistema "ciente de contexto" e como ele usa localização, sensores e tempo
- Conhecer a arquitetura em camadas da IoT e os protocolos MQTT e CoAP
- Distinguir cloud, fog e edge computing e saber quando cada abordagem é necessária

> **"The most profound technologies are those that disappear. They weave themselves into the fabric of everyday life until they are indistinguishable from it."**  
> — WEISER, M. The Computer for the 21st Century. *Scientific American*, v. 265, n. 3, pp. 94–104, 1991.

---

## I. Computação Ubíqua — A visão de 1991 que ainda guia o campo

Em 1991, Mark Weiser era chief technologist do Xerox PARC, o laboratório que inventou a interface gráfica e o mouse. Seu artigo propôs uma inversão radical: em vez de o usuário entrar no mundo do computador, o computador entraria no mundo do usuário.

### Os três form factors de Weiser

**📌 Tabs — escala de centímetros:** Dispositivos pequenos, especializados, distribuídos pelo ambiente. Análogo atual: **sensores IoT, wearables, tags NFC**.

**📋 Pads — escala de folha A4:** Dispositivos de superfície, portáteis, descartáveis. Análogo atual: **tablets e smartphones**.

**💻 Boards — escala de parede:** Superfícies interativas grandes e compartilhadas. Análogo atual: **smart TVs, displays públicos, quadros interativos**.

Analogias hoje:
- Tab → sensor IoT, wearable
- Pad → smartphone, tablet
- Board → smart TV, display público

### 💡 Por que essa visão era radical em 1991

Em 1991 existia um computador por empresa. A ideia de que cada pessoa teria dezenas de dispositivos ao seu redor era ficção científica. Hoje um brasileiro com smartphone, smartwatch, smart TV e roteador Wi-Fi já tem mais de dez computadores ao seu redor.

### Evolução do conceito: de ubíquo a IoT

**1991 — Ubiquitous Computing — Weiser (Xerox PARC)**  
Computação invisível integrada ao ambiente físico. Foco em hardware e redes. PCs eram raros e caros.

**1996 — Calm Technology — Weiser & Brown**  
Tecnologia que opera na periferia da atenção humana: informa sem interromper, age sem exigir foco.

**1999 — Pervasive Computing — IBM**  
Computação em objetos cotidianos conectados. Foco em mobilidade e dispositivos embarcados.

**2001 — Context-Aware Computing — Dey & Abowd (Georgia Tech)**  
Sistemas que adaptam comportamento com base no contexto do usuário. Foco em software adaptativo.

**2009 — Internet of Things — Ashton (MIT/P&G)**  
Kevin Ashton cunha o termo ao propor sensores RFID conectando objetos físicos à internet.

**2019+ — Ambient Intelligence + Edge AI**  
Ambientes que percebem, aprendem e agem. A visão de Weiser em escala industrial com IA embarcada.

> **Referências:** WEISER, M. The Computer for the 21st Century. *Scientific American*, v. 265, n. 3, pp. 94–104, set. 1991. | WEISER, M.; BROWN, J. S. The Coming Age of Calm Technology. Xerox PARC, 1996. DOI: 10.1007/978-1-4612-0685-9_6. | ASHTON, K. That 'Internet of Things' Thing. *RFID Journal*, jun. 2009. | ATZORI, L.; IERA, A.; MORABITO, G. The Internet of Things: A Survey. *Computer Networks*, v. 54, n. 15, pp. 2787–2805, 2010. DOI: 10.1016/j.comnet.2010.05.010.

---

## I. Computação Ubíqua — Calm Technology e Ambient Intelligence

Weiser e Brown (1996) aprofundaram a visão original: tecnologia que pode estar tanto no centro quanto na periferia da atenção humana, sem exigir foco constante. O objetivo é o sistema agir no momento certo, invisível quando não é necessário.

### Centro vs. periferia da atenção

**Centro da atenção:** O usuário foca ativamente aqui. Ex: Ler uma mensagem, digitar um texto.

**Periferia da atenção:** Percebido sem esforço consciente. Ex: Luz ambiente, ícone de bateria, som de fundo.

A *calm technology* opera na periferia — informa sem interromper. Alerta quando necessário, invisível quando não é.

### 📱 Exemplos práticos hoje

- **Smartwatch:** batimentos no pulso em periferia constante; alerta no centro se anormal
- **Termostato inteligente:** aprende rotinas sem interação explícita
- **Semáforo adaptativo:** ajusta timing pelo tráfego sem que ninguém perceba
- **ABS do carro:** Weiser citava como exemplo pioneiro de ubicomp bem-sucedido

### Ambient Intelligence (AmI)

**O que é:** Evolução da computação ubíqua: ambientes que percebem, aprendem e respondem proativamente às necessidades humanas. O ambiente inteiro se torna o "computador". Definido formalmente por Espinilla, Villarreal e McChesney (Sensors, 2019) como "nova geração de ambientes centrados no usuário".

**Componentes técnicos de um ambiente AmI:**
- **Sensores:** câmera, microfone, temperatura, presença, luminosidade
- **Atuadores:** iluminação inteligente, HVAC, travas automáticas
- **Inferência:** modelos de ML para reconhecer padrões de comportamento
- **Conectividade:** IoT, Wi-Fi, Bluetooth, Zigbee, Z-Wave
- **Interface natural:** voz, gesto, presença — sem teclado ou mouse

### 💡 O desafio central: invisibilidade sem invasão

Quanto mais o sistema percebe, mais útil ele pode ser — mas também mais exposto fica o usuário. A privacidade em sistemas AmI não é uma funcionalidade opcional: é o equilíbrio que determina se a tecnologia realmente "desaparece" de forma positiva ou se o usuário começa a desconfiar dela. Voltaremos a isso quando falarmos de contexto e LGPD.

> **Referências:** WEISER, M.; BROWN, J. S. The Coming Age of Calm Technology. Xerox PARC, 1996. In: DENNING, P.; METCALFE, R. (ed.) *Beyond Calculation*. Springer, 1997. DOI: 10.1007/978-1-4612-0685-9_6. | ESPINILLA, M.; VILLARREAL, V.; McCHESNEY, I. Ubiquitous Computing and Ambient Intelligence. *Sensors*, v. 19, n. 18, art. 4034, 2019. DOI: 10.3390/s19184034.

---

## II. Sistemas Cientes de Contexto — O que é contexto e por que importa

> **"Context is any information that can be used to characterize the situation of an entity. An entity is a person, place, or object that is considered relevant to the interaction between a user and an application."**  
> — DEY, A. K. Understanding and Using Context. *Personal and Ubiquitous Computing*, v. 5, n. 1, pp. 4–7, 2001. DOI: 10.1007/s007790170019

### Os quatro tipos primários de contexto

**📍 Localização:** Onde o usuário está: GPS, triangulação Wi-Fi, beacons BLE, indoor positioning. É o contexto mais usado — habilita serviços baseados em proximidade.  
*Ex: "o usuário está no campus → mostrar cardápio do restaurante universitário"*

**👤 Identidade:** Quem é o usuário: perfil, preferências, histórico. Permite personalização sem perguntar.  
*Ex: "usuário João prefere tema escuro e notícias de tecnologia"*

**⚡ Atividade:** O que o usuário está fazendo: dirigindo, correndo, em reunião, dormindo. Detectado por acelerômetro, giroscópio e microfone.  
*Ex: "usuário está dirigindo → não enviar notificações, ativar leitura em voz alta"*

**⏰ Tempo:** Hora do dia, dia da semana, estação. O contexto mais simples e frequentemente mais poderoso.  
*Ex: "são 7h e o usuário está em sono leve → ativar alarme suave"*

### Exemplos que você já usa hoje

| Contexto detectado | Comportamento adaptado |
|---|---|
| Fones de ouvido desconectados | Spotify pausa automaticamente |
| Luz ambiente baixa (sensor) | Tela ativa modo escuro |
| Localização: trabalho (GPS) | Maps sugere rota para casa às 18h |
| Atividade: correndo (acelerômetro) | Siri lê mensagens em voz alta |
| Horário: madrugada + silêncio | Modo não-perturbe automático |

### 🤖 HAR — Human Activity Recognition

Técnica de ML que classifica atividades físicas (andar, correr, parado, no veículo) a partir do acelerômetro e giroscópio. No Flutter, o pacote `activity_recognition_flutter` usa a API nativa do SO (Android Activity Recognition API / iOS CMMotionActivity) — sem treinar modelo: o próprio sistema operacional já faz a classificação e expõe o resultado.

### 🔒 Privacidade: o princípio do mínimo necessário

Peça apenas os sensores que o app realmente usa. Localização precisa o tempo todo raramente é necessária — "apenas durante o uso" cobre 90% dos casos. A LGPD (Art. 6, III) exige minimização de dados coletados.

> **Referências:** DEY, A. K. Understanding and Using Context. *Personal and Ubiquitous Computing*, v. 5, n. 1, pp. 4–7, 2001. DOI: 10.1007/s007790170019. | DEY, A. K.; ABOWD, G. D.; SALBER, D. A Conceptual Framework and a Toolkit for Supporting the Rapid Prototyping of Context-Aware Applications. *Human-Computer Interaction*, v. 16, n. 2–4, pp. 97–166, 2001. | HAR: surveys IEEE Sensors Journal, 2022–2024.

---

## II. Sistemas Cientes de Contexto — Contexto em aplicações móveis com Flutter

O smartphone é a plataforma de contexto mais rica da história: GPS, acelerômetro, giroscópio, bússola, barômetro, microfone, câmera, NFC e sensores de luz e proximidade. A questão não é *se* capturar contexto, mas *como* usá-lo sem invadir privacidade.

### Pacotes Flutter para contexto

**📍 Localização — geolocator:** GPS, accuracy configurável, geofencing. Permissão explícita obrigatória: `ACCESS_FINE_LOCATION` no Android, `NSLocationWhenInUseUsageDescription` no iOS.

**🏃 Movimento — sensors_plus:** Acelerômetro, giroscópio, magnetômetro em stream contínuo. Pacote `activity_recognition_flutter` classifica atividade usando a API nativa do SO (Android Activity Recognition, iOS CMMotionActivity).

**📶 Conectividade e ambiente:** `connectivity_plus`: tipo de rede (Wi-Fi, 4G, offline). `battery_plus`: nível e estado de carga. `light_sensor`: luminosidade ambiente.

### O pipeline de contexto

**Exemplo concreto:**  
**Situação:** usuário a 200m do supermercado (GPS) com "leite" na lista de compras.  
**Resultado esperado:** notificação push "Você está perto do Supermercado X — leite está na sua lista."

```
1. Coleta (Sensores)
   GPS → coordenadas do usuário a cada 30s
        ↓
2. Interpretação
   Coordenadas + geofence → "dentro de 200m do supermercado"
        ↓
3. Adaptação do Comportamento
   Lista tem "leite" → enviar push notification
```

**Ferramentas Flutter para esse exemplo:**  
`geolocator` + geofencing → `flutter_local_notifications`  
Permissão necessária: localização em background (`ACCESS_BACKGROUND_LOCATION` Android 10+)

### 🔒 Privacidade: mínimo necessário

Localização em background exige justificativa clara ao usuário e revisão rigorosa da app store. A LGPD (Art. 6, III) exige minimização: colete só o que o app realmente usa.

> **Referências:** geolocator: pub.dev/packages/geolocator | sensors_plus: pub.dev/packages/sensors_plus (Flutter Favorite) | connectivity_plus: pub.dev/packages/connectivity_plus | Android Activity Recognition API: developers.android.com/training/location/transitions | DEY, A. K. The Context Toolkit: Aiding the Development of Context-Aware Applications. Doctoral dissertation, Georgia Tech, 2000.

---

## III. Internet das Coisas — Da visão à escala industrial

O termo *Internet of Things* foi cunhado por **Kevin Ashton** em **1999**, enquanto trabalhava na Procter & Gamble: ele propôs conectar etiquetas RFID de produtos à internet para rastreamento automático de estoque. O termo ficou conhecido mundialmente após Ashton publicar uma retrospectiva no *RFID Journal* em 2009, explicando a origem do conceito. A escala que imaginava para um armazém hoje existe em toda a cadeia de produção global.

### Escala atual e projeções

| Métrica | Valor |
|---|---|
| Dispositivos IoT conectados no mundo (2024) | ~18 bilhões |
| Projeção para 2030 (Ericsson) | ~29 bilhões |
| Dados gerados por dispositivos conectados em 2025 (IDC*) | 79 ZB |
| Mercado IoT global 2024 (estimativa) | US$ 600 bilhões |

*IDC Data Age 2025: 79 ZB refere-se ao total de dados gerados, capturados e copiados por todos os dispositivos conectados ao mundo — não exclusivamente IoT.

### 🏭 Domínios de aplicação

- **Smart Home:** automação residencial, segurança, energia
- **Saúde:** monitoramento remoto de pacientes, wearables médicos
- **Indústria 4.0:** manutenção preditiva, robótica conectada
- **Cidades inteligentes:** semáforos adaptativos, coleta de lixo otimizada
- **Agronegócio:** sensores de solo, drones, irrigação de precisão
- **Logística:** rastreamento de frota, cadeia do frio, inventário RFID

### Arquitetura IoT em três camadas

```
Camada de Aplicação
  Dashboards · Analytics · Alertas · Automação · Business Intelligence
        ↑ MQTT / CoAP / HTTP ↑
Camada de Rede / Middleware
  Gateways · Brokers MQTT · Edge Nodes · Cloud Platforms
        ↑ Zigbee / BLE / LoRa / Wi-Fi ↑
Camada de Percepção (dispositivos)
  Sensores · Atuadores · MCUs (ESP32, Arduino) · RFID · Câmeras
```

### 🔗 Por que três camadas?

Dispositivos da camada inferior têm CPU, memória e bateria limitadas — não executam TLS completo nem HTTP. Protocolos leves como MQTT e CoAP foram criados para essa restrição. A camada de aplicação usa protocolos padrão para expor dados aos sistemas de negócio.

> **Referências:** ASHTON, K. That 'Internet of Things' Thing. *RFID Journal*, jun. 2009. | ATZORI, L.; IERA, A.; MORABITO, G. The Internet of Things: A Survey. *Computer Networks*, v. 54, n. 15, pp. 2787–2805, 2010. DOI: 10.1016/j.comnet.2010.05.010 (>25.000 citações). | Arquitetura em três camadas: ITU-T Y.2060 (2012). | Projeções: Ericsson Mobility Report 2024.

---

## III. Internet das Coisas — Protocolos para dispositivos restritos: MQTT e CoAP

Dispositivos IoT não podem usar HTTP como um servidor web — consome muita bateria e banda. Dois protocolos dominam o campo: MQTT para telemetria contínua com sessões persistentes, e CoAP para consultas pontuais em redes com perdas.

### MQTT — Message Queuing Telemetry Transport

- **Origem:** criado em 1999 por Andy Stanford-Clark (IBM) e Arlen Nipper (Arcom) para monitorar oleodutos via satélite com banda mínima.
- **Modelo:** Publish/Subscribe via broker centralizado
- **Transporte:** TCP/IP — garante entrega ordenada
- **Header mínimo:** apenas 2 bytes
- **QoS:** 0 (at most once) · 1 (at least once) · 2 (exactly once)
- **Padrão:** OASIS v3.1.1 (out. 2014), ISO/IEC 20922:2016

**Fluxo:** Sensor publica em `sensor/temp/sala01` → Broker distribui → App e Dashboard inscrevem e recebem

**Ideal para:** Telemetria contínua, dispositivos com bateria, conexão intermitente. Sessões persistentes: broker retém mensagens enquanto o cliente está offline.

### CoAP — Constrained Application Protocol (RFC 7252)

- **Origem:** IETF CoRE Working Group. RFC 7252 publicado em junho de 2014. Autores: Shelby, Hartke, Bormann.
- **Modelo:** Request/Response — similar ao HTTP, mas binário e compacto
- **Transporte:** UDP — sem overhead de conexão TCP
- **Métodos:** GET, POST, PUT, DELETE (como HTTP REST)
- **Segurança:** DTLS (Datagram TLS, equivalente ao TLS sobre UDP)

**Fluxo:** App GET `coap://sensor.local/temp` → Sensor responde imediatamente → Sem broker necessário

**Ideal para:** Consultas pontuais, dispositivos em rede local, integração com arquitetura REST. Menor overhead que MQTT para leituras esporádicas.

### Comparativo MQTT vs CoAP

| Critério | MQTT | CoAP |
|---|---|---|
| Modelo | Pub/Sub | Request/Response |
| Transporte | TCP | UDP |
| Broker necessário | Sim | Não |
| Offline / sessão | QoS + sessão persistente | Sem suporte |
| Padrão | OASIS / ISO/IEC | IETF RFC 7252 |

> **Referências:** MQTT: STANFORD-CLARK, A.; NIPPER, A. (IBM/Arcom), 1999. OASIS MQTT v3.1.1, out. 2014. ISO/IEC 20922:2016. | CoAP: SHELBY, Z.; HARTKE, K.; BORMANN, C. The Constrained Application Protocol (CoAP). *RFC 7252*, IETF, jun. 2014. | BORMANN, C.; CASTELLANI, A. P.; SHELBY, Z. CoAP: An application protocol for billions of tiny internet nodes. *IEEE Internet Computing*, v. 16, n. 2, pp. 62–67, 2012.

---

## III. Internet das Coisas — Plataformas IoT e um caso prático

### Principais plataformas de nuvem IoT

| Plataforma | Broker | Edge | Destaque |
|---|---|---|---|
| AWS IoT Core | MQTT gerenciado | Greengrass | Regras para Lambda/S3/DynamoDB |
| Azure IoT Hub | MQTT / AMQP / HTTP | IoT Edge | Device twins (estado desejado vs. real) |
| Google Cloud IoT | MQTT via Pub/Sub | Edge TPU | Analytics com BigQuery e Vertex AI |
| Mosquitto (open source) | MQTT local | — | Referência para lab e MVPs locais |

### Exemplo: monitoramento de câmara fria

🌡️ Sensor de temperatura em farmácia — alerta em tempo real

```
ESP32 + sensor DS18B20
  Publica a cada 30s em farmacia/camara1/temp via MQTT TLS (porta 8883)
        ↓ MQTT ↓
Broker MQTT (AWS IoT Core)
  Regra: se temp > 8°C → acionar Lambda
        ↓ Trigger ↓
Lambda + SNS
  Envia SMS e push notification para o farmacêutico responsável
        ↓ Dashboard ↓
App Flutter
  Histórico de temperatura, gráfico em tempo real, alertas configuráveis
```

### ⚠️ Segurança em IoT

TLS obrigatório (porta 8883 no MQTT), credenciais únicas por dispositivo via certificados X.509. Nunca use usuário/senha compartilhado entre dispositivos. OWASP IoT Top 10: "Weak Credentials" é o risco número 1.

> **Referências:** AWS IoT Core: aws.amazon.com/iot-core | Azure IoT Hub: azure.microsoft.com/products/iot-hub | Mosquitto: mosquitto.org (Eclipse Foundation). | MQTT TLS porta 8883: OASIS MQTT v3.1.1, seção 4.3. | OWASP IoT Top 10 2024: owasp.org/www-project-iot-security.

---

## IV. Edge & Cloud — Por que a nuvem centralizada não é suficiente para IoT

A nuvem funciona bem para armazenar e analisar dados históricos. Mas bilhões de dispositivos IoT gerando dados em tempo real criam dois problemas que a computação centralizada não resolve: **latência** e **volume de banda**.

### ☁️ Cloud Computing

**Pontos fortes:**
- Armazenamento praticamente ilimitado
- Poder computacional escalável sob demanda
- Analytics complexo sobre dados históricos
- Treinamento de modelos ML de grande escala

**Limitações para IoT:**
- **Latência:** 50–200ms — inaceitável para controle em tempo real
- **Banda:** enviar 79 ZB/ano para a nuvem seria inviável
- **Offline:** sem rede, sem processamento
- **Privacidade:** enviar tudo pode violar LGPD

### 🌫️ Fog Computing (Cisco, 2012)

**Conceito:** Processamento distribuído entre dispositivos e nuvem. "Fog" porque está entre a nuvem e o chão. Cisco propôs em 2012 via OpenFog Consortium. Roteadores e switches inteligentes processam dados localmente.

**Na prática:**
- Agrega dados de múltiplos sensores antes de enviar para nuvem
- Filtra e comprime: de 1000 leituras/s envia só anomalias
- Latência: 10–50ms
- Continua funcionando sem acesso à internet

### ⚡ Edge Computing

**Conceito:** Processamento no próprio dispositivo ou imediatamente ao lado. Satyanarayanan (CMU) propôs *cloudlets* em 2009: mini-nuvens locais com capacidade de VM, com os mesmos padrões técnicos da nuvem mas muito mais próximas do usuário.

**Na prática:**
- Raspberry Pi / Jetson Nano processa vídeo de câmera localmente
- Decisão tomada em menos de 10ms sem internet
- Apenas resultado vai para nuvem — não o dado bruto
- AWS Greengrass e Azure IoT Edge são plataformas edge

> **Referências:** Fog Computing: Cisco, OpenFog Consortium, 2012. | SATYANARAYANAN, M. et al. The Case for VM-Based Cloudlets in Mobile Computing. *IEEE Pervasive Computing*, v. 8, n. 4, pp. 14–23, 2009. | SATYANARAYANAN, M. The Emergence of Edge Computing. *IEEE Computer*, v. 50, n. 1, pp. 30–39, 2017.

---

## IV. Edge & Cloud — Onde processar cada dado: critérios práticos

A decisão de processar na borda ou na nuvem não é binária. Sistemas modernos usam arquitetura em camadas onde cada tipo de dado vai para onde faz mais sentido, guiados por três critérios: **latência exigida**, **privacidade** e **volume de dados**.

### Tabela de decisão

| Dado / Decisão | Onde processar | Por quê |
|---|---|---|
| Freada de emergência (ABS) | Dispositivo | Latência <1ms exigida |
| Detecção de intruso em câmera | Edge | Privacidade + latência <100ms |
| Alerta de temperatura fora do padrão | Edge ou Fog | Decisão simples, precisa ser rápida |
| Manutenção preditiva de máquinas | Fog / Cloud | ML complexo sobre dados históricos |
| Relatório mensal de consumo de energia | Cloud | Sem urgência, escala de dados |
| Treinamento de modelos ML | Cloud | Requer poder computacional enorme |

### 🏗️ Arquitetura de referência: três camadas

```
Cloud: analytics histórico, ML, armazenamento de longo prazo
Fog:   agregação, filtragem, decisões de negócio com baixa latência
Edge:  decisões críticas em tempo real, privacidade, offline
```

### Exemplo: câmera inteligente em linha de produção

🎥 Detecção de defeitos em peças a 1200 unidades/hora

```
Câmera + Jetson Nano (Edge)
  Detecta defeito em menos de 50ms, aciona parada da esteira localmente.
  O vídeo nunca sai do dispositivo.
        ↓ Envia apenas: "peça 4872 — defeito detectado — 14h32" ↓
Gateway local (Fog)
  Agrega ocorrências de todas as câmeras. Acumula padrões por turno.
        ↓ Envia apenas: taxa de defeitos por hora ↓
Cloud (Azure / AWS)
  "Terças às 14h a taxa de defeitos sobe 40% — verificar desgaste de ferramenta."
  Dashboard da gerência.
```

### 💡 O princípio fundamental

**Não suba para a nuvem o que pode ser decidido na borda.** Cada dado que sobe consome banda, adiciona latência e pode violar privacidade. Neste exemplo, o vídeo nunca saiu da câmera — apenas a decisão binária (defeito ou não) percorreu a rede.

> **Referências:** SATYANARAYANAN, M. The Emergence of Edge Computing. *IEEE Computer*, v. 50, n. 1, pp. 30–39, jan. 2017. | AWS Greengrass: aws.amazon.com/greengrass | Azure IoT Edge: azure.microsoft.com/products/iot-edge | ETSI MEC (Multi-Access Edge Computing): etsi.org/technologies/multi-access-edge-computing.

---

## Checkpoint — Verifique seu entendimento

### 🎯 Questão 1 — Computação ubíqua

Um carro moderno com ABS, controle de tração e assistente de manutenção de faixa é frequentemente citado como exemplo de computação ubíqua bem-sucedida. Por qual razão específica esse exemplo ilustra o princípio de Weiser?

- A) Porque os computadores do carro são fisicamente pequenos e invisíveis ao motorista, o que satisfaz o critério de miniaturização proposto por Weiser.
- B) Porque o carro está conectado à internet e envia dados para a nuvem, integrando o veículo à infraestrutura de computação ubíqua global.
- **C) Porque o motorista usa esses sistemas sem pensar neles. O ABS age no momento exato necessário sem qualquer interação explícita: percebe o contexto (derrapagem iminente) e responde. A tecnologia entrou completamente na periferia da atenção do usuário. ✅**
- D) Porque o carro coleta dados do motorista em tempo real e os processa na nuvem para devolver assistência personalizada ao condutor.

**Resposta:** O ABS é o exemplo que o próprio Weiser usava para ilustrar a computação ubíqua bem-sucedida. O sistema percebe o contexto (rodas travando), age imediatamente e o motorista sequer sabe que o computador interveio. A tecnologia entrou completamente na periferia da atenção: "desapareceu" na experiência do usuário. Isso é exatamente o que Weiser e Brown chamaram de calm technology em 1996. Miniaturização e conectividade com internet são características técnicas, mas não capturam o princípio central de Weiser. O argumento não é sobre tamanho físico nem sobre estar online. O que importa é a invisibilidade experiencial.

---

### 🎯 Questão 2 — MQTT vs CoAP

Um sistema de monitoramento de solo agrícola tem 500 sensores que publicam temperatura e umidade automaticamente a cada 10 minutos. Os sensores ficam offline durante a madrugada por falta de energia solar. Qual protocolo é mais adequado e por quê?

- A) CoAP, pois usa UDP, que consome menos energia que TCP nas leituras periódicas e tem menor overhead por mensagem.
- **B) MQTT, pois o modelo publish/subscribe com sessões persistentes garante que as mensagens geradas antes de o sensor ficar offline sejam entregues ao broker quando a conexão retornar. O QoS 1 confirma entrega sem sobrecarregar o dispositivo. ✅**
- C) HTTP REST, pois é o protocolo padrão da internet e garante compatibilidade com qualquer servidor sem configuração adicional.
- D) CoAP com o modo "observe", pois foi projetado especificamente para notificações periódicas de sensores e é mais eficiente que MQTT para esse padrão.

**Resposta:** O MQTT é a escolha certa por duas razões complementares. Primeiro: o modelo publish/subscribe com sessões persistentes (Clean Session = false) garante que mensagens publicadas antes da desconexão sejam armazenadas pelo broker e entregues quando o sensor reconectar. Segundo: o QoS 1 confirma que cada mensagem chegou ao broker sem o handshake completo do QoS 2, economizando energia. O CoAP não tem mecanismo de sessão persistente para lidar com desconexões programadas.

---

### 🎯 Questão 3 — Edge vs Cloud

Uma fábrica usa câmeras para detectar defeitos em peças numa linha de produção a 1200 unidades/hora. O sistema de controle exige que a esteira seja parada em **menos de 10ms** após a detecção do defeito para evitar danos ao equipamento. Onde o processamento de imagem deve ocorrer?

- A) Na nuvem (AWS ou Azure), pois possuem GPUs poderosas para executar modelos de visão computacional avançados em alta resolução.
- B) Em um servidor fog dentro da própria rede da fábrica, pois reduz a latência para 10–50ms sem depender de conexão com a internet.
- **C) No próprio dispositivo edge acoplado à câmera (ex: NVIDIA Jetson Nano ou câmera com NPU integrada), pois apenas o processamento local garante latência abaixo de 10ms. Mesmo um servidor fog na mesma sub-rede introduz latência de rede de 10–50ms, o que já excede o limite exigido pelo sistema de controle. ✅**
- D) Em um cluster Kubernetes on-premises, pois oferece equilíbrio entre poder computacional local e facilidade de gerenciamento de modelos de ML.

**Resposta:** A latência exigida é menos de 10ms, um threshold muito mais restritivo que os 50ms do exemplo anterior. A nuvem tem round-trip de 50–200ms. O servidor fog, mesmo na mesma rede local, introduz 10–50ms de latência de rede, o que já viola o limite. Kubernetes on-premises tem a mesma limitação de rede. Apenas o dispositivo edge acoplado fisicamente à câmera consegue tomar a decisão sem percorrer nenhuma rede.

---

## Síntese — Da visão de 1991 ao mundo conectado

### O fio condutor desta aula

```
Weiser (1991): computação invisível
  A tecnologia some para o usuário poder aparecer
        ↓ exige
Context-Aware: o sistema entende a situação
  Localização · Atividade · Identidade · Tempo
        ↓ escala com
IoT: bilhões de pontos de percepção e atuação
  MQTT · CoAP · Sensores · Atuadores
        ↓ processado por
Edge + Fog + Cloud: cada dado no lugar certo
  Latência · Privacidade · Escala · Custo
```

### Referências essenciais

- **Weiser (1991):** Scientific American, v. 265, n. 3
- **Weiser & Brown (1996):** Calm Technology, Xerox PARC
- **Dey (2001):** Personal and Ubiquitous Computing. DOI: 10.1007/s007790170019
- **Atzori et al. (2010):** Computer Networks. DOI: 10.1016/j.comnet.2010.05.010
- **CoAP:** RFC 7252, IETF, jun. 2014
- **MQTT:** OASIS v3.1.1, out. 2014 / ISO/IEC 20922:2016
- **Satyanarayanan (2009):** Cloudlets — IEEE Pervasive Computing, v. 8, n. 4
- **Satyanarayanan (2017):** Edge Computing — IEEE Computer, v. 50, n. 1

### 🔧 Conexão com o projeto

Apps Flutter que usam `geolocator`, `sensors_plus` ou `connectivity_plus` já são sistemas cientes de contexto. Um app que publica dados via MQTT para um broker IoT implementa a arquitetura desta aula de ponta a ponta.
