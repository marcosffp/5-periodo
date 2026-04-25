# RESUMO — Arquitetura de Software: Sistema de Leilão Online (Aula 14)

O desafio desta aula consiste em projetar a arquitetura de um sistema de leilão online de veículos apreendidos para o DETRAN-MG. O objetivo é substituir processos manuais e fragmentados por uma plataforma digital única, transparente e auditável, chamada Leilão MG.

## Funcionamento do sistema

Os veículos são organizados em lotes, cada um com informações como marca, modelo, ano, condição (Conservado, Sucata ou Média Monta), valor inicial e incremento mínimo. Durante o leilão, usuários cadastrados enviam lances em tempo real. O sistema valida cada lance — verificando se supera o atual e respeita o incremento mínimo — e atualiza os dados para todos os participantes quase instantaneamente. Se um lance for enviado nos últimos 60 segundos, o tempo do lote é automaticamente prorrogado por mais 60 segundos, podendo ocorrer várias vezes. Ao encerrar, o vencedor é declarado, notificado por e-mail e o resultado é registrado para auditoria.

## Requisitos Funcionais (RF)

O sistema precisa oferecer: cadastro de usuários e arrematantes, login integrado ao Gov.br, gerenciamento de leilões e lotes, envio e validação de lances, encerramento automático de lotes, emissão de carta e boleto de arrematação, e envio de e-mail ao vencedor.

## Requisitos Não Funcionais (RNF)

Desempenho: lances registrados sem latência, com atualizações em tempo real. Disponibilidade: redundância nos momentos críticos do leilão. Segurança: lances validados com base no valor exato exibido ao usuário. Escalabilidade: suporte a até 8.000 usuários simultâneos, com escalonamento vertical e testes de carga. Auditoria: registro completo de lances e arrematantes por edital.

## Requisitos Arquiteturalmente Significativos (RAS)

Os três pontos que mais impactam as decisões de arquitetura são: centenas de lances simultâneos, milhares de visualizações ao mesmo tempo e alta disponibilidade. Esses fatores exigem comunicação em tempo real e cache eficiente.

## Protocolos e Tecnologias

HTTP puro não é suficiente para tempo real, pois apenas o cliente inicia requisições. O WebSocket resolve isso: após um handshake inicial via HTTP, a conexão se torna bidirecional e permanente, permitindo que o servidor envie atualizações sem o cliente precisar perguntar.

## Arquitetura proposta

Front-end em Vue.js (SPA) comunica-se via HTTP e WebSocket com um API Gateway. O back-end usa .NET com SignalR (biblioteca que gerencia WebSockets). O Redis Cache armazena temporariamente dados voláteis como maior lance atual e estado do lote. O SQL Server persiste dados permanentes (usuários, lances, logs). Arquivos como fotos e PDFs ficam no Azure Blob Storage. O envio de e-mails usa um sistema de filas (Azure Service Bus) com Azure Functions consumindo as mensagens e disparando e-mails via Simple Email Service (AWS).

## O que mais cai em prova

Saber diferenciar HTTP de WebSocket; entender por que WebSocket é necessário para tempo real; identificar corretamente RF, RNF e RAS; e compreender o papel de cada componente da arquitetura (cache, banco, fila de mensagens, armazenamento de arquivos).
