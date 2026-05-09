# Resumo para Prova — Frameworks Híbridos e Multiplataforma

## Contexto Geral

Desenvolver apps para iOS e Android separadamente custa caro e duplica esforço. Os frameworks multiplataforma resolvem isso de maneiras distintas: Flutter, React Native, Ionic/Capacitor e Kotlin Multiplatform (KMP) são as quatro principais opções do mercado em 2025. Cada um tem uma filosofia diferente sobre onde compartilhar código e como renderizar a interface.

## Principais Conceitos

**React Native e a Nova Arquitetura:** O React Native original usava uma Bridge (ponte) para comunicar JavaScript com o código nativo. Toda mensagem era convertida para JSON e de volta, o que causava lentidão perceptível, especialmente em tarefas pesadas como processar frames de câmera em tempo real. A partir da versão 0.76 (2024), a Bridge foi substituída por quatro componentes. O JSI (JavaScript Interface) permite ao JavaScript manter referências diretas a objetos nativos em C++, sem serialização. O Fabric é o novo renderizador, também em C++, compartilhado entre plataformas. Os TurboModules carregam módulos nativos apenas quando são chamados pela primeira vez (lazy loading), acelerando a inicialização. O Codegen gera automaticamente contratos tipo-seguros entre TypeScript e C++, capturando erros antes de o app rodar. O Hermes, engine padrão desde a versão 0.70, pré-compila JavaScript em bytecode durante o build, reduzindo o tempo de inicialização.

**Ionic e Capacitor:** Funciona de forma completamente diferente: o app é basicamente um site web (HTML, CSS e JavaScript) rodando dentro de uma WebView, que é um navegador invisível embutido no app nativo. O Capacitor faz a ponte entre esse site e as APIs nativas do dispositivo (câmera, GPS, notificações). A vantagem é a baixíssima curva de entrada para equipes front-end e a possibilidade de empacotar um app web existente sem reescrita. A limitação é o teto de performance: animações complexas e processamento em tempo real não são adequados para WebView.

**Kotlin Multiplatform (KMP):** Parte de uma premissa diferente: compartilha apenas a lógica de negócio (repositórios, chamadas de API, banco de dados), enquanto a interface permanece 100% nativa em cada plataforma — SwiftUI no iOS, Jetpack Compose no Android. O mecanismo expect/actual permite declarar uma interface comum e implementá-la de forma diferente por plataforma. O grande diferencial é a adoção gradual: um app nativo existente pode começar a usar KMP em um único módulo, sem reescrita total. A adoção dobrou de 7% para 18% entre 2024 e 2025.

## Diferenças entre os Frameworks

Flutter desenha seus próprios pixels com uma engine gráfica própria, garantindo visual idêntico em todas as plataformas. React Native controla componentes nativos reais via JSI, entregando performance nativa com produtividade de React. Ionic/Capacitor roda um site dentro do app, ideal para portais e conteúdo, mas limitado em performance. KMP compartilha lógica e mantém UI totalmente nativa, sendo o caminho mais conservador para times com apps nativos existentes.

A escolha prática segue o contexto: app web existente que precisa ir para mobile rapidamente escolhe Ionic; time com base em React/JavaScript escolhe React Native; novo projeto sem restrições de linguagem e com foco em performance escolhe Flutter; time Kotlin com apps nativos já em produção escolhe KMP.

## O Que Mais Cai em Prova

O que a Bridge fazia e por que era um problema (serialização JSON, assincronicidade, inicialização pesada). O papel de cada componente da nova arquitetura do React Native: JSI elimina serialização, Fabric renderiza em C++, TurboModules fazem lazy loading, Codegen garante tipos, Hermes pré-compila bytecode. A diferença entre WebView (Ionic) e componentes nativos reais (React Native e KMP). O diferencial do KMP ser adoção gradual sem reescrita. Qual framework escolher para cada perfil de projeto.
