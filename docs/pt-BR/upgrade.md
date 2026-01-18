> 🇧🇷 Esta documentação está em Português do Brasil. [English version](../upgrade.md)

# Guia de Atualização

> Você tem o Spec Kit instalado e quer atualizar para a versão mais recente para obter novas funcionalidades, correções de bugs ou slash commands atualizados. Este guia cobre tanto a atualização da ferramenta CLI quanto a atualização dos arquivos do seu projeto.

---

## Referência Rápida

| O Que Atualizar | Comando | Quando Usar |
|----------------|---------|-------------|
| **Apenas a Ferramenta CLI** | `uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git` | Obter as últimas funcionalidades da CLI sem tocar nos arquivos do projeto |
| **Arquivos do Projeto** | `specify init --here --force --ai <seu-agente>` | Atualizar slash commands, templates e scripts no seu projeto |
| **Ambos** | Execute a atualização da CLI, depois a atualização do projeto | Recomendado para atualizações de versão major |

---

## Parte 1: Atualizar a Ferramenta CLI

A ferramenta CLI (`specify`) é separada dos arquivos do seu projeto. Atualize-a para obter as últimas funcionalidades e correções de bugs.

### Se você instalou com `uv tool install`

```bash
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git
```

### Se você usa comandos `uvx` únicos

Nenhuma atualização necessária—`uvx` sempre busca a versão mais recente. Apenas execute seus comandos normalmente:

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init --here --ai copilot
```

### Verificar a atualização

```bash
specify check
```

Isso mostra as ferramentas instaladas e confirma que a CLI está funcionando.

---

## Parte 2: Atualização dos Arquivos do Projeto

Quando o Spec Kit lança novas funcionalidades (como novos slash commands ou templates atualizados), você precisa atualizar os arquivos do Spec Kit do seu projeto.

### O que é atualizado?

Executar `specify init --here --force` irá atualizar:

- ✅ **Arquivos de slash command** (`.claude/commands/`, `.github/prompts/`, etc.)
- ✅ **Arquivos de script** (`.specify/scripts/`)
- ✅ **Arquivos de template** (`.specify/templates/`)
- ✅ **Arquivos de memória compartilhada** (`.specify/memory/`) - **⚠️ Veja avisos abaixo**

### O que permanece seguro?

Estes arquivos **nunca são tocados** pela atualização—os pacotes de template nem os contêm:

- ✅ **Suas especificações** (`specs/001-minha-funcionalidade/spec.md`, etc.) - **CONFIRMADO SEGURO**
- ✅ **Seus planos de implementação** (`specs/001-minha-funcionalidade/plan.md`, `tasks.md`, etc.) - **CONFIRMADO SEGURO**
- ✅ **Seu código fonte** - **CONFIRMADO SEGURO**
- ✅ **Seu histórico git** - **CONFIRMADO SEGURO**

O diretório `specs/` é completamente excluído dos pacotes de template e nunca será modificado durante atualizações.

### Comando de atualização

Execute isso dentro do diretório do seu projeto:

```bash
specify init --here --force --ai <seu-agente>
```

Substitua `<seu-agente>` pelo seu assistente de IA. Consulte esta lista de [Agentes de IA Suportados](../../README.pt-BR.md#-agentes-de-ia-suportados)

**Exemplo:**

```bash
specify init --here --force --ai copilot
```

### Entendendo a flag `--force`

Sem `--force`, a CLI avisa você e pede confirmação:

```text
Aviso: O diretório atual não está vazio (25 itens)
Arquivos de template serão mesclados com o conteúdo existente e podem sobrescrever arquivos existentes
Continuar? [y/N]
```

Com `--force`, ele pula a confirmação e prossegue imediatamente.

**Importante: Seu diretório `specs/` está sempre seguro.** A flag `--force` afeta apenas arquivos de template (comandos, scripts, templates, memória). Suas especificações de funcionalidade, planos e tarefas em `specs/` nunca são incluídos em pacotes de atualização e não podem ser sobrescritos.

---

## ⚠️ Avisos Importantes

### 1. O arquivo de constituição será sobrescrito

**Problema conhecido:** `specify init --here --force` atualmente sobrescreve `.specify/memory/constitution.md` com o template padrão, apagando quaisquer customizações que você fez.

**Solução alternativa:**

```bash
# 1. Faça backup da sua constituição antes de atualizar
cp .specify/memory/constitution.md .specify/memory/constitution-backup.md

# 2. Execute a atualização
specify init --here --force --ai copilot

# 3. Restaure sua constituição customizada
mv .specify/memory/constitution-backup.md .specify/memory/constitution.md
```

Ou use git para restaurá-la:

```bash
# Após a atualização, restaure do histórico git
git restore .specify/memory/constitution.md
```

### 2. Modificações customizadas de templates

Se você customizou quaisquer templates em `.specify/templates/`, a atualização irá sobrescrevê-los. Faça backup primeiro:

```bash
# Faça backup de templates customizados
cp -r .specify/templates .specify/templates-backup

# Após a atualização, mescle suas alterações de volta manualmente
```

### 3. Slash commands duplicados (agentes baseados em IDE)

Alguns agentes baseados em IDE (como Kilo Code, Windsurf) podem mostrar **slash commands duplicados** após a atualização—tanto versões antigas quanto novas aparecem.

**Solução:** Exclua manualmente os arquivos de comando antigos da pasta do seu agente.

**Exemplo para Kilo Code:**

```bash
# Navegue até a pasta de comandos do agente
cd .kilocode/rules/

# Liste os arquivos e identifique duplicatas
ls -la

# Exclua versões antigas (nomes de arquivo de exemplo - os seus podem ser diferentes)
rm speckit.specify-old.md
rm speckit.plan-v1.md
```

Reinicie sua IDE para atualizar a lista de comandos.

---

## Cenários Comuns

### Cenário 1: "Só quero novos slash commands"

```bash
# Atualizar CLI (se usando instalação persistente)
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git

# Atualizar arquivos do projeto para obter novos comandos
specify init --here --force --ai copilot

# Restaurar sua constituição se customizada
git restore .specify/memory/constitution.md
```

### Cenário 2: "Customizei templates e constituição"

```bash
# 1. Faça backup das customizações
cp .specify/memory/constitution.md /tmp/constitution-backup.md
cp -r .specify/templates /tmp/templates-backup

# 2. Atualize CLI
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git

# 3. Atualize projeto
specify init --here --force --ai copilot

# 4. Restaure customizações
mv /tmp/constitution-backup.md .specify/memory/constitution.md
# Mescle alterações de template manualmente se necessário
```

### Cenário 3: "Vejo slash commands duplicados na minha IDE"

Isso acontece com agentes baseados em IDE (Kilo Code, Windsurf, Roo Code, etc.).

```bash
# Encontre a pasta do agente (exemplo: .kilocode/rules/)
cd .kilocode/rules/

# Liste todos os arquivos
ls -la

# Exclua arquivos de comando antigos
rm speckit.old-command-name.md

# Reinicie sua IDE
```

### Cenário 4: "Estou trabalhando em um projeto sem Git"

Se você inicializou seu projeto com `--no-git`, ainda pode atualizar:

```bash
# Faça backup manualmente dos arquivos que você customizou
cp .specify/memory/constitution.md /tmp/constitution-backup.md

# Execute a atualização
specify init --here --force --ai copilot --no-git

# Restaure customizações
mv /tmp/constitution-backup.md .specify/memory/constitution.md
```

A flag `--no-git` pula a inicialização do git mas não afeta atualizações de arquivos.

---

## Usando a Flag `--no-git`

A flag `--no-git` diz ao Spec Kit para **pular a inicialização do repositório git**. Isso é útil quando:

- Você gerencia controle de versão de forma diferente (Mercurial, SVN, etc.)
- Seu projeto faz parte de um monorepo maior com configuração git existente
- Você está experimentando e não quer controle de versão ainda

**Durante a configuração inicial:**

```bash
specify init meu-projeto --ai copilot --no-git
```

**Durante a atualização:**

```bash
specify init --here --force --ai copilot --no-git
```

### O que `--no-git` NÃO faz

❌ NÃO impede atualizações de arquivos
❌ NÃO pula instalação de slash commands
❌ NÃO afeta mesclagem de templates

Ele **apenas** pula executar `git init` e criar o commit inicial.

### Trabalhando sem Git

Se você usa `--no-git`, precisará gerenciar diretórios de funcionalidade manualmente:

**Defina a variável de ambiente `SPECIFY_FEATURE`** antes de usar comandos de planejamento:

```bash
# Bash/Zsh
export SPECIFY_FEATURE="001-minha-funcionalidade"

# PowerShell
$env:SPECIFY_FEATURE = "001-minha-funcionalidade"
```

Isso diz ao Spec Kit qual diretório de funcionalidade usar ao criar especificações, planos e tarefas.

**Por que isso importa:** Sem git, o Spec Kit não pode detectar o nome da sua branch atual para determinar a funcionalidade ativa. A variável de ambiente fornece esse contexto manualmente.

---

## Solução de Problemas

### "Slash commands não aparecem após atualização"

**Causa:** O agente não recarregou os arquivos de comando.

**Solução:**

1. **Reinicie completamente sua IDE/editor** (não apenas recarregar janela)
2. **Para agentes baseados em CLI**, verifique se os arquivos existem:

   ```bash
   ls -la .claude/commands/      # Claude Code
   ls -la .gemini/commands/       # Gemini
   ls -la .cursor/commands/       # Cursor
   ```

3. **Verifique configuração específica do agente:**
   - Codex requer variável de ambiente `CODEX_HOME`
   - Alguns agentes precisam reiniciar workspace ou limpar cache

### "Perdi minhas customizações de constituição"

**Solução:** Restaure do git ou backup:

```bash
# Se você commitou antes de atualizar
git restore .specify/memory/constitution.md

# Se você fez backup manualmente
cp /tmp/constitution-backup.md .specify/memory/constitution.md
```

**Prevenção:** Sempre commite ou faça backup de `constitution.md` antes de atualizar.

### "Aviso: O diretório atual não está vazio"

**Mensagem de aviso completa:**

```text
Aviso: O diretório atual não está vazio (25 itens)
Arquivos de template serão mesclados com o conteúdo existente e podem sobrescrever arquivos existentes
Deseja continuar? [y/N]
```

**O que isso significa:**

Este aviso aparece quando você executa `specify init --here` (ou `specify init .`) em um diretório que já tem arquivos. Está dizendo:

1. **O diretório tem conteúdo existente** - No exemplo, 25 arquivos/pastas
2. **Arquivos serão mesclados** - Novos arquivos de template serão adicionados junto com seus arquivos existentes
3. **Alguns arquivos podem ser sobrescritos** - Se você já tem arquivos do Spec Kit (`.claude/`, `.specify/`, etc.), eles serão substituídos pelas novas versões

**O que é sobrescrito:**

Apenas arquivos de infraestrutura do Spec Kit:

- Arquivos de comando do agente (`.claude/commands/`, `.github/prompts/`, etc.)
- Scripts em `.specify/scripts/`
- Templates em `.specify/templates/`
- Arquivos de memória em `.specify/memory/` (incluindo constituição)

**O que permanece intocado:**

- Seu diretório `specs/` (especificações, planos, tarefas)
- Seus arquivos de código fonte
- Seu diretório `.git/` e histórico git
- Quaisquer outros arquivos que não fazem parte dos templates do Spec Kit

**Como responder:**

- **Digite `y` e pressione Enter** - Prosseguir com a mesclagem (recomendado se estiver atualizando)
- **Digite `n` e pressione Enter** - Cancelar a operação
- **Use a flag `--force`** - Pular esta confirmação completamente:

  ```bash
  specify init --here --force --ai copilot
  ```

**Quando você vê este aviso:**

- ✅ **Esperado** ao atualizar um projeto Spec Kit existente
- ✅ **Esperado** ao adicionar Spec Kit a uma base de código existente
- ⚠️ **Inesperado** se você pensou que estava criando um novo projeto em um diretório vazio

**Dica de prevenção:** Antes de atualizar, commite ou faça backup do seu `.specify/memory/constitution.md` se você o customizou.

### "Atualização da CLI não parece funcionar"

Verifique a instalação:

```bash
# Verifique ferramentas instaladas
uv tool list

# Deve mostrar specify-cli

# Verifique o caminho
which specify

# Deve apontar para o diretório de instalação do uv tool
```

Se não encontrado, reinstale:

```bash
uv tool uninstall specify-cli
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

### "Preciso executar specify toda vez que abro meu projeto?"

**Resposta curta:** Não, você só executa `specify init` uma vez por projeto (ou ao atualizar).

**Explicação:**

A ferramenta CLI `specify` é usada para:

- **Configuração inicial:** `specify init` para inicializar o Spec Kit no seu projeto
- **Atualizações:** `specify init --here --force` para atualizar templates e comandos
- **Diagnósticos:** `specify check` para verificar instalação de ferramentas

Uma vez que você executou `specify init`, os slash commands (como `/speckit.specify`, `/speckit.plan`, etc.) são **permanentemente instalados** na pasta do agente do seu projeto (`.claude/`, `.github/prompts/`, etc.). Seu assistente de IA lê esses arquivos de comando diretamente—não há necessidade de executar `specify` novamente.

**Se seu agente não está reconhecendo slash commands:**

1. **Verifique se os arquivos de comando existem:**

   ```bash
   # Para GitHub Copilot
   ls -la .github/prompts/

   # Para Claude
   ls -la .claude/commands/
   ```

2. **Reinicie completamente sua IDE/editor** (não apenas recarregar janela)

3. **Verifique se você está no diretório correto** onde executou `specify init`

4. **Para alguns agentes**, pode ser necessário recarregar o workspace ou limpar cache

**Problema relacionado:** Se o Copilot não consegue abrir arquivos locais ou usa comandos PowerShell inesperadamente, isso é tipicamente um problema de contexto da IDE, não relacionado ao `specify`. Tente:

- Reiniciar o VS Code
- Verificar permissões de arquivo
- Garantir que a pasta do workspace está aberta corretamente

---

## Compatibilidade de Versão

O Spec Kit segue versionamento semântico para lançamentos major. A CLI e os arquivos do projeto são projetados para serem compatíveis dentro da mesma versão major.

**Melhor prática:** Mantenha tanto a CLI quanto os arquivos do projeto sincronizados atualizando ambos juntos durante mudanças de versão major.

---

## Próximos Passos

Após atualizar:

- **Teste novos slash commands:** Execute `/speckit.constitution` ou outro comando para verificar se tudo funciona
- **Revise notas de lançamento:** Verifique [GitHub Releases](https://github.com/github/spec-kit/releases) para novas funcionalidades e mudanças breaking
- **Atualize fluxos de trabalho:** Se novos comandos foram adicionados, atualize os fluxos de trabalho de desenvolvimento da sua equipe
- **Verifique documentação:** Visite [github.io/spec-kit](https://github.github.io/spec-kit/) para guias atualizados
