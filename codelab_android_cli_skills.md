summary: Aprenda a instalar, configurar e acelerar o desenvolvimento Android diretamente pelo terminal utilizando o Android CLI e as Android Skills para agentes de IA.

id: android-cli-e-android-skills

categories: Android, CLI, AI Agents, Productivity

tags: android-cli, android-skills, gemini, terminal, devops

status: Published

authors: Aline Ayres

# Codelab Android CLI e Android Skills: Desenvolvimento Android com Agentes de IA pelo Terminal

## 1. Introdução e Pré-requisitos
Duration: 03:00

O desenvolvimento Android tradicional depende fortemente da interface gráfica do Android Studio para gerenciar SDKs, configurar emuladores, compilar projetos e depurar falhas. Embora a IDE seja poderosa, esse fluxo pode ser pesado e cria atritos quando você deseja trabalhar rapidamente pelo terminal, automatizar tarefas em pipelines ou integrar **agentes de IA** (como Gemini CLI, Antigravity, Claude Code ou Cursor) ao seu ciclo de desenvolvimento.

O **Android CLI** e as **Android Skills** resolvem exatamente esse problema: oferecem uma interface de linha de comando unificada, rápida e *agent-friendly* para executar todo o ciclo de vida de um aplicativo Android sem precisar abrir a IDE.

### O que você vai aprender
* Como instalar e validar o ambiente com o **Android CLI**.
* Como inicializar projetos e personalizar preferências locais com o arquivo `.androidrc`.
* Como gerenciar SDKs, criar e iniciar emuladores diretamente pelo terminal.
* Como compilar, instalar APKs e inspecionar logs de forma enxuta.
* O que são **Android Skills** e como instalá-las para capacitar agentes de IA no seu projeto.
* Como conduzir um fluxo prático de desenvolvimento assistido por IA no terminal.

### Pré-requisitos
Antes de começar, certifique-se de ter:
* Sistema operacional **macOS**, **Linux** ou **Windows** (com WSL2 ou PowerShell).
* **JDK 17** ou superior instalado e configurado na variável `JAVA_HOME`.
* **Git** instalado na máquina.
* Acesso ao terminal com conexão à internet para download de pacotes.
* *(Opcional)* Um agente de código baseado em terminal instalado (ex.: Gemini CLI, Claude Code, Codex ou Antigravity) para a etapa prática final.

---

## 2. Instalação e Diagnóstico do Ambiente
Duration: 04:00

Nesta etapa, você vai instalar o **Android CLI** e verificar se todas as dependências do sistema estão prontas para compilar e executar aplicativos Android.

### Instalando o Android CLI

Escolha o método correspondente ao seu sistema operacional:

**macOS / Linux (via Homebrew):**
```bash
brew install android-cli
```

**macOS / Linux (via script oficial):**
```bash
curl -fsSL https://dl.google.com/android/cli/install.sh | bash
```

**Windows (via PowerShell):**
```powershell
irm https://dl.google.com/android/cli/install.ps1 | iex
```

> **Nota:** Após a instalação via script, reinicie o terminal ou recarregue o perfil do seu shell (`source ~/.zshrc` ou `source ~/.bashrc`) para garantir que o binário `android` esteja no seu `PATH`.

### Verificando a instalação

Execute o comando abaixo para confirmar que a ferramenta está acessível:

```bash
android --version
```

Em seguida, execute o comando de diagnóstico para inspecionar o JDK, variáveis de ambiente, ferramentas de build e o Android SDK:

```bash
android doctor
```

A saída exibirá um checklist com o status de cada componente (`[✓]` ou `[!]`). Caso o Android SDK ainda não esteja instalado ou configurado na máquina, o próprio `android doctor` indicará o comando exato para corrigir ou instalar os componentes básicos.

---

## 3. Inicialização de Projetos e Configuração `.androidrc`
Duration: 05:00

O Android CLI permite tanto iniciar um projeto novo a partir de templates modernos quanto configurar um repositório existente para responder aos comandos rápidos do terminal.

### Criando um novo projeto

Crie um diretório de trabalho e inicialize um aplicativo base em Kotlin com Jetpack Compose:

```bash
mkdir hello-android-cli
cd hello-android-cli
android init --name "HelloCLI" --package "com.example.hellocli" --template compose
```

> **Dica:** Se você já estiver dentro da pasta de um projeto Android existente, basta rodar `android init` sem flags. O CLI detectará a estrutura do Gradle automaticamente e criará o arquivo de configuração local.

### Configurando o arquivo `.androidrc`

Durante a inicialização, um arquivo oculto chamado `.androidrc` é gerado na raiz do projeto. Ele funciona como o perfil local do workspace, evitando que você precise digitar flags repetitivas em cada comando.

Abra ou edite o arquivo `.androidrc` para deixá-lo semelhante ao exemplo abaixo:

```json
{
  "project": {
    "defaultModule": "app",
    "defaultVariant": "debug"
  },
  "sdk": {
    "compileSdk": 35,
    "targetSdk": 35,
    "minSdk": 24
  },
  "emulator": {
    "defaultAvd": "Pixel_8_API_35",
    "headless": false
  },
  "output": {
    "format": "concise"
  }
}
```

A opção `"format": "concise"` é especialmente útil: ela reduz o excesso de logs verbosos do Gradle, exibindo apenas o progresso essencial e mensagens de erro claras (ideal tanto para leitura humana quanto para economizar tokens quando usado por agentes de IA).

---

## 4. Gerenciando SDKs e Emuladores pelo Terminal
Duration: 05:00

Esqueça a necessidade de abrir o *SDK Manager* ou o *Device Manager* apenas para baixar uma imagem de sistema ou subir um dispositivo virtual.

### Gerenciando pacotes do SDK

Para listar os pacotes instalados e disponíveis:

```bash
android sdk list --installed
```

Para instalar uma versão específica da plataforma Android, Build Tools e a imagem de sistema do emulador em um único comando:

```bash
android sdk install "platforms;android-35" "build-tools;35.0.0" "system-images;android-35;google_apis;arm64-v8a"
```
*(Em máquinas x86_64 sem suporte ARM, substitua `arm64-v8a` por `x86_64`.)*

### Criando e iniciando um Emulador (AVD)

Crie o dispositivo virtual configurado anteriormente no seu `.androidrc`:

```bash
android emu create --name "Pixel_8_API_35" --device "pixel_8" --package "system-images;android-35;google_apis;arm64-v8a"
```

Inicie o emulador em segundo plano e aguarde até que o sistema operacional esteja completamente inicializado:

```bash
android emu start Pixel_8_API_35 --wait
```

Verifique se o dispositivo está pronto para receber builds:

```bash
android devices
```

---

## 5. Build, Execução e Diagnóstico Rápido
Duration: 05:00

Com o projeto configurado e o emulador ativo, todo o ciclo de compilação, instalação e leitura de logs acontece em poucos segundos.

### Compilando e executando o app

Para compilar a variante padrão (`debug`), instalar o APK no emulador ativo e abrir a `MainActivity` automaticamente:

```bash
android run
```

Se desejar apenas compilar o APK ou gerar um Android App Bundle (AAB) para release:

```bash
android build --variant release --bundle
```

### Executando testes automatizados

Rode os testes unitários locais sem precisar lembrar o nome completo da task do Gradle:

```bash
android test unit
```

Ou execute testes instrumentados diretamente no emulador conectado:

```bash
android test connected
```

### Inspecionando logs filtrados

Para acompanhar os logs apenas do pacote do seu aplicativo (filtrando o ruído global do sistema Android):

```bash
android logcat --app com.example.hellocli --level warn
```

---

## 6. Introdução às Android Skills
Duration: 04:00

### O que são Android Skills?

Agentes de IA de propósito geral frequentemente cometem erros ao escrever código Android moderno: sugerem APIs depreciadas, ignoram boas práticas de gerenciamento de estado no Jetpack Compose ou não sabem operar ferramentas locais do ecossistema.

As **Android Skills** (mantidas oficialmente no repositório `android/skills`) são pacotes modulares de conhecimento, instruções estruturadas e fluxos de trabalho validados que ensinam agentes de código a operar como especialistas Android.

Cada *Skill* contém:
* **Diretrizes arquiteturais e boas práticas atualizadas** (ex.: Compose, Navigation, Room, Coroutines, Material 3).
* **Instruções de uso de ferramentas** (como operar o próprio `android` CLI para validar código).
* **Padrões de verificação** para que o agente compile, teste e corrija os próprios erros antes de entregar a resposta final.

### Explorando o catálogo de Skills

Você pode consultar todas as skills oficiais disponíveis diretamente pelo terminal:

```bash
android skills search
```

Entre as principais skills disponíveis no catálogo oficial, destacam-se:
* `android-cli`: Ensina o agente a usar os comandos do Android CLI (`build`, `run`, `test`, `logcat`) para verificar o próprio código.
* `jetpack-compose`: Padrões modernos de UI declarativa, hoisting de estado, performance e Material 3.
* `android-architecture`: Guia oficial de arquitetura em camadas (UI, Domain, Data) e injeção de dependências com Hilt.
* `android-testing`: Boas práticas para testes unitários, Compose UI Tests e fakes.

---

## 7. Instalando e Configurando Skills no Projeto
Duration: 05:00

Nesta etapa, você vai adicionar skills ao seu projeto para que qualquer agente compatível consiga utilizá-las automaticamente.

### Adicionando skills essenciais

Na raiz do projeto `hello-android-cli`, instale a skill de operação do CLI e a skill de Jetpack Compose:

```bash
android skills add android-cli
android skills add jetpack-compose
```

### Onde as skills são armazenadas?

Ao executar `android skills add`, o CLI baixa as definições da skill e as organiza na estrutura padrão reconhecida pelos agentes de IA no seu workspace (como `.agents/skills/` ou `.gemini/skills/`, dependendo da configuração do projeto):

```text
hello-android-cli/
├── .androidrc
├── .agents/
│   └── skills/
│       ├── android-cli/
│       │   └── SKILL.md
│       └── jetpack-compose/
│           └── SKILL.md
├── app/
└── build.gradle.kts
```

### Gerenciando skills instaladas

Para listar as skills ativas no projeto atual e verificar se há atualizações disponíveis no repositório oficial:

```bash
android skills list
android skills update
```

> **Dica:** Você pode versionar a pasta `.agents/skills/` no Git do seu time para garantir que todos os desenvolvedores e agentes de CI/CD sigam exatamente os mesmos padrões de código.

---

## 8. Fluxo Prático com Agente de IA
Duration: 06:00

Agora vamos unir o **Android CLI** e as **Android Skills** em um fluxo real de desenvolvimento assistido por agente no terminal.

### Cenário prático

Abra o seu agente de IA favorito no terminal na raiz do projeto `hello-android-cli` (por exemplo, executando `gemini`, `claude` ou `codex`) e envie o seguinte prompt:

```text
Usando as Android Skills instaladas no projeto:
1. Crie uma tela em Jetpack Compose com um contador interativo e histórico das últimas 5 ações.
2. Siga as diretrizes de gerenciamento de estado da skill jetpack-compose.
3. Utilize o Android CLI (skill android-cli) para compilar o projeto, rodar os testes unitários e instalar o app no emulador ativo. Se houver algum erro de compilação, corrija antes de concluir.
```

### O que acontece nos bastidores?

1. **Carregamento de contexto:** O agente detecta automaticamente os arquivos `SKILL.md` de `jetpack-compose` e `android-cli`.
2. **Geração idiomática:** O código gerado utiliza `remember`, `mutableStateOf` ou `StateFlow` com *state hoisting* adequado e componentes Material 3 atualizados.
3. **Loop de autoverificação:** Em vez de apenas escrever o código e encerrar, o agente executa `android build` e `android test unit` pelo terminal.
4. **Correção autônoma:** Caso falte algum import ou ocorra erro de sintaxe, o agente lê a saída enxuta do CLI, ajusta o arquivo Kotlin e executa `android run` até confirmar que o aplicativo está rodando no emulador.

---

## 9. Conclusão e Próximos Passos
Duration: 02:00

Parabéns! Você configurou um ambiente moderno de desenvolvimento Android centrado no terminal e preparado para colaboração eficiente com agentes de IA.

### Resumo dos comandos principais

| Tarefa | Comando |
| :--- | :--- |
| Verificar saúde do ambiente | `android doctor` |
| Inicializar projeto / `.androidrc` | `android init` |
| Gerenciar SDKs | `android sdk install "<pacote>"` |
| Iniciar emulador | `android emu start <nome> --wait` |
| Compilar, instalar e abrir app | `android run` |
| Rodar testes unitários | `android test unit` |
| Filtrar logs do aplicativo | `android logcat --app <pacote>` |
| Buscar e instalar Android Skills | `android skills add <nome-da-skill>` |

### Próximos passos
* **Explore a documentação oficial:** Acesse [developer.android.com/tools/agents/android-cli](https://developer.android.com/tools/agents/android-cli) para consultar todas as flags avançadas de build e diagnóstico.
* **Conheça o repositório de Skills:** Visite [github.com/android/skills](https://github.com/android/skills) para acompanhar novas skills oficiais.
* **Crie sua própria Skill interna:** Experimente criar um arquivo `SKILL.md` customizado dentro de `.agents/skills/` documentando o Design System ou a arquitetura específica da sua empresa.
