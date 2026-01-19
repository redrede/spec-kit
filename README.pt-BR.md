> 🇧🇷 Esta documentação está em Português do Brasil. [English version](./README.md)

<div align="center">
    <img src="./media/logo_large.webp" alt="Logo do Spec Kit" width="200" height="200"/>
    <h1>🌱 Spec Kit</h1>
    <h3><em>Construa software de alta qualidade mais rapidamente.</em></h3>
</div>

<p align="center">
    <strong>Um toolkit open source que permite que você se concentre em cenários de produto e resultados previsíveis, em vez de programar cada peça do zero.</strong>
</p>

<p align="center">
    <a href="https://github.com/github/spec-kit/actions/workflows/release.yml"><img src="https://github.com/github/spec-kit/actions/workflows/release.yml/badge.svg" alt="Release"/></a>
    <a href="https://github.com/github/spec-kit/stargazers"><img src="https://img.shields.io/github/stars/github/spec-kit?style=social" alt="Estrelas no GitHub"/></a>
    <a href="https://github.com/github/spec-kit/blob/main/LICENSE"><img src="https://img.shields.io/github/license/github/spec-kit" alt="Licença"/></a>
    <a href="https://github.github.io/spec-kit/"><img src="https://img.shields.io/badge/docs-GitHub_Pages-blue" alt="Documentação"/></a>
</p>

---

## Índice

- [🤔 O que é Desenvolvimento Orientado por Especificação?](#-o-que-é-desenvolvimento-orientado-por-especificação)
- [⚡ Primeiros Passos](#-primeiros-passos)
- [📽️ Visão Geral em Vídeo](#️-visão-geral-em-vídeo)
- [🤖 Agentes de IA Suportados](#-agentes-de-ia-suportados)
- [🔧 Referência da CLI Specify](#-referência-da-cli-specify)
- [📚 Filosofia Central](#-filosofia-central)
- [🌟 Fases de Desenvolvimento](#-fases-de-desenvolvimento)
- [🎯 Objetivos Experimentais](#-objetivos-experimentais)
- [🔧 Pré-requisitos](#-pré-requisitos)
- [📖 Saiba Mais](#-saiba-mais)
- [📋 Processo Detalhado](#-processo-detalhado)
- [🔍 Solução de Problemas](#-solução-de-problemas)
- [👥 Mantenedores](#-mantenedores)
- [💬 Suporte](#-suporte)
- [🙏 Agradecimentos](#-agradecimentos)
- [📄 Licença](#-licença)

## 🤔 O que é Desenvolvimento Orientado por Especificação?

O Desenvolvimento Orientado por Especificação **inverte a lógica** do desenvolvimento de software tradicional. Por décadas, o código foi rei — especificações eram apenas andaimes que construíamos e descartávamos quando o "trabalho real" de codificação começava. O Desenvolvimento Orientado por Especificação muda isso: **especificações se tornam executáveis**, gerando implementações funcionais diretamente, em vez de apenas guiá-las.

## ⚡ Primeiros Passos

### 1. Instale a CLI Specify

Escolha seu método de instalação preferido:

#### Opção 1: Instalação Persistente (Recomendado)

Instale uma vez e use em qualquer lugar:

```bash
uv tool install specify-cli --from git+https://github.com/redrede/spec-kit.git
```

Depois use a ferramenta diretamente:

```bash
# Criar novo projeto
specify init <NOME_DO_PROJETO>

# Ou inicializar em projeto existente
specify init . --ai claude
# ou
specify init --here --ai claude

# Verificar ferramentas instaladas
specify check
```

Para atualizar o Specify, consulte o [Guia de Atualização](./docs/pt-BR/upgrade.md) para instruções detalhadas. Atualização rápida:

```bash
uv tool install specify-cli --force --from git+https://github.com/redrede/spec-kit.git
```

#### Opção 2: Uso Único

Execute diretamente sem instalar:

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <NOME_DO_PROJETO>
```

**Benefícios da instalação persistente:**

- A ferramenta fica instalada e disponível no PATH
- Não é necessário criar aliases de shell
- Melhor gerenciamento de ferramentas com `uv tool list`, `uv tool upgrade`, `uv tool uninstall`
- Configuração de shell mais limpa

### 2. Estabeleça os princípios do projeto

Inicie seu assistente de IA no diretório do projeto. Os comandos `/speckit.*` estão disponíveis no assistente.

Use o comando **`/speckit.constitution`** para criar os princípios de governança e diretrizes de desenvolvimento do seu projeto que guiarão todo o desenvolvimento subsequente.

```bash
/speckit.constitution Criar princípios focados em qualidade de código, padrões de teste, consistência de experiência do usuário e requisitos de performance
```

### 3. Crie a especificação

Use o comando **`/speckit.specify`** para descrever o que você quer construir. Foque no **o quê** e no **por quê**, não na stack tecnológica.

```bash
/speckit.specify Construir uma aplicação que pode me ajudar a organizar minhas fotos em álbuns separados. Álbuns são agrupados por data e podem ser reorganizados arrastando e soltando na página principal. Álbuns nunca estão aninhados dentro de outros álbuns. Dentro de cada álbum, as fotos são visualizadas em uma interface de grade.
```

### 4. Crie um plano de implementação técnica

Use o comando **`/speckit.plan`** para fornecer sua stack tecnológica e escolhas de arquitetura.

```bash
/speckit.plan A aplicação usa Vite com número mínimo de bibliotecas. Use HTML, CSS e JavaScript vanilla o máximo possível. Imagens não são enviadas para nenhum lugar e os metadados são armazenados em um banco de dados SQLite local.
```

### 5. Divida em tarefas

Use **`/speckit.tasks`** para criar uma lista de tarefas acionáveis a partir do seu plano de implementação.

```bash
/speckit.tasks
```

### 6. Execute a implementação

Use **`/speckit.implement`** para executar todas as tarefas e construir sua funcionalidade de acordo com o plano.

```bash
/speckit.implement
```

Para instruções detalhadas passo a passo, consulte nosso [guia completo](./spec-driven.pt-BR.md).

## 📽️ Visão Geral em Vídeo

Quer ver o Spec Kit em ação? Assista nossa [visão geral em vídeo](https://www.youtube.com/watch?v=a9eR1xsfvHg&pp=0gcJCckJAYcqIYzv)!

[![Cabeçalho do vídeo do Spec Kit](/media/spec-kit-video-header.jpg)](https://www.youtube.com/watch?v=a9eR1xsfvHg&pp=0gcJCckJAYcqIYzv)

## 🤖 Agentes de IA Suportados

| Agente                                                                               | Suporte | Notas                                                                                                                                         |
| ------------------------------------------------------------------------------------ | ------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| [Qoder CLI](https://qoder.com/cli)                                                   | ✅      |                                                                                                                                               |
| [Amazon Q Developer CLI](https://aws.amazon.com/developer/learning/q-developer-cli/) | ⚠️      | Amazon Q Developer CLI [não suporta](https://github.com/aws/amazon-q-developer-cli/issues/3064) argumentos personalizados para slash commands. |
| [Amp](https://ampcode.com/)                                                          | ✅      |                                                                                                                                               |
| [Auggie CLI](https://docs.augmentcode.com/cli/overview)                              | ✅      |                                                                                                                                               |
| [Claude Code](https://www.anthropic.com/claude-code)                                 | ✅      |                                                                                                                                               |
| [CodeBuddy CLI](https://www.codebuddy.ai/cli)                                        | ✅      |                                                                                                                                               |
| [Codex CLI](https://github.com/openai/codex)                                         | ✅      |                                                                                                                                               |
| [Cursor](https://cursor.sh/)                                                         | ✅      |                                                                                                                                               |
| [Gemini CLI](https://github.com/google-gemini/gemini-cli)                            | ✅      |                                                                                                                                               |
| [GitHub Copilot](https://code.visualstudio.com/)                                     | ✅      |                                                                                                                                               |
| [IBM Bob](https://www.ibm.com/products/bob)                                          | ✅      | Agente baseado em IDE com suporte a slash commands                                                                                            |
| [Jules](https://jules.google.com/)                                                   | ✅      |                                                                                                                                               |
| [Kilo Code](https://github.com/Kilo-Org/kilocode)                                    | ✅      |                                                                                                                                               |
| [opencode](https://opencode.ai/)                                                     | ✅      |                                                                                                                                               |
| [Qwen Code](https://github.com/QwenLM/qwen-code)                                     | ✅      |                                                                                                                                               |
| [Roo Code](https://roocode.com/)                                                     | ✅      |                                                                                                                                               |
| [SHAI (OVHcloud)](https://github.com/ovh/shai)                                       | ✅      |                                                                                                                                               |
| [Windsurf](https://windsurf.com/)                                                    | ✅      |                                                                                                                                               |

## 🔧 Referência da CLI Specify

O comando `specify` suporta as seguintes opções:

### Comandos

| Comando | Descrição                                                                                                                                               |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `init`  | Inicializar um novo projeto Specify a partir do template mais recente                                                                                   |
| `check` | Verificar ferramentas instaladas (`git`, `claude`, `gemini`, `code`/`code-insiders`, `cursor-agent`, `windsurf`, `qwen`, `opencode`, `codex`, `shai`, `qoder`) |

### Argumentos e Opções do `specify init`

| Argumento/Opção        | Tipo      | Descrição                                                                                                                                                            |
| ---------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `<project-name>`       | Argumento | Nome para o novo diretório do projeto (opcional se usar `--here`, ou use `.` para o diretório atual)                                                                  |
| `--ai`                 | Opção     | Assistente de IA a usar: `claude`, `gemini`, `copilot`, `cursor-agent`, `qwen`, `opencode`, `codex`, `windsurf`, `kilocode`, `auggie`, `roo`, `codebuddy`, `amp`, `shai`, `q`, `bob`, ou `qoder` |
| `--language`, `-l`     | Opção     | Idioma para templates e saída da CLI: `en` (Inglês, padrão) ou `pt-BR` (Português do Brasil)                                                                         |
| `--script`             | Opção     | Variante de script a usar: `sh` (bash/zsh) ou `ps` (PowerShell)                                                                                                      |
| `--ignore-agent-tools` | Flag      | Pular verificações de ferramentas de agentes de IA como Claude Code                                                                                                   |
| `--no-git`             | Flag      | Pular inicialização do repositório git                                                                                                                                |
| `--here`               | Flag      | Inicializar projeto no diretório atual em vez de criar um novo                                                                                                        |
| `--force`              | Flag      | Forçar mesclagem/sobrescrita ao inicializar no diretório atual (pular confirmação)                                                                                    |
| `--skip-tls`           | Flag      | Pular verificação SSL/TLS (não recomendado)                                                                                                                           |
| `--debug`              | Flag      | Habilitar saída de debug detalhada para solução de problemas                                                                                                          |
| `--github-token`       | Opção     | Token do GitHub para requisições de API (ou defina a variável de ambiente GH_TOKEN/GITHUB_TOKEN)                                                                      |

### Exemplos

```bash
# Inicialização básica de projeto
specify init meu-projeto

# Inicializar com assistente de IA específico
specify init meu-projeto --ai claude

# Inicializar com suporte a Cursor
specify init meu-projeto --ai cursor-agent

# Inicializar com suporte a Qoder
specify init meu-projeto --ai qoder

# Inicializar com suporte a Windsurf
specify init meu-projeto --ai windsurf

# Inicializar com suporte a Amp
specify init meu-projeto --ai amp

# Inicializar com suporte a SHAI
specify init meu-projeto --ai shai

# Inicializar com suporte a IBM Bob
specify init meu-projeto --ai bob

# Inicializar com scripts PowerShell (Windows/multiplataforma)
specify init meu-projeto --ai copilot --script ps

# Inicializar no diretório atual
specify init . --ai copilot
# ou use a flag --here
specify init --here --ai copilot

# Forçar mesclagem no diretório atual (não vazio) sem confirmação
specify init . --force --ai copilot
# ou
specify init --here --force --ai copilot

# Pular inicialização do git
specify init meu-projeto --ai gemini --no-git

# Habilitar saída de debug para solução de problemas
specify init meu-projeto --ai claude --debug

# Usar token do GitHub para requisições de API (útil para ambientes corporativos)
specify init meu-projeto --ai claude --github-token ghp_seu_token_aqui

# Inicializar com idioma Português do Brasil
specify init meu-projeto --ai claude --language pt-BR
# ou use a forma abreviada
specify init meu-projeto --ai claude -l pt-BR

# Verificar requisitos do sistema
specify check
```

### Slash Commands Disponíveis

Após executar `specify init`, seu agente de codificação de IA terá acesso a estes slash commands para desenvolvimento estruturado:

#### Comandos Principais

Comandos essenciais para o fluxo de trabalho de Desenvolvimento Orientado por Especificação:

| Comando                 | Descrição                                                                    |
| ----------------------- | ---------------------------------------------------------------------------- |
| `/speckit.constitution` | Criar ou atualizar princípios de governança e diretrizes de desenvolvimento   |
| `/speckit.specify`      | Definir o que você quer construir (requisitos e histórias de usuário)        |
| `/speckit.plan`         | Criar planos de implementação técnica com sua stack tecnológica escolhida    |
| `/speckit.tasks`        | Gerar listas de tarefas acionáveis para implementação                        |
| `/speckit.implement`    | Executar todas as tarefas para construir a funcionalidade de acordo com o plano |

#### Comandos Opcionais

Comandos adicionais para qualidade aprimorada e validação:

| Comando              | Descrição                                                                                                                                          |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `/speckit.clarify`   | Esclarecer áreas subespecificadas (recomendado antes de `/speckit.plan`; anteriormente `/quizme`)                                                   |
| `/speckit.analyze`   | Análise de consistência e cobertura entre artefatos (executar após `/speckit.tasks`, antes de `/speckit.implement`)                                |
| `/speckit.checklist` | Gerar checklists de qualidade personalizados que validam completude, clareza e consistência de requisitos (como "testes unitários para inglês")    |

### Variáveis de Ambiente

| Variável          | Descrição                                                                                                                                                                                                                                                                                                    |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `SPECIFY_FEATURE` | Sobrescrever detecção de funcionalidade para repositórios não-Git. Defina para o nome do diretório da funcionalidade (ex., `001-albuns-fotos`) para trabalhar em uma funcionalidade específica quando não estiver usando branches Git.<br/>\*\*Deve ser definido no contexto do agente que você está usando antes de usar `/speckit.plan` ou comandos subsequentes. |

## 📚 Filosofia Central

O Desenvolvimento Orientado por Especificação é um processo estruturado que enfatiza:

- **Desenvolvimento orientado por intenção** onde especificações definem o "*o quê*" antes do "*como*"
- **Criação rica de especificações** usando guardrails e princípios organizacionais
- **Refinamento em múltiplas etapas** em vez de geração de código em uma única tentativa a partir de prompts
- **Forte dependência** de capacidades avançadas de modelos de IA para interpretação de especificações

## 🌟 Fases de Desenvolvimento

| Fase                                      | Foco                          | Atividades Principais                                                                                                                                                              |
| ----------------------------------------- | ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Desenvolvimento 0-para-1** ("Greenfield")| Gerar do zero                  | <ul><li>Começar com requisitos de alto nível</li><li>Gerar especificações</li><li>Planejar etapas de implementação</li><li>Construir aplicações prontas para produção</li></ul>    |
| **Exploração Criativa**                    | Implementações paralelas       | <ul><li>Explorar soluções diversas</li><li>Suportar múltiplas stacks tecnológicas e arquiteturas</li><li>Experimentar com padrões de UX</li></ul>                                  |
| **Melhoria Iterativa** ("Brownfield")      | Modernização brownfield        | <ul><li>Adicionar funcionalidades iterativamente</li><li>Modernizar sistemas legados</li><li>Adaptar processos</li></ul>                                                           |

## 🎯 Objetivos Experimentais

Nossa pesquisa e experimentação focam em:

### Independência de tecnologia

- Criar aplicações usando stacks tecnológicas diversas
- Validar a hipótese de que o Desenvolvimento Orientado por Especificação é um processo não vinculado a tecnologias, linguagens de programação ou frameworks específicos

### Restrições empresariais

- Demonstrar desenvolvimento de aplicações de missão crítica
- Incorporar restrições organizacionais (provedores de nuvem, stacks tecnológicas, práticas de engenharia)
- Suportar design systems empresariais e requisitos de conformidade

### Desenvolvimento centrado no usuário

- Construir aplicações para diferentes coortes de usuários e preferências
- Suportar várias abordagens de desenvolvimento (de vibe-coding a desenvolvimento nativo de IA)

### Processos criativos e iterativos

- Validar o conceito de exploração de implementação paralela
- Fornecer fluxos de trabalho robustos de desenvolvimento iterativo de funcionalidades
- Estender processos para lidar com atualizações e tarefas de modernização

## 🔧 Pré-requisitos

- **Linux/macOS/Windows**
- Agente de codificação de IA [suportado](#-agentes-de-ia-suportados).
- [uv](https://docs.astral.sh/uv/) para gerenciamento de pacotes
- [Python 3.11+](https://www.python.org/downloads/)
- [Git](https://git-scm.com/downloads)

Se você encontrar problemas com um agente, por favor abra uma issue para que possamos refinar a integração.

## 📖 Saiba Mais

- **[Metodologia Completa de Desenvolvimento Orientado por Especificação](./spec-driven.pt-BR.md)** - Mergulho profundo no processo completo
- **[Passo a Passo Detalhado](#-processo-detalhado)** - Guia de implementação passo a passo

---

## 📋 Processo Detalhado

<details>
<summary>Clique para expandir o passo a passo detalhado</summary>

Você pode usar a CLI Specify para inicializar seu projeto, que trará os artefatos necessários em seu ambiente. Execute:

```bash
specify init <nome_do_projeto>
```

Ou inicialize no diretório atual:

```bash
specify init .
# ou use a flag --here
specify init --here
# Pular confirmação quando o diretório já tem arquivos
specify init . --force
# ou
specify init --here --force
```

![CLI Specify inicializando um novo projeto no terminal](./media/specify_cli.gif)

Você será solicitado a selecionar o agente de IA que está usando. Você também pode especificá-lo proativamente diretamente no terminal:

```bash
specify init <nome_do_projeto> --ai claude
specify init <nome_do_projeto> --ai gemini
specify init <nome_do_projeto> --ai copilot

# Ou no diretório atual:
specify init . --ai claude
specify init . --ai codex

# ou use a flag --here
specify init --here --ai claude
specify init --here --ai codex

# Forçar mesclagem em diretório atual não vazio
specify init . --force --ai claude

# ou
specify init --here --force --ai claude
```

A CLI verificará se você tem Claude Code, Gemini CLI, Cursor CLI, Qwen CLI, opencode, Codex CLI, Qoder CLI ou Amazon Q Developer CLI instalados. Se não tiver, ou se preferir obter os templates sem verificar as ferramentas certas, use `--ignore-agent-tools` com seu comando:

```bash
specify init <nome_do_projeto> --ai claude --ignore-agent-tools
```

### **PASSO 1:** Estabeleça os princípios do projeto

Vá para a pasta do projeto e execute seu agente de IA. Em nosso exemplo, estamos usando `claude`.

![Inicializando ambiente Claude Code](./media/bootstrap-claude-code.gif)

Você saberá que as coisas estão configuradas corretamente se vir os comandos `/speckit.constitution`, `/speckit.specify`, `/speckit.plan`, `/speckit.tasks` e `/speckit.implement` disponíveis.

O primeiro passo deve ser estabelecer os princípios de governança do seu projeto usando o comando `/speckit.constitution`. Isso ajuda a garantir tomada de decisão consistente ao longo de todas as fases de desenvolvimento subsequentes:

```text
/speckit.constitution Criar princípios focados em qualidade de código, padrões de teste, consistência de experiência do usuário e requisitos de performance. Incluir governança para como esses princípios devem guiar decisões técnicas e escolhas de implementação.
```

Este passo cria ou atualiza o arquivo `.specify/memory/constitution.md` com as diretrizes fundamentais do seu projeto que o agente de IA referenciará durante as fases de especificação, planejamento e implementação.

### **PASSO 2:** Crie as especificações do projeto

Com os princípios do seu projeto estabelecidos, você pode agora criar as especificações funcionais. Use o comando `/speckit.specify` e então forneça os requisitos concretos para o projeto que você quer desenvolver.

> [!IMPORTANT]
> Seja o mais explícito possível sobre *o que* você está tentando construir e *por quê*. **Não foque na stack tecnológica neste ponto**.

Um exemplo de prompt:

```text
Desenvolver Taskify, uma plataforma de produtividade de equipe. Deve permitir que usuários criem projetos, adicionem membros da equipe,
atribuam tarefas, comentem e movam tarefas entre quadros em estilo Kanban. Nesta fase inicial para esta funcionalidade,
vamos chamá-la de "Criar Taskify", vamos ter múltiplos usuários mas os usuários serão declarados antecipadamente, predefinidos.
Quero cinco usuários em duas categorias diferentes, um gerente de produto e quatro engenheiros. Vamos criar três
projetos de amostra diferentes. Vamos ter as colunas Kanban padrão para o status de cada tarefa, como "A Fazer",
"Em Progresso", "Em Revisão" e "Concluído". Não haverá login para esta aplicação pois isso é apenas o
primeiro teste para garantir que nossas funcionalidades básicas estão configuradas. Para cada tarefa na UI para um card de tarefa,
você deve poder mudar o status atual da tarefa entre as diferentes colunas no quadro de trabalho Kanban.
Você deve poder deixar um número ilimitado de comentários para um card específico. Você deve poder, a partir desse card de tarefa,
atribuir um dos usuários válidos. Quando você iniciar o Taskify pela primeira vez, ele vai te dar uma lista dos cinco usuários para escolher.
Não será necessária senha. Quando você clicar em um usuário, você vai para a visualização principal, que mostra a lista de
projetos. Quando você clica em um projeto, você abre o quadro Kanban para aquele projeto. Você vai ver as colunas.
Você poderá arrastar e soltar cards entre diferentes colunas. Você verá qualquer card que está
atribuído a você, o usuário atualmente logado, em uma cor diferente de todos os outros, para que você possa rapidamente
ver os seus. Você pode editar qualquer comentário que você fez, mas não pode editar comentários que outras pessoas fizeram. Você pode
deletar qualquer comentário que você fez, mas não pode deletar comentários que qualquer outra pessoa fez.
```

Após este prompt ser inserido, você deve ver o Claude Code iniciar o processo de planejamento e elaboração da especificação. O Claude Code também acionará alguns dos scripts internos para configurar o repositório.

Uma vez que este passo esteja completo, você deve ter uma nova branch criada (ex., `001-criar-taskify`), assim como uma nova especificação no diretório `specs/001-criar-taskify`.

A especificação produzida deve conter um conjunto de histórias de usuário e requisitos funcionais, conforme definido no template.

Neste estágio, o conteúdo da pasta do seu projeto deve se parecer com o seguinte:

```text
└── .specify
    ├── memory
    │  └── constitution.md
    ├── scripts
    │  ├── check-prerequisites.sh
    │  ├── common.sh
    │  ├── create-new-feature.sh
    │  ├── setup-plan.sh
    │  └── update-claude-md.sh
    ├── specs
    │  └── 001-criar-taskify
    │      └── spec.md
    └── templates
        ├── plan-template.md
        ├── spec-template.md
        └── tasks-template.md
```

### **PASSO 3:** Clarificação da especificação funcional (necessário antes do planejamento)

Com a especificação base criada, você pode ir em frente e clarificar quaisquer requisitos que não foram capturados adequadamente na primeira tentativa.

Você deve executar o fluxo de trabalho de clarificação estruturada **antes** de criar um plano técnico para reduzir retrabalho posteriormente.

Ordem preferida:

1. Use `/speckit.clarify` (estruturado) – questionamento sequencial baseado em cobertura que registra respostas em uma seção de Clarificações.
2. Opcionalmente acompanhe com refinamento ad-hoc de forma livre se algo ainda parecer vago.

Se você intencionalmente quiser pular a clarificação (ex., spike ou protótipo exploratório), declare isso explicitamente para que o agente não bloqueie em clarificações ausentes.

Exemplo de prompt de refinamento de forma livre (após `/speckit.clarify` se ainda necessário):

```text
Para cada projeto de amostra ou projeto que você criar deve haver um número variável de tarefas entre 5 e 15
tarefas para cada um distribuídas aleatoriamente em diferentes estados de conclusão. Certifique-se de que há pelo menos
uma tarefa em cada estágio de conclusão.
```

Você também deve pedir ao Claude Code para validar a **Checklist de Revisão e Aceite**, marcando as coisas que são validadas/passam nos requisitos, e deixar as que não são desmarcadas. O seguinte prompt pode ser usado:

```text
Leia a checklist de revisão e aceite, e marque cada item na checklist se a especificação da funcionalidade atende aos critérios. Deixe vazio se não atender.
```

É importante usar a interação com o Claude Code como uma oportunidade para clarificar e fazer perguntas sobre a especificação - **não trate sua primeira tentativa como final**.

### **PASSO 4:** Gere um plano

Agora você pode ser específico sobre a stack tecnológica e outros requisitos técnicos. Você pode usar o comando `/speckit.plan` que está embutido no template do projeto com um prompt assim:

```text
Vamos gerar isso usando .NET Aspire, usando Postgres como banco de dados. O frontend deve usar
Blazor server com quadros de tarefas drag-and-drop, atualizações em tempo real. Deve haver uma API REST criada com uma API de projetos,
API de tarefas e uma API de notificações.
```

A saída deste passo incluirá vários documentos de detalhes de implementação, com sua árvore de diretórios se parecendo com isto:

```text
.
├── CLAUDE.md
├── memory
│  └── constitution.md
├── scripts
│  ├── check-prerequisites.sh
│  ├── common.sh
│  ├── create-new-feature.sh
│  ├── setup-plan.sh
│  └── update-claude-md.sh
├── specs
│  └── 001-criar-taskify
│      ├── contracts
│      │  ├── api-spec.json
│      │  └── signalr-spec.md
│      ├── data-model.md
│      ├── plan.md
│      ├── quickstart.md
│      ├── research.md
│      └── spec.md
└── templates
    ├── CLAUDE-template.md
    ├── plan-template.md
    ├── spec-template.md
    └── tasks-template.md
```

Verifique o documento `research.md` para garantir que a stack tecnológica correta está sendo usada, baseada em suas instruções. Você pode pedir ao Claude Code para refiná-lo se algum dos componentes se destacar, ou até mesmo fazê-lo verificar a versão instalada localmente da plataforma/framework que você quer usar (ex., .NET).

Adicionalmente, você pode querer pedir ao Claude Code para pesquisar detalhes sobre a stack tecnológica escolhida se for algo que está mudando rapidamente (ex., .NET Aspire, frameworks JS), com um prompt assim:

```text
Quero que você analise o plano de implementação e os detalhes de implementação, procurando por áreas que poderiam
se beneficiar de pesquisa adicional já que .NET Aspire é uma biblioteca que muda rapidamente. Para essas áreas que você identificar que
requerem mais pesquisa, quero que você atualize o documento de pesquisa com detalhes adicionais sobre as versões específicas
que vamos usar nesta aplicação Taskify e inicie tarefas de pesquisa paralelas para clarificar
quaisquer detalhes usando pesquisa da web.
```

Durante este processo, você pode descobrir que o Claude Code fica preso pesquisando a coisa errada - você pode ajudá-lo a ir na direção certa com um prompt assim:

```text
Acho que precisamos dividir isso em uma série de passos. Primeiro, identifique uma lista de tarefas
que você precisaria fazer durante a implementação sobre as quais você não tem certeza ou que se beneficiariam
de mais pesquisa. Escreva uma lista dessas tarefas. E então para cada uma dessas tarefas,
quero que você inicie uma tarefa de pesquisa separada para que o resultado líquido seja que estamos pesquisando
todas essas tarefas muito específicas em paralelo. O que vi você fazendo parece que você estava
pesquisando .NET Aspire em geral e não acho que isso vai nos ajudar muito neste caso.
Essa é uma pesquisa muito pouco direcionada. A pesquisa precisa ajudá-lo a resolver uma pergunta específica e direcionada.
```

> [!NOTE]
> O Claude Code pode ser excessivamente ansioso e adicionar componentes que você não pediu. Peça para ele clarificar a razão e a fonte da mudança.

### **PASSO 5:** Faça o Claude Code validar o plano

Com o plano em vigor, você deve fazer o Claude Code revisá-lo para garantir que não há peças faltando. Você pode usar um prompt assim:

```text
Agora quero que você analise e audite o plano de implementação e os arquivos de detalhes de implementação.
Leia com um olhar para determinar se há ou não uma sequência de tarefas que você precisa
fazer que são óbvias da leitura. Porque não sei se há o suficiente aqui. Por exemplo,
quando olho para a implementação principal, seria útil referenciar os lugares apropriados nos detalhes de implementação
onde pode encontrar a informação enquanto percorre cada passo na implementação principal ou no refinamento.
```

Isso ajuda a refinar o plano de implementação e ajuda você a evitar pontos cegos potenciais que o Claude Code perdeu em seu ciclo de planejamento. Uma vez que a passagem inicial de refinamento esteja completa, peça ao Claude Code para passar pela checklist mais uma vez antes que você possa ir para a implementação.

Você também pode pedir ao Claude Code (se você tiver a [CLI do GitHub](https://docs.github.com/pt/github-cli/github-cli) instalada) para criar um pull request da sua branch atual para `main` com uma descrição detalhada, para garantir que o esforço seja adequadamente rastreado.

> [!NOTE]
> Antes de você ter o agente implementando, também vale a pena pedir ao Claude Code para verificar os detalhes para ver se há peças super-engenheiradas (lembre-se - pode ser excessivamente ansioso). Se existirem componentes ou decisões super-engenheirados, você pode pedir ao Claude Code para resolvê-los. Garanta que o Claude Code siga a [constituição](base/memory/constitution.md) como a peça fundamental que deve aderir ao estabelecer o plano.

### **PASSO 6:** Gere a divisão de tarefas com /speckit.tasks

Com o plano de implementação validado, agora você pode dividir o plano em tarefas específicas e acionáveis que podem ser executadas na ordem correta. Use o comando `/speckit.tasks` para gerar automaticamente uma divisão detalhada de tarefas a partir do seu plano de implementação:

```text
/speckit.tasks
```

Este passo cria um arquivo `tasks.md` no diretório de especificação da sua funcionalidade que contém:

- **Divisão de tarefas organizada por história de usuário** - Cada história de usuário se torna uma fase de implementação separada com seu próprio conjunto de tarefas
- **Gerenciamento de dependências** - Tarefas são ordenadas para respeitar dependências entre componentes (ex., modelos antes de serviços, serviços antes de endpoints)
- **Marcadores de execução paralela** - Tarefas que podem rodar em paralelo são marcadas com `[P]` para otimizar o fluxo de trabalho de desenvolvimento
- **Especificações de caminho de arquivo** - Cada tarefa inclui os caminhos exatos dos arquivos onde a implementação deve ocorrer
- **Estrutura de desenvolvimento orientado por testes** - Se testes são solicitados, tarefas de teste são incluídas e ordenadas para serem escritas antes da implementação
- **Validação de checkpoint** - Cada fase de história de usuário inclui checkpoints para validar funcionalidade independente

O tasks.md gerado fornece um roteiro claro para o comando `/speckit.implement`, garantindo implementação sistemática que mantém a qualidade do código e permite entrega incremental de histórias de usuário.

### **PASSO 7:** Implementação

Uma vez pronto, use o comando `/speckit.implement` para executar seu plano de implementação:

```text
/speckit.implement
```

O comando `/speckit.implement` irá:

- Validar que todos os pré-requisitos estão em vigor (constituição, especificação, plano e tarefas)
- Analisar a divisão de tarefas do `tasks.md`
- Executar tarefas na ordem correta, respeitando dependências e marcadores de execução paralela
- Seguir a abordagem TDD definida no seu plano de tarefas
- Fornecer atualizações de progresso e tratar erros apropriadamente

> [!IMPORTANT]
> O agente de IA executará comandos CLI locais (como `dotnet`, `npm`, etc.) - certifique-se de ter as ferramentas necessárias instaladas em sua máquina.

Uma vez que a implementação esteja completa, teste a aplicação e resolva quaisquer erros de runtime que podem não ser visíveis nos logs da CLI (ex., erros no console do navegador). Você pode copiar e colar esses erros de volta para seu agente de IA para resolução.

</details>

---

## 🔍 Solução de Problemas

### Git Credential Manager no Linux

Se você está tendo problemas com autenticação do Git no Linux, você pode instalar o Git Credential Manager:

```bash
#!/usr/bin/env bash
set -e
echo "Baixando Git Credential Manager v2.6.1..."
wget https://github.com/git-ecosystem/git-credential-manager/releases/download/v2.6.1/gcm-linux_amd64.2.6.1.deb
echo "Instalando Git Credential Manager..."
sudo dpkg -i gcm-linux_amd64.2.6.1.deb
echo "Configurando Git para usar GCM..."
git config --global credential.helper manager
echo "Limpando..."
rm gcm-linux_amd64.2.6.1.deb
```

## 👥 Mantenedores

- Den Delimarsky ([@localden](https://github.com/localden))
- John Lam ([@jflam](https://github.com/jflam))

## 💬 Suporte

Para suporte, por favor abra uma [issue no GitHub](https://github.com/github/spec-kit/issues/new). Aceitamos relatórios de bugs, solicitações de funcionalidades e perguntas sobre o uso do Desenvolvimento Orientado por Especificação.

## 🙏 Agradecimentos

Este projeto é fortemente influenciado e baseado no trabalho e pesquisa de [John Lam](https://github.com/jflam).

## 📄 Licença

Este projeto está licenciado sob os termos da licença open source MIT. Por favor, consulte o arquivo [LICENSE](./LICENSE) para os termos completos.
