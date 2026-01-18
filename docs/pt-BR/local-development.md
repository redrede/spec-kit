> 🇧🇷 Esta documentação está em Português do Brasil. [English version](../local-development.md)

# Guia de Desenvolvimento Local

Este guia mostra como iterar na CLI `specify` localmente sem publicar um release ou commitar para `main` primeiro.

> Scripts agora têm variantes Bash (`.sh`) e PowerShell (`.ps1`). A CLI seleciona automaticamente baseado no SO, a menos que você passe `--script sh|ps`.

## 1. Clone e Troque de Branch

```bash
git clone https://github.com/github/spec-kit.git
cd spec-kit
# Trabalhe em uma branch de funcionalidade
git checkout -b sua-branch-funcionalidade
```

## 2. Execute a CLI Diretamente (Feedback Mais Rápido)

Você pode executar a CLI através do entrypoint do módulo sem instalar nada:

```bash
# Da raiz do repositório
python -m src.specify_cli --help
python -m src.specify_cli init projeto-demo --ai claude --ignore-agent-tools --script sh
```

Se você preferir invocar no estilo de arquivo de script (usa shebang):

```bash
python src/specify_cli/__init__.py init projeto-demo --script ps
```

## 3. Use Instalação Editável (Ambiente Isolado)

Crie um ambiente isolado usando `uv` para que as dependências resolvam exatamente como os usuários finais as obtêm:

```bash
# Crie e ative o ambiente virtual (uv gerencia automaticamente .venv)
uv venv
source .venv/bin/activate  # ou no Windows PowerShell: .venv\Scripts\Activate.ps1

# Instale o projeto em modo editável
uv pip install -e .

# Agora o entrypoint 'specify' está disponível
specify --help
```

Re-executar após edições de código não requer reinstalação por causa do modo editável.

## 4. Invoque com uvx Diretamente do Git (Branch Atual)

`uvx` pode executar de um caminho local (ou uma ref Git) para simular fluxos de usuário:

```bash
uvx --from . specify init demo-uvx --ai copilot --ignore-agent-tools --script sh
```

Você também pode apontar uvx para uma branch específica sem fazer merge:

```bash
# Faça push da sua branch de trabalho primeiro
git push origin sua-branch-funcionalidade
uvx --from git+https://github.com/github/spec-kit.git@sua-branch-funcionalidade specify init demo-teste-branch --script ps
```

### 4a. uvx com Caminho Absoluto (Execute de Qualquer Lugar)

Se você está em outro diretório, use um caminho absoluto em vez de `.`:

```bash
uvx --from /mnt/c/GitHub/spec-kit specify --help
uvx --from /mnt/c/GitHub/spec-kit specify init demo-qualquerlugar --ai copilot --ignore-agent-tools --script sh
```

Defina uma variável de ambiente para conveniência:

```bash
export SPEC_KIT_SRC=/mnt/c/GitHub/spec-kit
uvx --from "$SPEC_KIT_SRC" specify init demo-env --ai copilot --ignore-agent-tools --script ps
```

(Opcional) Defina uma função de shell:

```bash
specify-dev() { uvx --from /mnt/c/GitHub/spec-kit specify "$@"; }
# Então
specify-dev --help
```

## 5. Testando Lógica de Permissão de Script

Após executar um `init`, verifique se os scripts shell são executáveis em sistemas POSIX:

```bash
ls -l scripts | grep .sh
# Espere bit de execução do proprietário (ex. -rwxr-xr-x)
```

No Windows você usará os scripts `.ps1` (não precisa de chmod).

## 6. Execute Lint / Verificações Básicas (Adicione as Suas)

Atualmente nenhuma configuração de lint imposta é incluída, mas você pode fazer uma verificação rápida de importabilidade:

```bash
python -c "import specify_cli; print('Import OK')"
```

## 7. Construa uma Wheel Localmente (Opcional)

Valide o empacotamento antes de publicar:

```bash
uv build
ls dist/
```

Instale o artefato construído em um ambiente temporário novo se necessário.

## 8. Usando um Workspace Temporário

Ao testar `init --here` em um diretório sujo, crie um workspace temporário:

```bash
mkdir /tmp/spec-test && cd /tmp/spec-test
python -m src.specify_cli init --here --ai claude --ignore-agent-tools --script sh  # se o repositório foi copiado aqui
```

Ou copie apenas a parte modificada da CLI se quiser um sandbox mais leve.

## 9. Debug de Rede / Pulos de TLS

Se você precisar ignorar validação TLS enquanto experimenta:

```bash
specify check --skip-tls
specify init demo --skip-tls --ai gemini --ignore-agent-tools --script ps
```

(Use apenas para experimentação local.)

## 10. Resumo do Loop de Edição Rápida

| Ação | Comando |
|------|---------|
| Executar CLI diretamente | `python -m src.specify_cli --help` |
| Instalação editável | `uv pip install -e .` então `specify ...` |
| Execução uvx local (raiz do repo) | `uvx --from . specify ...` |
| Execução uvx local (caminho abs) | `uvx --from /mnt/c/GitHub/spec-kit specify ...` |
| uvx de branch Git | `uvx --from git+URL@branch specify ...` |
| Construir wheel | `uv build` |

## 11. Limpando

Remova artefatos de build / ambiente virtual rapidamente:

```bash
rm -rf .venv dist build *.egg-info
```

## 12. Problemas Comuns

| Sintoma | Solução |
|---------|---------|
| `ModuleNotFoundError: typer` | Execute `uv pip install -e .` |
| Scripts não executáveis (Linux) | Re-execute init ou `chmod +x scripts/*.sh` |
| Passo Git pulado | Você passou `--no-git` ou Git não está instalado |
| Tipo de script errado baixado | Passe `--script sh` ou `--script ps` explicitamente |
| Erros TLS em rede corporativa | Tente `--skip-tls` (não para produção) |

## 13. Próximos Passos

- Atualize docs e execute o Quick Start usando sua CLI modificada
- Abra um PR quando estiver satisfeito
- (Opcional) Crie uma tag de release quando as mudanças chegarem em `main`
