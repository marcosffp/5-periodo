# Desenvolvimento Móvel com Flutter

**PUC Minas — ICEI — Aplicações Móveis e Distribuídas**  
**Aula 10 · Engenharia de Software · 2026/1**  
**Professor:** Cristiano de Macedo Neto

---

## Sumário

- [I. Flutter + Dart](#i-flutter--dart) — Slides 2–3
- [II. UI Adaptativa](#ii-ui-adaptativa) — Slides 4–5
- [III. APIs REST](#iii-apis-rest) — Slides 6–7
- [IV. Storage Local](#iv-storage-local) — Slides 8–9
- [V. Segurança + LGPD](#v-segurança--lgpd) — Slides 10–11

---

> *"O melhor framework é aquele que permite ao time entregar valor com confiança — não necessariamente o mais sofisticado."*  
> — Princípio de escolha de tecnologia em engenharia de software

---

## 🎯 Objetivos da Aula

- Entender **por que** o Flutter existe e qual problema ele resolve
- Conhecer a estrutura básica de um app Flutter — widgets, estado e Dart
- Buscar dados de uma API REST e exibi-los na tela
- Guardar dados no dispositivo de forma simples e segura
- Identificar os principais erros de segurança de iniciantes — e como evitá-los

---

## I. Flutter + Dart

### Por que Flutter existe?

**🤔 O problema: dois times para o mesmo produto**

Um app nativo para iOS é escrito em **Swift**. Para Android, em **Kotlin**. São linguagens, SDKs e ferramentas completamente diferentes. Na prática: **dois times, dois codebases, dois cronogramas** para entregar o mesmo produto. Uma feature nova precisa ser implementada duas vezes. Um bug corrigido em um lado pode existir no outro. O custo dobra.

**A solução: um codebase, duas plataformas**

```
Seu código Dart
(Widgets · Lógica · Estado · APIs)
        ↓
Flutter Engine
(Renderiza pixels diretamente, sem tradução para UI nativa)
        ↓
iOS  |  Android  |  Web  |  Desktop
```

> 💡 **O que "renderiza pixels diretamente" significa?**  
> A maioria dos frameworks traduz o seu código para componentes nativos do SO (botões do iOS, caixas de texto do Android). O Flutter **pula essa etapa** e desenha cada pixel da tela por conta própria, usando uma engine gráfica. Resultado: a UI é idêntica em todas as plataformas, independente de como cada SO renderiza seus próprios componentes.

### Hot Reload — o maior ganho do dia a dia

| Antes | Depois |
|---|---|
| ⏱️ 30–60 segundos por mudança — compilar, assinar e instalar o APK no emulador a cada alteração | ⚡ < 1 segundo com Hot Reload — injeta o código alterado na Dart VM em execução e reconstrói apenas os widgets afetados, sem reiniciar o app ou perder o estado |

### 📊 Adoção do Flutter (Statista, 2023)

**46%** dos desenvolvedores cross-platform usam Flutter globalmente — o framework mais adotado nessa categoria, à frente do React Native (35%). Empresas como Nubank, iFood e Itaú Unibanco usam Flutter em produção no Brasil.

> **Referências:** Flutter Architectural Overview: docs.flutter.dev/resources/architectural-overview | Statista. Cross-platform mobile frameworks used by developers worldwide, 2023. | MUSHTAQ, F. et al. *Performance Comparison of Single Code Base Development Tools: Flutter, React Native, and Xamarin*. IEEE Conf., 2024.

---

### Dart, Widgets e Estado

No Flutter, **tudo é um widget** — texto, botão, padding, coluna, imagem. Você constrói a UI combinando widgets como blocos de Lego. O Dart é a linguagem que os une: moderna, com tipagem segura e async/await nativo.

#### Null Safety — o erro mais comum evitado

> 💡 **O que é NullPointerException?**  
> O erro mais frequente em apps mobile: o código tenta acessar uma variável que ainda não foi inicializada. O Dart resolve isso **em tempo de compilação**: o app nem chega a rodar com esse problema.

```dart
// null_safety.dart

// String normal: NUNCA pode ser null
String nome = 'João';

// String? com "?": PODE ser null
String? apelido;

// "??" — valor padrão se for null
int tamanho = apelido?.length ?? 0;

// async/await — rede sem travar a UI
Future<String> buscarNome() async {
  final resposta = await chamarAPI();
  return resposta.nome;
}
```

#### StatefulWidget — widgets com estado

```dart
// contador.dart

class Contador extends StatefulWidget {
  const Contador({super.key});

  @override
  State<Contador> createState() => _ContadorState();
}

class _ContadorState extends State<Contador> {
  int _total = 0;

  @override
  Widget build(BuildContext ctx) {
    return Column(children: [
      Text('Total: $_total'),
      ElevatedButton(
        // setState reconstrói apenas este widget
        onPressed: () => setState(() => _total++),
        child: const Text('Adicionar'),
      ),
    ]);
  }
}
```

> 🤔 **E quando o estado precisa ser compartilhado?**  
> `setState()` funciona para o estado **local** de um widget. Quando o login do usuário precisa ser visível em várias telas ao mesmo tempo, usamos um framework de estado global. O **Provider** é o ponto de partida recomendado pelo time do Flutter.

> **Referências:** Dart Null Safety: THOMSEN, M. blog.dart.dev, 3 mar. 2021. | Dart Language: dart.dev/language | Flutter Widgets: docs.flutter.dev/ui/widgets | StatefulWidget lifecycle: docs.flutter.dev/ui/interactivity | Provider: pub.dev/packages/provider

---

## II. UI Adaptativa

### Material Design 3 e Design Responsivo

**O que é um design system?**  
Um conjunto de regras que garante que botões, cores e tipografia sejam **coerentes em todas as telas** do app. O **Material Design 3** (Google, 2021) é o design system do Flutter — já vem com todos os componentes implementados e suporte automático a tema claro e escuro.

```dart
// tema.dart

MaterialApp(
  theme: ThemeData(
    useMaterial3: true, // padrão desde Flutter 3.16
    // gera paleta light+dark a partir de uma cor
    colorScheme: ColorScheme.fromSeed(
      seedColor: Colors.deepPurple,
    ),
  ),
  darkTheme: ThemeData(
    useMaterial3: true,
    colorScheme: ColorScheme.fromSeed(
      seedColor: Colors.deepPurple,
      brightness: Brightness.dark,
    ),
  ),
);
```

#### Design Responsivo — um app para todos os tamanhos

```dart
// responsivo.dart

// 📐 LayoutBuilder — a forma correta
// Recebe as constraints do widget pai,
// mais confiável que MediaQuery (que retorna o tamanho da janela inteira).
LayoutBuilder(
  builder: (context, constraints) {
    if (constraints.maxWidth > 720) {
      return LayoutLargo();  // tablet/web
    }
    return LayoutEstreito(); // celular
  },
)
```

> 📱 **Cupertino — app com cara de iOS**  
> Para que o app pareça nativo no iOS (scroll com bounce, navegação estilo Apple, pickers), o Flutter tem a biblioteca **Cupertino** com os mesmos widgets da Apple Human Interface Guidelines. Um widget, dois comportamentos automáticos.

> **Referências:** Material Design 3: m3.material.io | Flutter M3 default: docs.flutter.dev/release/breaking-changes/material-3-default | Cupertino: docs.flutter.dev/ui/design/cupertino | Responsive/Adaptive: docs.flutter.dev/ui/adaptive-responsive

---

### Acessibilidade: por que todo app precisa dela

> 🎯 **Experimento: feche os olhos agora**  
> Abra o app do seu banco com os olhos fechados e ative o **TalkBack** (Android) ou **VoiceOver** (iOS). Tente fazer um Pix. Se você travar em algum ponto — um botão sem nome, uma imagem sem descrição — uma pessoa com deficiência visual **não consegue usar esse app**. No Brasil são cerca de **6,5 milhões de pessoas com deficiência visual** (IBGE 2022). Acessibilidade ruim é exclusão real.

#### Regras Básicas — WCAG 2.2 (W3C, 2023)

| Regra | Detalhe |
|---|---|
| Alvos táteis ≥ **48 × 48 dp** | Botões menores são inacessíveis |
| Contraste de cor ≥ **4,5:1** | Entre texto e fundo |
| Nunca usar **só cor** | Para transmitir informação |
| Texto ampliado | Suporte à configuração de tamanho de fonte do SO |

> ⚖️ **É também uma obrigação legal**  
> A **Lei Brasileira de Inclusão (Lei nº 13.146/2015, Art. 63)** exige acessibilidade digital em aplicativos de empresas. Descumprir pode gerar ação civil pública.

#### Semantics — como o Flutter lê a tela

```dart
// acessibilidade.dart

// SEM acessibilidade — o leitor de tela não sabe o que esse botão faz
IconButton(
  icon: Icon(Icons.favorite),
  onPressed: salvarFavorito,
)

// COM acessibilidade — correto
Semantics(
  label: 'Salvar nos favoritos',
  button: true,
  child: IconButton(
    icon: Icon(Icons.favorite),
    onPressed: salvarFavorito,
  ),
)

// Ícone decorativo — excluir da leitura
ExcludeSemantics(
  child: Icon(Icons.star, color: Colors.amber),
)
```

> 💡 **Regra prática:** todo `IconButton` sem texto visível precisa de um `Semantics` com `label`. O Flutter já adiciona semantics automaticamente em `ElevatedButton`, `TextField` e outros widgets padrão.

> **Referências:** WCAG 2.2: W3C Recommendation, 5 out. 2023. w3.org/WAI/standards-guidelines/wcag/ | Flutter Accessibility: docs.flutter.dev/ui/accessibility-and-internationalization/accessibility | ZAINA, L.; FORTES, R.; CASADEI, V. *Journal of Systems and Software*, ScienceDirect, 2022. | Lei nº 13.146/2015 (LBI), Art. 63.

---

## III. APIs REST

### Buscando dados de uma API

> ⚠️ **Regra de ouro: rede nunca na thread principal**  
> Em qualquer plataforma mobile, uma chamada de rede **jamais pode bloquear a thread principal** (a que desenha a UI). Se bloquear, o app congela visivelmente. O Dart resolve isso com `async`/`await`: enquanto aguarda a resposta, a thread é liberada e o app continua respondendo ao usuário.

#### Fazendo um GET simples

```dart
// usuarios_service.dart

import 'package:http/http.dart' as http;
import 'dart:convert';

Future<List<Usuario>> buscarUsuarios() async {
  final url = Uri.parse(
    'https://jsonplaceholder.typicode.com/users',
  );
  final resposta = await http.get(url);

  if (resposta.statusCode == 200) {
    final lista = jsonDecode(resposta.body) as List;
    return lista
        .map((j) => Usuario.fromJson(j))
        .toList();
  }
  throw Exception('Erro: ${resposta.statusCode}');
}
```

> 📦 **http vs dio — qual usar?**  
> `http` (oficial dart.dev) é suficiente para começar: API simples, sem configuração extra. `dio` é mais poderoso: adiciona interceptors, cancelamento e download com progresso. Comece com `http` e migre quando precisar.

#### Exibindo na tela com FutureBuilder

```dart
// tela_usuarios.dart

FutureBuilder<List<Usuario>>(
  future: buscarUsuarios(),
  builder: (ctx, snapshot) {
    // ainda carregando
    if (snapshot.connectionState == ConnectionState.waiting) {
      return const CircularProgressIndicator();
    }
    // deu erro
    if (snapshot.hasError) {
      return Text('Erro: ${snapshot.error}');
    }
    // dados chegaram
    final usuarios = snapshot.data!;
    return ListView.builder(
      itemCount: usuarios.length,
      itemBuilder: (ctx, i) =>
          ListTile(title: Text(usuarios[i].nome)),
    );
  },
)
```

**✅ Os três estados que todo app precisa tratar:**
- **Carregando** — mostre um indicador de progresso
- **Erro** — mostre uma mensagem e botão de tentar novamente
- **Sucesso** — exiba os dados

> **Referências:** http: pub.dev/packages/http (dart.dev, Flutter Favorite) | dio: pub.dev/packages/dio | FutureBuilder: docs.flutter.dev/cookbook/networking/fetch-data | jsonplaceholder.typicode.com — API pública gratuita para testes

---

### Autenticação: o que você precisa saber agora

Quase todo app precisa que o usuário faça login. Há uma hierarquia clara de complexidade — iniciantes costumam pular para o mais complexo sem precisar.

#### ✅ Nível 1 — Bearer Token

O backend retorna um token após o login. Você o armazena e envia no header de cada requisição:

```dart
// auth.dart

final headers = {
  'Authorization': 'Bearer $token',
  'Content-Type': 'application/json',
};
```

Suficiente para a maioria dos projetos acadêmicos e MVPs. O token vem do seu próprio backend.

#### ⚠️ Nível 2 — OAuth 2.0 + PKCE

Necessário quando o usuário faz login com Google, GitHub, Microsoft ou qualquer provedor externo. O PKCE (RFC 7636) protege o fluxo em apps móveis, que não podem guardar segredos com segurança. Use o pacote `flutter_appauth` — implementar manualmente é fonte de vulnerabilidades.

#### 📱 Nível 3 — Biometria

Para desbloquear o app com impressão digital ou Face ID após o login inicial. O pacote `local_auth` (oficial Flutter) abstrai TalkBack no Android e Face ID/Touch ID no iOS.

```dart
// biometria.dart

final auth = LocalAuthentication();
final ok = await auth.authenticate(
  localizedReason: 'Confirme sua identidade',
);
```

#### 🔴 Nunca faça isso — erros clássicos de iniciante

- ❌ Guardar token em `SharedPreferences` — sem criptografia, qualquer app no mesmo dispositivo pode ler
- ❌ Chamar APIs com `http://` sem TLS — dados em texto puro na rede
- ❌ Incluir `client_secret` no código-fonte — o binário pode ser decompilado
- ❌ Ignorar erros de certificado SSL — "funciona em dev" vira brecha em produção

> **Referências:** RFC 6749 — OAuth 2.0: HARDT, D. (Microsoft), IETF, out. 2012. | RFC 7636 — PKCE: SAKIMURA, N. et al., IETF, set. 2015. | flutter_appauth: pub.dev/packages/flutter_appauth | local_auth: pub.dev/packages/local_auth

---

## IV. Storage Local

### Onde guardar dados no dispositivo?

Nem tudo precisa de banco de dados. A escolha certa depende do tipo de dado — começar pela solução mais simples é sempre a decisão correta.

```
shared_preferences      → flags e configurações
flutter_secure_storage  → tokens e senhas
sqflite (SQLite)        → dados relacionais
drift (ORM tipado)      → apps robustos com reatividade
```

#### shared_preferences — o mais simples

```dart
// preferencias.dart

import 'package:shared_preferences/shared_preferences.dart';

// Gravar — ex: onboarding concluído
Future<void> salvarOnboarding() async {
  final prefs = await SharedPreferences.getInstance();
  await prefs.setBool('onboarding_visto', true);
}

// Ler — ao abrir o app
Future<bool> onboardingVisto() async {
  final prefs = await SharedPreferences.getInstance();
  return prefs.getBool('onboarding_visto') ?? false; // false se nunca gravou
}
```

> ⚠️ **Quando NÃO usar SharedPreferences:**  
> Nunca para tokens de autenticação, senhas ou dados sensíveis: ficam em texto puro no dispositivo. Para isso use `flutter_secure_storage`.

#### sqflite — quando precisar de banco de dados

```dart
// banco.dart

import 'package:sqflite/sqflite.dart';

// Abrir/criar banco
final db = await openDatabase(
  'meu_app.db', version: 1,
  onCreate: (db, v) async {
    await db.execute('''
      CREATE TABLE tarefas (
        id INTEGER PRIMARY KEY,
        titulo TEXT,
        feita INTEGER
      )''');
  },
);

// Inserir uma tarefa
await db.insert('tarefas', {'titulo': 'Estudar Flutter', 'feita': 0});

// Buscar todas as tarefas
final tarefas = await db.query('tarefas');
```

> 🚀 **Próximo nível: drift**  
> O `drift` é um ORM type-safe sobre SQLite com queries verificadas em compile-time e streams reativos que atualizam a UI automaticamente. Recomendado quando o projeto cresce além do sqflite manual.

> **Referências:** shared_preferences: pub.dev/packages/shared_preferences | sqflite: pub.dev/packages/sqflite (v2.4.2) | flutter_secure_storage: pub.dev/packages/flutter_secure_storage | drift: simonbinder.eu (Flutter Favorite) | KLEPPMANN, M. et al. Local-first software. *ACM Onward! '19*. DOI: 10.1145/3359591.3359737.

---

### Guardando dados sensíveis com segurança

Token de autenticação, senha salva e chave de API não podem ficar em texto puro no dispositivo. O `flutter_secure_storage` usa os cofres criptografados nativos de cada plataforma.

| Plataforma | Mecanismo |
|---|---|
| iOS | Keychain Services |
| Android | Keystore + Google Tink |

```dart
// storage_seguro.dart

import 'package:flutter_secure_storage/flutter_secure_storage.dart';

const storage = FlutterSecureStorage(
  aOptions: AndroidOptions(
    encryptedSharedPreferences: true, // EncryptedSharedPreferences do Jetpack
  ),
  iOptions: IOSOptions(
    // dados acessíveis após 1º desbloqueio
    accessibility: KeychainAccessibility.first_unlock,
  ),
);

// Gravar token com segurança
await storage.write(key: 'access_token', value: token);

// Ler — null se não existir
final token = await storage.read(key: 'access_token');

// Apagar ao fazer logout
await storage.delete(key: 'access_token');
```

#### Regra de ouro: qual storage usar para quê?

| Dado | Use | Nunca use |
|---|---|---|
| Tema escuro ativo? | SharedPreferences | — |
| Onboarding visto? | SharedPreferences | — |
| Token de autenticação | flutter_secure_storage | SharedPreferences |
| Senha salva | flutter_secure_storage | SharedPreferences |
| Histórico de pedidos | sqflite / drift | SharedPreferences |
| Lista de favoritos | sqflite / drift | SharedPreferences |

#### 🔴 Os três erros mais comuns de iniciantes

- Guardar token JWT em `SharedPreferences` — texto puro, sem criptografia
- Guardar dados do usuário em variáveis globais — somem ao fechar o app
- Não apagar o token no logout — usuário "deslogado" ainda tem acesso

> ✅ **SDK mínimo:** `flutter_secure_storage` v10+ requer Android SDK 23 (Android 6.0), cobrindo 99%+ dos dispositivos ativos hoje.

> **Referências:** flutter_secure_storage v10.0.0: pub.dev/packages/flutter_secure_storage — AES/GCM (dados) + RSA/OAEP-SHA256 (chave), armazenada no Android Keystore; iOS Keychain Services. Migrou Jetpack Security → Google Tink na v10. SDK mínimo Android: 23.

---

## V. Segurança + LGPD

### Os riscos que todo iniciante precisa conhecer

O **OWASP Mobile Top 10 2024** lista os dez maiores riscos em apps mobile. Três deles são responsáveis pela maioria das vulnerabilidades encontradas em projetos de iniciantes:

#### 🔴 M9 — Insecure Data Storage

Dados sensíveis em `SharedPreferences`, logs ou SQLite sem criptografia.

- **Exemplo real:** token JWT gravado em SharedPreferences — qualquer backup do dispositivo expõe o token em texto puro.
- **Solução:** `flutter_secure_storage` para qualquer dado de autenticação.

#### 🔴 M5 — Insecure Communication

Chamadas de API com `http://` em vez de `https://`. Os dados trafegam em texto puro pela rede.

- **Exemplo real:** login enviando usuário e senha via HTTP — qualquer ponto da rede pode interceptar.
- **Solução:** sempre HTTPS. Nunca desabilite verificação de certificado, nem em desenvolvimento.

#### 🔴 M3 — Insecure Authentication

Autenticação feita no client-side, tokens sem expiração ou segredos hardcoded no código.

- **Exemplo real:** `client_secret` do OAuth escrito direto no código — extraível pelo APK decompilado.
- **Solução:** segredos sempre no backend. Use `flutter_appauth` para OAuth sem `client_secret` no client.

#### 🗺️ Os outros 7 riscos do OWASP Mobile Top 10

M1 (credenciais indevidas), M2 (supply chain), M4 (validação de input), M6 (privacidade), M7 (proteção de binário), M8 (configuração), M10 (criptografia) são igualmente importantes, mas exigem maturidade de produto antes de se tornarem prioridade. Checklist completo em: owasp.org/www-project-mobile-top-10

> **Referências:** OWASP Mobile Top 10 2024: owasp.org/www-project-mobile-top-10/2023-risks/ | OWASP MASVS v2.1.0 (jan. 2024): mas.owasp.org/MASVS/

---

### LGPD — o que muda no seu app

Se seu app coleta qualquer dado pessoal — nome, e-mail, localização, comportamento de uso — a **Lei Geral de Proteção de Dados (Lei nº 13.709/2018)** se aplica. Não é opcional, não é só para empresas grandes, e a ANPD já autua.

**O que é dado pessoal?**  
Qualquer informação que identifique ou possa identificar uma pessoa: nome, CPF, e-mail, telefone, endereço IP, localização GPS, cookies de rastreamento, histórico de navegação, biometria.

> Se o seu app usa Firebase Analytics, AdMob ou qualquer SDK de rastreamento → está coletando dados pessoais.

#### Os direitos do usuário (Art. 18)

| Direito | Descrição |
|---|---|
| Acesso | Saber quais dados você tem sobre ele |
| Correção | Corrigir dados errados |
| Exclusão | Apagar seus dados quando pedir |
| Portabilidade | Exportar seus dados |
| Revogar consentimento | A qualquer momento |

#### Checklist mínimo para seu app

- ✅ **Política de privacidade** acessível antes do primeiro uso
- ✅ **Consentimento explícito** antes de coletar dados — não pré-marcado
- ✅ **Coletar o mínimo necessário** — não peça o que não vai usar
- ✅ **Botão "excluir minha conta"** com exclusão real dos dados
- ✅ Dados sensíveis **nunca em logs** de produção
- ✅ **HTTPS** em todas as comunicações

#### 🔴 Penalidades da LGPD

Multa de até **2% do faturamento anual** no Brasil, com teto de **R$ 50 milhões por infração**. A ANPD pode ainda determinar bloqueio ou eliminação dos dados coletados irregularmente, o que pode inviabilizar o app.

#### 🌍 LGPD vs GDPR

Se seu app tiver usuários na União Europeia, o **GDPR** também se aplica, com multas maiores (até €20M ou 4% do faturamento global). A lógica é semelhante: consentimento, minimização e direito à exclusão.

> **Referências:** LGPD: Lei nº 13.709, de 14 ago. 2018. planalto.gov.br | Vigência plena: 18 set. 2020. Sanções: 1º ago. 2021. ANPD: gov.br/anpd | GDPR: Regulamento EU 2016/679, 25 mai. 2018. | OWASP MASVS-PRIVACY v2.1.0: mas.owasp.org/MASVS/

---

## Síntese — O que levar para o primeiro projeto

### Decisões simples para começar

| Situação | Decisão |
|---|---|
| 📱 Preciso de um app para iOS e Android? | Flutter. Um codebase, dois apps. Hot reload para iterar rápido. |
| 🌐 Preciso buscar dados de uma API? | Pacote `http` + `async/await` + `FutureBuilder`. Simples e suficiente para começar. |
| 💾 Preciso guardar dados no dispositivo? | `SharedPreferences` para flags. `flutter_secure_storage` para tokens. `sqflite` quando precisar de banco. |
| 🔒 Meu app é seguro o suficiente? | Sempre HTTPS. Token em secure_storage. Nunca hardcode de segredos. Consentimento LGPD antes de coletar dados. |
| 🧪 Como sei que meu código funciona? | `flutter_test` para unit tests (lógica) e widget tests (render). |

### O que aprofundar depois desta aula

| Tema | Caminho |
|---|---|
| Estado global | Provider → Riverpod → BLoC (escala com o projeto) |
| Storage avançado | drift (ORM reativo sobre SQLite) |
| Auth avançada | OAuth 2.0 + PKCE com `flutter_appauth` |
| Offline-first | Fila de sincronização + resolução de conflitos |
| Testes | Pirâmide unit / widget / integration |
| Internacionalização | `flutter_localizations` + ARB |

---

## Referências Essenciais

- Flutter docs: [docs.flutter.dev](https://docs.flutter.dev)
- Dart language: [dart.dev/language](https://dart.dev/language)
- pub.dev: repositório de pacotes Flutter
- WCAG 2.2: [w3.org/WAI](https://w3.org/WAI)
- OWASP Mobile: [mas.owasp.org](https://mas.owasp.org)
- LGPD: Lei 13.709/2018 · [gov.br/anpd](https://gov.br/anpd)

---

*PUC Minas / ICEI — Aplicações Móveis e Distribuídas · 2026/1*
