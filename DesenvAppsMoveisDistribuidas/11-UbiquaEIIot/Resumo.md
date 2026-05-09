# Resumo para Prova — Computação Ubíqua, Contexto e IoT

## Contexto Geral

Em 1991, Mark Weiser publicou uma visão revolucionária: em vez de o usuário entrar no mundo do computador, o computador deveria entrar no mundo do usuário, tornando-se invisível na experiência cotidiana. Essa ideia, chamada computação ubíqua, é o ponto de partida de tudo que está nesta aula — de sensores IoT a processamento na borda da rede.

## Principais Conceitos

**Computação Ubíqua e Calm Technology:** Weiser propôs três form factors: Tabs (escala de centímetros, análogos aos sensores IoT e wearables hoje), Pads (escala de folha A4, análogos a smartphones e tablets) e Boards (escala de parede, análogos a smart TVs e displays públicos). Em 1996, Weiser e Brown aprofundaram o conceito com a Calm Technology: tecnologia que opera na periferia da atenção humana, informando sem interromper e agindo sem exigir foco. O ABS do carro é o exemplo clássico — o motorista não percebe a intervenção do sistema. Ambient Intelligence é a evolução atual: ambientes que percebem, aprendem e respondem proativamente, usando sensores, atuadores, modelos de ML e interfaces naturais como voz e gesto.

**Sistemas Cientes de Contexto:** Definidos por Dey (2001), são sistemas que adaptam o comportamento com base na situação do usuário. Os quatro tipos primários de contexto são: localização (onde está, via GPS ou beacons), identidade (quem é, suas preferências e histórico), atividade (o que está fazendo, detectado por acelerômetro e giroscópio) e tempo (hora, dia da semana, estação). O pipeline de um sistema ciente de contexto passa por coleta de sensores, interpretação das informações e adaptação do comportamento. Em Flutter, os pacotes geolocator, sensors_plus e connectivity_plus implementam esse pipeline. A LGPD exige minimização: coletar apenas o contexto que o app realmente usa.

**Internet das Coisas (IoT):** O termo foi cunhado por Kevin Ashton em 1999 ao propor conectar etiquetas RFID à internet para rastrear estoque. Hoje existem cerca de 18 bilhões de dispositivos conectados, com projeção de 29 bilhões até 2030. A arquitetura IoT tem três camadas: a camada de percepção (sensores, atuadores, microcontroladores como ESP32), a camada de rede/middleware (gateways, brokers MQTT, plataformas cloud) e a camada de aplicação (dashboards, analytics, alertas). Dispositivos da camada inferior têm recursos limitados e não suportam HTTP completo, por isso existem protocolos específicos.

**MQTT e CoAP:** O MQTT, criado em 1999 pela IBM, usa o modelo publish/subscribe com broker centralizado sobre TCP. Tem header de apenas 2 bytes e três níveis de qualidade de serviço (QoS 0, 1 e 2). Sua principal vantagem é a sessão persistente: o broker retém mensagens enquanto o dispositivo está offline e entrega quando reconecta. É ideal para telemetria contínua. O CoAP, definido pelo RFC 7252 de 2014, usa o modelo request/response sobre UDP, sem broker. Tem os mesmos métodos REST (GET, POST, PUT, DELETE) e é ideal para consultas pontuais em redes locais. A diferença central: MQTT garante entrega em conexões intermitentes; CoAP é mais leve para leituras esporádicas sem necessidade de sessão.

**Edge, Fog e Cloud:** A nuvem tem latência de 50 a 200 ms e depende de conexão constante, o que é inaceitável para controle em tempo real. O Fog Computing (proposto pela Cisco em 2012) é o processamento distribuído entre dispositivos e nuvem, com latência de 10 a 50 ms, agregando e filtrando dados antes de enviar. O Edge Computing, proposto por Satyanarayanan (CMU) em 2009, processa no próprio dispositivo ou imediatamente ao lado, com decisões abaixo de 10 ms sem depender de rede. O princípio fundamental é: não subir para a nuvem o que pode ser decidido na borda.

## Diferenças entre MQTT e CoAP

MQTT usa TCP, tem broker centralizado, suporta sessões persistentes e QoS, e é ideal para sensores que ficam offline. CoAP usa UDP, não precisa de broker, é um protocolo request/response direto e é ideal para consultas rápidas em rede local.

## O Que Mais Cai em Prova

Os três form factors de Weiser e seus análogos atuais. A definição de Calm Technology e o exemplo do ABS. Os quatro tipos primários de contexto de Dey (localização, identidade, atividade, tempo). As diferenças entre MQTT e CoAP: modelo pub/sub vs request/response, TCP vs UDP, com e sem broker, suporte a sessão persistente. A diferença entre Edge (menos de 10 ms, no dispositivo), Fog (10 a 50 ms, rede local) e Cloud (50 a 200 ms, análise histórica). Quando usar cada camada de processamento com base na latência exigida.