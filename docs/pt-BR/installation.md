> 🇧🇷 Esta documentação está em Português do Brasil. [English version](../installation.md)

# Guia de Instalação

## Pré-requisitos

- **Linux/macOS** (ou Windows; scripts PowerShell agora suportados sem WSL)
- Agente de codificação de IA: [Claude Code](https://www.anthropic.com/claude-code), [GitHub Copilot](https://code.visualstudio.com/), [Codebuddy CLI](https://www.codebuddy.ai/cli) ou [Gemini CLI](https://github.com/google-gemini/gemini-cli)
- [uv](https://docs.astral.sh/uv/) para gerenciamento de pacotes
- [Python 3.11+](https://www.python.org/downloads/)
- [Git](https://git-scm.com/downloads)

## Instalação

### Inicializar um Novo Projeto

A maneira mais fácil de começar é inicializar um novo projeto:

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <NOME_DO_PROJETO>
```

Ou inicialize no diretório atual:

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init .
# ou use a flag --here
uvx --from git+https://github.com/github/spec-kit.git specify init --here
```

### Especificar Agente de IA

Você pode especificar proativamente seu agente de IA durante a inicialização:

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <nome_do_projeto> --ai claude
uvx --from git+https://github.com/github/spec-kit.git specify init <nome_do_projeto> --ai gemini
uvx --from git+https://github.com/github/spec-kit.git specify init <nome_do_projeto> --ai copilot
uvx --from git+https://github.com/github/spec-kit.git specify init <nome_do_projeto> --ai codebuddy
```

### Especificar Tipo de Script (Shell vs PowerShell)

Todos os scripts de automação agora têm variantes Bash (`.sh`) e PowerShell (`.ps1`).

Comportamento automático:

- Windows padrão: `ps`
- Outros SO padrão: `sh`
- Modo interativo: você será perguntado, a menos que passe `--script`

Forçar um tipo de script específico:

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <nome_do_projeto> --script sh
uvx --from git+https://github.com/github/spec-kit.git specify init <nome_do_projeto> --script ps
```

### Ignorar Verificação de Ferramentas de Agente

Se você preferir obter os templates sem verificar as ferramentas corretas:

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <nome_do_projeto> --ai claude --ignore-agent-tools
```

## Verificação

Após a inicialização, você deve ver os seguintes comandos disponíveis no seu agente de IA:

- `/speckit.specify` - Criar especificações
- `/speckit.plan` - Gerar planos de implementação
- `/speckit.tasks` - Dividir em tarefas acionáveis

O diretório `.specify/scripts` conterá tanto scripts `.sh` quanto `.ps1`.

## Solução de Problemas

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
