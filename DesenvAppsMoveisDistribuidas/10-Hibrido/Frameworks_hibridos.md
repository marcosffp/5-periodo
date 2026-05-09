# Frameworks Híbridos e Multiplataforma — PUC Minas

**Disciplina:** Engenharia de Software · ICEI — Aplicações Móveis e Distribuídas · 2026/1  
**Professor:** Cristiano de Macedo Neto  
**Aula:** 11 (Continuação da Aula 10 · Comparativo com Flutter · Decisão arquitetural)

---

## Mapa da Aula

| Bloco | Conteúdo | Slides |
|---|---|---|
| I | React Native — Nova Arquitetura | 2–4 |
| II | Ionic & Capacitor | 5–6 |
| III | Kotlin Multiplatform | 7–8 |
| IV | Comparativo e Decisão | 9–11 |

### 🎯 Nesta aula você vai:

- Entender por que o React Native substituiu a bridge por JSI, Fabric e TurboModules
- Distinguir a abordagem WebView do Ionic/Capacitor e quando ela é vantajosa
- Compreender o modelo "lógica compartilhada, UI nativa" do Kotlin Multiplatform
- Comparar os quatro frameworks em critérios objetivos e tomar uma decisão de projeto

> **"With 0.76, the New Architecture is enabled by default in all React Native projects."**  
> — React Native Team · reactnative.dev · React Native 0.76, outubro 2024

---

## I. React Native — O problema que a Bridge criava

O React Native foi lançado pelo Facebook em 2015 com uma ideia poderosa: escrever interfaces mobile com React e JavaScript. A implementação original, porém, criava um gargalo estrutural que limitava a performance por quase uma década.

### Arquitetura antiga — a Bridge

```
Thread JavaScript
  (Executa o código React)
        ↕ serializava JSON ↕
Bridge (assíncrona)
  (Toda comunicação passava aqui)
        ↕ deserializava JSON ↕
Thread Nativa (UI)
  (Componentes iOS / Android reais)
```

### 🔴 Os três problemas da Bridge

- **Serialização:** cada mensagem JS↔nativo era convertida para JSON e de volta. Dados grandes (ex: frames de câmera de ~30 MB) eram proibitivos.
- **Assincronicidade:** não havia chamadas síncronas. Qualquer operação que precisasse de resposta imediata tinha latência perceptível.
- **Inicialização:** todos os módulos nativos eram carregados na inicialização, mesmo os que nunca seriam usados.

### 📷 VisionCamera com a arquitetura antiga

Uma biblioteca de câmera que processa frames em tempo real precisava enviar cada frame (~30 MB) pelo bridge como JSON. A ~30 FPS, isso equivalia a **900 MB de dados por segundo** sendo serializados e desserializados. Era impraticável.

Com a nova arquitetura (JSI), o JavaScript mantém uma **referência direta** ao objeto C++ do frame — sem cópia, sem serialização.

### ⚠️ O problema era estrutural

Não era um bug que uma atualização resolveria. A Bridge era a fundação do framework. O React Native precisava ser reescrito desde os alicerces — e foi. O projeto durou de 2018 a 2024.

### 📊 Benchmark Meta (2024)

Cold start em Android intermediário: arquitetura antiga 3.200 ms, nova arquitetura com Hermes + TurboModules: melhoria significativa.  
Fonte: engineering.fb.com/2024/12/18/android/react-native-new-architecture/

> **Referências:** React Native New Architecture: reactnative.dev/architecture/landing-page — "With 0.76, the New Architecture is enabled by default." | Facebook Engineering: engineering.fb.com/2024/12/18/android/react-native-new-architecture/ | JSI/VisionCamera: "Typical frame buffers are ~30 MB, which amounts to roughly 2 GB of data per second."

---

## I. React Native — JSI, Fabric, TurboModules e Codegen

A nova arquitetura substitui a Bridge por quatro componentes interligados. Juntos, eliminam a serialização e permitem comunicação síncrona direta entre JavaScript e código nativo em C++.

### ⚡ JSI — JavaScript Interface

Camada em C++ que permite ao JavaScript **manter referências diretas a objetos nativos**. Sem serialização JSON. O JS pode chamar métodos nativos de forma síncrona, como se fossem funções locais. Funciona com qualquer engine JS (Hermes, V8, JavaScriptCore).

```
Antigo: JS → [JSON] → Bridge → [JSON] → Nativo
Novo:   JS → [referência direta C++] → Nativo
```

### 🖼️ Fabric — novo renderizador

Substitui o UIManager. É um renderizador C++ compartilhado entre plataformas. Mantém as três threads (JS, Shadow, Native UI) mas conecta tudo via JSI. Cada atualização de UI cria uma nova versão imutável da Shadow Tree — renderização concorrente sem travamentos.

### 📦 TurboModules — módulos sob demanda

Substitui os NativeModules da arquitetura antiga. A diferença principal: **lazy loading**. Módulos são carregados apenas quando chamados pela primeira vez. Um app que usa câmera, geolocalização e Bluetooth não mais carregava os três no boot — carrega cada um quando necessário.

- Startup mais rápido
- Menor uso de memória inicial
- Comunicação síncrona via JSI

### 🔐 Codegen — segurança de tipos

Ferramenta que gera automaticamente as ligações (bindings) tipo-seguras entre TypeScript/Flow e C++. O desenvolvedor define a interface em TypeScript; o Codegen produz os contratos C++ em build time. Erros de tipo são capturados antes de rodar, não em produção.

### 🎯 Hermes — engine padrão

O React Native usa o Hermes como engine JS padrão desde 0.70. Otimizado para mobile: pré-compila JavaScript em bytecode durante o build (não em runtime), reduzindo tempo de parse na inicialização.

> **Referências:** React Native New Architecture: reactnative.dev/architecture/landing-page | DEV Community: "New architecture comprises of Fabric, Turbo Native Modules, Codegen, and JSI" (dev.to/amazonappdev, fev 2025) | CodeMiner42: "Fabric is a C++ renderer shared across platforms, connected to JS through JSI" (blog.codeminer42.com, out 2025) | TurboModules lazy loading: "Modules are loaded only when first used, improving startup time" (reactnative.dev).

---

## I. React Native — Como é programar em React Native

React Native usa **JavaScript ou TypeScript** com a sintaxe do React — mas os componentes renderizam UI nativa da plataforma, não elementos HTML. Quem já conhece React para web tem a curva de aprendizado mais curta.

### Componentes nativos, sintaxe React

```typescript
// ListaUsuarios.tsx
import { useState, useEffect } from 'react';
import { View, Text, FlatList, StyleSheet }
  from 'react-native';

export default function ListaUsuarios() {
  const [usuarios, setUsuarios] =
    useState<Usuario[]>([]);

  useEffect(() => {
    // fetch roda em thread JS, não bloqueia UI
    fetch('https://api.exemplo.com/users')
      .then(r => r.json())
      .then(setUsuarios);
  }, []);

  return (
    <FlatList
      data={usuarios}
      keyExtractor={u => u.id.toString()}
      renderItem={({ item }) => (
        {/* View e Text = componentes nativos reais */}
        <View style={styles.item}>
          <Text style={styles.nome}>
            {item.nome}
          </Text>
        </View>
      )}
    />
  );
}
```

`View` vira `UIView` no iOS e `android.view.View` no Android. O usuário vê componentes nativos reais — não WebView.

### Posicionamento no mercado

**📊 Quem usa React Native (2025):** **35%** dos devs cross-platform (Stack Overflow 2024). Adotado por Meta, Microsoft, Shopify, Discord, Wix. É a segunda opção mais usada, atrás do Flutter.

**✅ Quando React Native é a escolha certa:**
- Time com forte experiência em React/JavaScript
- App precisa de UI que respeite as convenções de cada plataforma (gestos iOS, Material Android)
- Integrações profundas com APIs nativas específicas
- Acesso a um ecossistema JS maduro (npm, bibliotecas React)

**⚠️ Atenção na migração:** Muitas bibliotecas de terceiros ainda não foram migradas para a nova arquitetura. O site **reactnative.directory** tem filtro por compatibilidade. Projetos legados em RN < 0.76 precisam de migração cuidadosa.

> **Referências:** Stack Overflow Developer Survey 2024: 35% React Native entre frameworks cross-platform. | React Native Directory: reactnative.directory/?newArchitecture=true | Hermes como engine padrão: React Native 0.70+. | "Learn once, write anywhere" — slogan histórico do React Native diferenciando de "write once, run anywhere" do Flutter.

---

## II. Ionic & Capacitor — A abordagem WebView

### Como funciona

```
Seu app web
  (HTML + CSS + JavaScript — React, Vue, Angular…)
        ↓
WebView
  (Navegador invisível dentro do app nativo)
        ↕ plugins
Capacitor Runtime
  (Ponte para APIs nativas — câmera, GPS, arquivos…)
        ↓
iOS / Android / Web / Desktop
  (Mesmo código, quatro plataformas)
```

**💡 A intuição correta:** Imagine seu site sendo embutido dentro de um app nativo. O Capacitor é o "invólucro" nativo que dá ao seu site acesso à câmera, GPS e notificações push. O usuário instala da App Store, mas dentro está um site rodando num browser invisível.

### 🎨 Ionic Framework

Biblioteca de componentes UI (100+ componentes) que se adaptam automaticamente ao estilo da plataforma: usa Cupertino no iOS, Material Design no Android. Compatível com React, Vue, Angular ou JavaScript puro.

### ⚙️ Capacitor (criado em 2018)

Substitui o Cordova/PhoneGap com uma API mais moderna. Mais de **1.100 plugins** para funcionalidades nativas. Em 2022 o Ionic foi adquirido pela OutSystems; Ionic Framework e Capacitor continuam gratuitos e mantidos ativamente.

### ✅ Quando é a escolha certa

- Equipe com perfil front-end web (HTML/CSS/JS)
- Já existe um app web que precisa ir para mobile rapidamente
- App de conteúdo, formulários ou portal corporativo
- Precisa rodar também como PWA ou desktop

### ⚠️ Limite claro de performance

Animações complexas, jogos, câmera em tempo real e processamento intensivo não são adequados. A WebView tem um teto de performance que não compete com renderização nativa.

> **Referências:** Capacitor: criado pela equipe Ionic em 2018 como alternativa ao Cordova. Ionic adquirido pela OutSystems em 2022; Ionic Framework e Capacitor permanecem gratuitos (Medium/Haskett, dez 2025). Capacitor 8: suporte edge-to-edge Android, Swift Package Manager para iOS (metacto.com, mar 2026). Mais de 1.100 plugins: ionicframework.com/docs/native. | "Ionic has 5+ million developers and over 20% of apps in app stores" (Medium/Haskett, dez 2025).

---

## II. Ionic & Capacitor — Na prática: código e decisão

### Acessando câmera com Capacitor

```typescript
// foto.ts
import { Camera, CameraResultType }
  from '@capacitor/camera';

async function tirarFoto() {
  // Plugin Capacitor chama API nativa
  const foto = await Camera.getPhoto({
    quality: 90,
    allowEditing: false,
    resultType: CameraResultType.Uri,
  });

  // foto.webPath é uma URL usável no <img>
  imageElement.src = foto.webPath;
}

// No HTML do seu app web:
// <button onclick="tirarFoto()">Foto</button>
// <img id="imageElement" />
```

**💡 A vantagem real:** Se você tem uma equipe de front-end que já conhece TypeScript e React (ou Vue/Angular), esse código é imediatamente familiar. Não há nova linguagem, não há conceito de widget ou composable a aprender. **O tempo até o primeiro app funcionando é mínimo.**

### Comparativo Ionic vs React Native

| Critério | Ionic/Capacitor | React Native |
|---|---|---|
| UI renderizada por | WebView (browser) | Componentes nativos reais |
| Performance UI | Boa para conteúdo | Nativa |
| Linguagem | HTML/CSS/JS — qualquer framework web | JavaScript/TypeScript + JSX |
| Plataformas | iOS, Android, Web, Desktop | iOS, Android (Web limitado) |
| Time ideal | Perfil front-end web | Perfil React/JS mobile |
| App existente na web? | Sim, fácil de embalar | Precisa reescrever a UI |
| Animações complexas | Limitado | Suportado (Reanimated 3) |

### 🔴 Quando NÃO usar Ionic

- App com animações fluidas a 60 FPS
- Processamento de câmera em tempo real
- Jogos ou experiências imersivas
- Apps onde a "sensação nativa" é requisito

> **Referências:** Capacitor Camera plugin: capacitorjs.com/docs/apis/camera | "If you're trying to bring an existing web application to mobile, there really are only two viable options: Capacitor and Cordova" — ionic.io | Comparativo: capgo.app/blog/comparing-react-native-vs-capacitor; nextnative.dev/blog/capacitor-vs-react-native

---

## III. Kotlin Multiplatform — Compartilhe a lógica, mantenha a UI nativa

O Kotlin Multiplatform (KMP) parte de uma premissa diferente dos outros frameworks: não tenta resolver tudo com uma só abordagem. A proposta é compartilhar apenas o que faz sentido compartilhar — a lógica de negócio — e deixar a UI completamente nativa em cada plataforma.

### A arquitetura "lógica compartilhada, UI nativa"

```
UI iOS (SwiftUI nativo) | UI Android (Jetpack Compose nativo)
─────────────────────────────────────────────────────────────
       Módulo Kotlin Compartilhado (KMP)
  Lógica de negócio · Repositórios · APIs · Banco de dados
─────────────────────────────────────────────────────────────
iOS: compila via LLVM        |  Android: JVM/Kotlin
(Binário nativo ARM)         |  (Bytecode padrão)
```

### 💡 O mecanismo expect/actual

KMP usa palavras-chave `expect` (declaração comum) e `actual` (implementação por plataforma). Você declara a interface uma vez e implementa diferente onde necessário:

```kotlin
expect fun currentTimestamp(): Long
// iOS: actual usa NSDate
// Android: actual usa System.currentTimeMillis()
```

### Por que é diferente dos outros

**🎯 Adoção gradual — o diferencial:** Não é uma reescrita total. Um app Android existente pode começar a compartilhar um único módulo (ex: camada de networking) com o iOS. A adoção cresce conforme o time ganha confiança. Sem necessidade de descartar o código nativo existente.

**📈 Crescimento acelerado:** Adoção mais que dobrou: de **7% em 2024** para **18% em 2025** no Developer Ecosystem Survey da JetBrains. Google anunciou suporte oficial no Google I/O 2024. Compose Multiplatform (UI compartilhada) estabilizou para iOS em maio de 2025.

**🏢 Quem usa em produção:** Pinterest, Airbnb, Netflix, Slack, Forbes, McDonald's e Cash App adotaram KMP para compartilhar lógica entre plataformas, mantendo UI nativa em cada uma.

> **Referências:** KMP: JetBrains — kotlinlang.org/docs/multiplatform | Adoção: "KMP usage more than doubled... from 7% in 2024 to 18% in 2025" — JetBrains Developer Ecosystem Survey 2025. | Google I/O 2024: suporte oficial Google para KMP. | Compose Multiplatform iOS estável: maio 2025. | Promovido a Stable: novembro 2023.

---

## III. Kotlin Multiplatform — Na prática: código compartilhado

### Módulo compartilhado — repositório de dados

```kotlin
// UsuarioRepository.kt (commonMain)
// Este arquivo compila para iOS E Android
class UsuarioRepository(
    private val api: UsuarioApi,
    private val db: Database,
) {
    suspend fun buscarUsuarios(): List<Usuario> {
        return try {
            val remotos = api.listar()
            db.salvar(remotos)  // cache local
            remotos
        } catch (e: Exception) {
            // sem rede: retorna do cache
            db.listar()
        }
    }
}

// No Android: usa normalmente com ViewModel
// No iOS: exposto como Swift async/await
```

### Compose Multiplatform — UI compartilhada opcional

```kotlin
// TelaUsuarios.kt (UI compartilhada)
// Compose Multiplatform: mesma UI
// em Android E iOS (estável desde mai/2025)
@Composable
fun TelaUsuarios(viewModel: UsuariosViewModel) {
    val usuarios by viewModel.usuarios
        .collectAsState()

    LazyColumn {
        items(usuarios) { usuario ->
            Text(text = usuario.nome)
        }
    }
}
```

### Estratégias de compartilhamento

- **Só lógica:** compartilha repositórios, APIs, banco. UI 100% nativa por plataforma
- **Lógica + UI com Compose MP:** uma única codebase para tudo, UI idêntica
- **Gradual:** começa com um módulo e expande

### ⚠️ Curva de aprendizado

Requer conhecimento de Kotlin e — para UI nativa de verdade — SwiftUI no iOS. Sem hot reload na parte Swift/SwiftUI. Tooling mais jovem que Flutter ou React Native. Recomendado para times com base em Kotlin/Android.

> **Referências:** Kotlin Multiplatform: kotlinlang.org/docs/multiplatform | expect/actual: kotlinlang.org/docs/multiplatform-expect-actual | Compose Multiplatform: jb.gg/compose-multiplatform — iOS estável desde maio 2025. | SQLDelight e Ktor são bibliotecas KMP populares. | "No hot reload can slow down UI iteration compared to React Native or Flutter" (metacto.com).

---

## IV. Decisão Arquitetural — Comparativo: Flutter vs RN vs Ionic vs KMP

Os quatro frameworks resolvem o mesmo problema de maneiras fundamentalmente diferentes. Nenhum é superior em todos os critérios — a decisão depende do contexto do projeto e do time.

| Critério | Flutter | React Native | Ionic/Capacitor | Kotlin Multiplatform |
|---|---|---|---|---|
| UI renderizada por | Engine própria (pixels diretos) | Componentes nativos reais | WebView (browser) | 100% nativa por plataforma |
| Linguagem | Dart | JavaScript / TypeScript | HTML/CSS/JS — qualquer framework | Kotlin (+ Swift para UI iOS) |
| Consistência visual entre plataformas | Pixel-a-pixel idêntico | Varia por componente nativo | Alta (CSS unificado) | Baixa (UI 100% nativa) |
| Performance | ~0,84× nativo | Nativa (com nova arquitetura) | Boa para conteúdo | 100% nativa |
| Adoção (devs, 2024) | 46% | 35% | 5–8M devs / 20% apps store | 18% (crescendo) |
| Curva de aprendizado | Dart (nova linguagem) | JS/TS (familiar para web devs) | Mínima (HTML/CSS/JS) | Alta (Kotlin + nativo iOS) |
| Adoção gradual em projeto existente | Difícil (reescrita total) | Difícil (reescrita total) | Médio (embala app web) | Excelente (módulo a módulo) |
| Mantido por | Google | Meta + Microsoft | OutSystems (open source) | JetBrains + Google |

> **Referências:** Flutter 46% vs RN 35%: Stack Overflow Developer Survey 2024 / Statista 2023. | KMP 18%: JetBrains Developer Ecosystem Survey 2025. | Ionic: "5+ million developers and over 20% of apps in app stores" (Haskett, dez 2025). | Flutter ~0.84× nativo: University of Amsterdam Cross-Platform Study, 2025. | RN nativa com nova arquitetura: reactnative.dev/architecture/landing-page.

---

## IV. Decisão Arquitetural — Como decidir: perguntas que direcionam a escolha

A escolha de framework é uma decisão de engenharia — não de preferência pessoal. Cada pergunta abaixo elimina opções inadequadas para o contexto.

### 🔍 Já existe um app web que vai para mobile?

- **Sim** → considere **Ionic/Capacitor**. É a única opção que embala um app web existente sem reescrita.
- **Não** → continue avaliando.

### 🔍 O time tem base sólida em Android/Kotlin?

- **Sim** → **KMP** é candidato natural, especialmente para apps com lógica complexa que precisam de UI verdadeiramente nativa.
- **Não** → continue avaliando.

### 🔍 O time vem do ecossistema React/JavaScript?

- **Sim** → **React Native**. Menor curva de aprendizado, maior reaproveitamento de conhecimento.
- **Novo projeto, time sem restrições?** → **Flutter**. Melhor balanço de performance, consistência visual e ecossistema maduro.

### Resumo por perfil de projeto

**🎯 Flutter:** Novo projeto mobile, performance e consistência visual são prioridade, time disposto a aprender Dart. MVP que pode crescer para produto maduro.

**🎯 React Native:** Time React/JS experiente, app que precisa de componentes com look and feel da plataforma específica, integração profunda com APIs nativas.

**🎯 Ionic/Capacitor:** App de conteúdo, portal corporativo ou formulários. Time front-end web. App web existente que precisa ir para a App Store rapidamente.

**🎯 Kotlin Multiplatform:** Apps nativas Android e iOS existentes com lógica duplicada. Time Kotlin forte. Necessidade de UI rigorosamente nativa com código compartilhado por baixo.

> **Referências:** Haskett, A. "2025 Cross-Platform Mobile Development Frameworks: Technical Comparison" (Medium, dez 2025): "Flutter provides the best balance of performance, code sharing, and platform coverage for teams starting fresh. React Native remains the safest enterprise choice. KMP suits teams with existing native apps seeking gradual adoption." | Ionic: "Web teams should seriously consider Ionic/Capacitor when simultaneous web deployment matters alongside mobile."

---

## Checkpoint — Verifique seu entendimento

### 🎯 Questão 1 — A nova arquitetura do React Native

Uma biblioteca de câmera para React Native precisa processar frames de vídeo em tempo real. Com a arquitetura antiga (Bridge), isso era impraticável. Qual componente da nova arquitetura resolve esse problema e de que forma?

- A) O Fabric, pois é um renderizador C++ que processa frames diretamente no Shadow Thread sem passar pela thread JavaScript.
- **B) O JSI (JavaScript Interface), pois permite ao JavaScript manter referências diretas a objetos C++ — sem serializar cada frame para JSON. Um frame de ~30 MB é acessado por referência de memória, não copiado. ✅**
- C) O Hermes, pois pré-compila o JavaScript para bytecode, reduzindo o custo de processamento de cada frame em runtime.
- D) Os TurboModules, pois carregam o módulo de câmera de forma lazy apenas quando necessário, liberando CPU para o processamento de frames.

**Resposta:** O JSI resolve exatamente o problema de serialização. Na arquitetura antiga, cada frame de câmera precisava ser convertido para JSON para atravessar a Bridge. Com JSI, o JavaScript recebe uma referência direta ao objeto C++ que contém o frame — sem cópia, sem conversão. A documentação oficial cita o VisionCamera como o caso de uso que tornou o JSI indispensável: frames de ~30 MB a 30 FPS seriam 900 MB/s de serialização pela Bridge, o que era impraticável.

---

### 🎯 Questão 2 — Ionic/Capacitor vs React Native

Uma empresa tem um portal corporativo web construído em Angular que funciona bem. O cliente pede uma versão mobile. Qual framework resolve isso com menor custo e por quê?

- A) Flutter, pois tem a melhor performance entre as opções e o cliente vai perceber a diferença na velocidade da aplicação.
- B) React Native, pois usa JavaScript como o Angular e a migração de código seria mais direta.
- **C) Ionic/Capacitor, pois é a única opção que embala um app web existente sem reescrita da UI. O código Angular existente roda dentro do WebView; o Capacitor adiciona acesso às APIs nativas necessárias. ✅**
- D) Kotlin Multiplatform, pois permite compartilhar a lógica de negócio do portal com as versões iOS e Android sem duplicação.

**Resposta:** O contexto decisivo aqui é que o app web existente já funciona bem em Angular. Flutter e React Native exigiriam reescrever toda a interface do zero em suas próprias linguagens e componentes. KMP compartilha lógica Kotlin, não apps web Angular. Ionic/Capacitor é a única opção que embala o app web existente sem reescrita — é exatamente o caso de uso para o qual o Capacitor foi criado.

---

### 🎯 Questão 3 — Kotlin Multiplatform

Uma empresa tem apps nativos Android (Kotlin) e iOS (Swift) em produção há anos. O mesmo código de regras de negócio foi duplicado nas duas plataformas e está gerando inconsistências. Qual estratégia o KMP permite que os outros frameworks não permitem?

- A) Reescrever ambos os apps do zero em Kotlin, compartilhando 100% do código inclusive a UI em Compose Multiplatform.
- **B) Extrair gradualmente apenas a lógica de negócio para um módulo KMP compartilhado, mantendo as UIs nativas existentes (SwiftUI e Jetpack Compose) intactas. A adoção é incremental, sem reescrita total. ✅**
- C) Usar o mecanismo expect/actual para substituir as chamadas de API nativas por uma camada de abstração comum em JavaScript.
- D) Compartilhar a camada de UI entre as plataformas usando React componentes que compilam para SwiftUI no iOS e Jetpack Compose no Android.

**Resposta:** A reescrita total (alternativa A) é exatamente o que o KMP evita — seria o mesmo problema de custo que Flutter ou React Native. O mecanismo expect/actual é Kotlin, não JavaScript (alternativa C). Não existe compilação de React para SwiftUI ou Jetpack Compose (alternativa D). A resposta correta é B: a adoção gradual módulo a módulo, mantendo UIs nativas existentes, é o diferencial que justifica o KMP em projetos com apps nativos já em produção.

---

## Síntese — O que levar desta aula

### Os quatro modelos em uma frase cada

**Flutter:** Um codebase em Dart que desenha seus próprios pixels — consistência visual total entre plataformas sem depender de componentes do SO.

**React Native (0.76+):** JavaScript/TypeScript que controla componentes nativos reais via JSI — sem bridge, sem serialização, performance de app nativo com produtividade de React.

**Ionic/Capacitor:** Seu site web rodando dentro de um app nativo — a menor barreira de entrada para equipes front-end que precisam de distribuição mobile.

**Kotlin Multiplatform:** Compartilhe a lógica, mantenha a UI nativa — a aposta mais conservadora para times que não querem abrir mão da experiência nativa em nenhuma plataforma.

### Referências para ir além

- **React Native:** reactnative.dev/architecture/landing-page
- **JSI/Fabric/TurboModules:** reactnative.dev/architecture/overview
- **Capacitor:** capacitorjs.com/docs
- **Ionic Framework:** ionicframework.com/docs
- **KMP:** kotlinlang.org/docs/multiplatform
- **Compose Multiplatform:** jb.gg/compose-multiplatform
- **Comparativo atualizado:** React Native Directory — reactnative.directory

### 🎓 Perspectiva para o projeto

Para os projetos desta disciplina, **Flutter** é a recomendação padrão: ecossistema maduro, documentação excelente, hot reload, e não requer conhecimento prévio de mobile. React Native é uma alternativa válida para quem tem base sólida em React. Ionic resolve rapidamente se vocês já têm um app web funcionando.

---

### 🚀 Próxima Aula — Computação Ubíqua e IoT

De Weiser (1991) ao 5G e à Internet das Coisas — como conectar bilhões de dispositivos e o que muda no desenvolvimento de aplicações distribuídas.
