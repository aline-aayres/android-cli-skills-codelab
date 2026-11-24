# 🚀 Codelab: Android CLI & Android Skills na Prática

Um guia prático e interativo no formato **Google Codelabs** ensinando como acelerar o desenvolvimento Android diretamente pelo terminal e potencializar agentes de IA (Gemini CLI, Antigravity, Claude Code, Cursor) utilizando o **Android CLI** e as **Android Skills**.

🔗 **Acesse o Codelab Interativo:** [https://aline-aayres.github.io/android-cli-skills-codelab/](https://aline-aayres.github.io/android-cli-skills-codelab/)

---

## 📋 O que você vai aprender

* **Configuração do Ambiente:** Instalação e diagnóstico do Android CLI (`android doctor`).
* **Automação de Workspace:** Inicialização de projetos com `android init` e configuração persistente via `.androidrc`.
* **Gerenciamento via Terminal:** Controle de pacotes do SDK e criação/execução de emuladores (AVDs) sem depender da interface gráfica da IDE.
* **Build & Diagnóstico Rápido:** Compilação, instalação de APKs e filtragem de logs diretamente no fluxo do terminal.
* **Android Skills:** Instalação e uso de instruções modulares (`android skills add`) do catálogo oficial para orientar agentes de código em arquitetura Android e Jetpack Compose.
* **Fluxo Agêntico Completo:** Construção e validação de uma funcionalidade real combinando terminal, CLI e um agente de IA.

---

## 🗂️ Estrutura do Repositório

```text
.
├── .github/
│   └── workflows/
│       └── deploy.yml                  # Pipeline de CI/CD que converte o .md via claat e publica no GitHub Pages
├── codelab_android_cli_skills.md       # Fonte completa do codelab no formato claat Markdown
└── README.md                           # Documentação principal do repositório
```

---

## 🛠️ Pré-requisitos para acompanhar o Codelab

* **Sistema Operacional:** macOS, Linux ou Windows (WSL2)
* **Java:** JDK 17 ou superior configurado no `PATH`
* **Android SDK:** Configurado localmente (`ANDROID_HOME`)
* **Agente de IA (opcional para a etapa prática):** Gemini CLI, Antigravity, Claude Code ou Cursor

---

## 📚 Referências Oficiais

* [Documentação Oficial — Android CLI](https://developer.android.com/tools/agents/android-cli)
* [Repositório Oficial — android/skills](https://github.com/android/skills)
* [Ferramenta Google Codelabs (`claat`)](https://github.com/googlecodelabs/tools)

---

## 🤝 Contribuições

Encontrou algum comando desatualizado ou quer sugerir uma nova etapa prática? Sinta-se à vontade para abrir uma *Issue* ou enviar um *Pull Request* editando o arquivo `codelab_android_cli_skills.md`.
