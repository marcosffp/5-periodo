# Resumo para Prova — Desenvolvimento Móvel com Flutter

## Contexto Geral

Flutter é um framework criado pelo Google que permite desenvolver aplicativos para iOS, Android, Web e Desktop a partir de um único código-fonte, escrito em Dart. Antes dele, era necessário manter dois times separados: um para iOS (Swift) e outro para Android (Kotlin). O Flutter elimina essa duplicação ao renderizar a interface diretamente, pixel a pixel, sem depender dos componentes visuais nativos de cada sistema operacional. Um recurso fundamental do dia a dia é o Hot Reload, que aplica alterações no código em menos de 1 segundo, sem reiniciar o app. Em 2023, 46% dos desenvolvedores cross-platform utilizavam Flutter globalmente.

## Principais Conceitos

**Dart e Widgets:** No Flutter, tudo é um widget — texto, botão, padding, imagem. Eles se combinam como blocos. O Dart possui null safety, ou seja, o compilador impede erros por variáveis não inicializadas antes de o app rodar. Para operações de rede, usa-se async/await, que libera a thread principal enquanto aguarda a resposta, evitando travamentos na tela. Widgets com estado interno usam StatefulWidget e setState(); para estado compartilhado entre telas, usa-se o Provider.

**UI Adaptativa:** O Material Design 3 é o design system padrão do Flutter, com suporte automático a tema claro e escuro. Para layouts responsivos, usa-se LayoutBuilder, que adapta a tela conforme o espaço disponível. Para aparência nativa no iOS, existe a biblioteca Cupertino. Acessibilidade é obrigatória: botões precisam de mínimo 48x48 dp, contraste de cor de pelo menos 4,5:1, e ícones sem texto devem usar o widget Semantics com label para leitores de tela (TalkBack/VoiceOver).

**APIs REST:** Chamadas de rede nunca devem bloquear a thread principal. Usa-se o pacote http com async/await para fazer requisições GET/POST. O FutureBuilder exibe o resultado na tela tratando três estados obrigatórios: carregando (indicador de progresso), erro (mensagem e botão de tentar novamente) e sucesso (exibe os dados). Para autenticação, o nível básico é o Bearer Token no header das requisições. OAuth 2.0 com PKCE é necessário para login com provedores externos (Google, GitHub). Biometria usa o pacote local_auth.

**Storage Local:** A escolha do armazenamento depende do tipo de dado. SharedPreferences serve para flags e configurações simples (tema escuro ativo, onboarding visto). flutter_secure_storage deve ser usado para tokens e senhas, pois utiliza o Keychain do iOS e o Keystore do Android com criptografia real. sqflite é o SQLite do Flutter, indicado para dados relacionais como listas e históricos.

## Diferenças entre Tecnologias

| Dado | Use | Nunca use |
|---|---|---|
| Configurações simples | SharedPreferences | — |
| Token de autenticação | flutter_secure_storage | SharedPreferences |
| Dados relacionais | sqflite / drift | SharedPreferences |

## Segurança e LGPD

O OWASP Mobile Top 10 2024 lista os principais riscos. Os três mais comuns para iniciantes: M9 (armazenamento inseguro — token em SharedPreferences), M5 (comunicação insegura — usar http:// em vez de https://) e M3 (autenticação insegura — client_secret hardcoded no código-fonte, tokens sem expiração). A regra é: sempre HTTPS, token em secure_storage, segredos apenas no backend.

A LGPD (Lei 13.709/2018) exige consentimento explícito antes de coletar dados, política de privacidade acessível, coleta mínima de dados e botão real de exclusão de conta. Multas chegam a 2% do faturamento anual, com teto de R$ 50 milhões por infração.

## O Que Mais Cai em Prova

Diferença entre StatelessWidget e StatefulWidget; funcionamento do Hot Reload; null safety e o operador ??; os três estados do FutureBuilder; qual storage usar para cada tipo de dado; os erros clássicos de segurança (token em SharedPreferences, uso de http://, segredo no código); direitos do usuário pela LGPD (acesso, correção, exclusão, portabilidade, revogação de consentimento) e penalidades.