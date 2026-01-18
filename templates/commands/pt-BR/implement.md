---
description: Executar o plano de implementação processando e executando todas as tarefas definidas em tasks.md
scripts:
  sh: scripts/bash/check-prerequisites.sh --json --require-tasks --include-tasks
  ps: scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks
---

## Diretiva de Idioma

**IMPORTANTE**: Gere TODO o conteúdo em Português do Brasil. Isso inclui:
- Relatórios de progresso
- Mensagens de erro e status
- Validações de checkpoint
- Comunicações com o usuário

Mantenha em inglês apenas: IDs de tarefas (T001), código fonte, nomes de arquivo, identificadores técnicos, e termos técnicos universais quando apropriado.

## Entrada do Usuário

```text
$ARGUMENTS
```

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Roteiro

1. Execute `{SCRIPT}` da raiz do repositório e analise FEATURE_DIR e lista AVAILABLE_DOCS. Todos os caminhos devem ser absolutos. Para aspas simples em args como "I'm Groot", use sintaxe de escape: ex 'I'\''m Groot' (ou aspas duplas se possível: "I'm Groot").

2. **Verificar status dos checklists** (se FEATURE_DIR/checklists/ existir):
   - Escaneie todos os arquivos de checklist no diretório checklists/
   - Para cada checklist, conte:
     - Itens totais: Todas as linhas correspondendo `- [ ]` ou `- [X]` ou `- [x]`
     - Itens completos: Linhas correspondendo `- [X]` ou `- [x]`
     - Itens incompletos: Linhas correspondendo `- [ ]`
   - Crie tabela de status:

     ```text
     | Checklist | Total | Completos | Incompletos | Status |
     |-----------|-------|-----------|-------------|--------|
     | ux.md     | 12    | 12        | 0           | ✓ PASSA |
     | teste.md  | 8     | 5         | 3           | ✗ FALHA |
     | seguranca.md | 6  | 6         | 0           | ✓ PASSA |
     ```

   - Calcule status geral:
     - **PASSA**: Todos os checklists têm 0 itens incompletos
     - **FALHA**: Um ou mais checklists têm itens incompletos

   - **Se algum checklist estiver incompleto**:
     - Exiba a tabela com contagens de itens incompletos
     - **PARE** e pergunte: "Alguns checklists estão incompletos. Você quer prosseguir com a implementação mesmo assim? (sim/não)"
     - Aguarde resposta do usuário antes de continuar
     - Se usuário disser "não" ou "aguarde" ou "pare", interrompa execução
     - Se usuário disser "sim" ou "prossiga" ou "continue", prossiga para passo 3

   - **Se todos os checklists estiverem completos**:
     - Exiba a tabela mostrando todos os checklists passaram
     - Automaticamente prossiga para passo 3

3. Carregue e analise o contexto de implementação:
   - **OBRIGATÓRIO**: Leia tasks.md para a lista completa de tarefas e plano de execução
   - **OBRIGATÓRIO**: Leia plan.md para stack tecnológica, arquitetura, e estrutura de arquivos
   - **SE EXISTIR**: Leia data-model.md para entidades e relacionamentos
   - **SE EXISTIR**: Leia contracts/ para especificações de API e requisitos de teste
   - **SE EXISTIR**: Leia research.md para decisões técnicas e restrições
   - **SE EXISTIR**: Leia quickstart.md para cenários de integração

4. **Verificação de Setup do Projeto**:
   - **OBRIGATÓRIO**: Crie/verifique arquivos de ignore baseado no setup real do projeto:

   **Lógica de Detecção & Criação**:
   - Verifique se o seguinte comando tem sucesso para determinar se o repositório é um git repo (crie/verifique .gitignore se sim):

     ```sh
     git rev-parse --git-dir 2>/dev/null
     ```

   - Verifique se Dockerfile* existe ou Docker no plan.md → crie/verifique .dockerignore
   - Verifique se .eslintrc* existe → crie/verifique .eslintignore
   - Verifique se eslint.config.* existe → garanta que entradas `ignores` do config cobrem padrões requeridos
   - Verifique se .prettierrc* existe → crie/verifique .prettierignore
   - Verifique se .npmrc ou package.json existe → crie/verifique .npmignore (se publicando)
   - Verifique se arquivos terraform (*.tf) existem → crie/verifique .terraformignore
   - Verifique se .helmignore necessário (helm charts presentes) → crie/verifique .helmignore

   **Se arquivo de ignore já existe**: Verifique se contém padrões essenciais, adicione apenas padrões críticos faltantes
   **Se arquivo de ignore faltando**: Crie com conjunto completo de padrões para tecnologia detectada

   **Padrões Comuns por Tecnologia** (do stack tecnológico do plan.md):
   - **Node.js/JavaScript/TypeScript**: `node_modules/`, `dist/`, `build/`, `*.log`, `.env*`
   - **Python**: `__pycache__/`, `*.pyc`, `.venv/`, `venv/`, `dist/`, `*.egg-info/`
   - **Java**: `target/`, `*.class`, `*.jar`, `.gradle/`, `build/`
   - **C#/.NET**: `bin/`, `obj/`, `*.user`, `*.suo`, `packages/`
   - **Go**: `*.exe`, `*.test`, `vendor/`, `*.out`
   - **Ruby**: `.bundle/`, `log/`, `tmp/`, `*.gem`, `vendor/bundle/`
   - **PHP**: `vendor/`, `*.log`, `*.cache`, `*.env`
   - **Rust**: `target/`, `debug/`, `release/`, `*.rs.bk`, `*.rlib`, `*.prof*`, `.idea/`, `*.log`, `.env*`
   - **Kotlin**: `build/`, `out/`, `.gradle/`, `.idea/`, `*.class`, `*.jar`, `*.iml`, `*.log`, `.env*`
   - **C++**: `build/`, `bin/`, `obj/`, `out/`, `*.o`, `*.so`, `*.a`, `*.exe`, `*.dll`, `.idea/`, `*.log`, `.env*`
   - **C**: `build/`, `bin/`, `obj/`, `out/`, `*.o`, `*.a`, `*.so`, `*.exe`, `Makefile`, `config.log`, `.idea/`, `*.log`, `.env*`
   - **Swift**: `.build/`, `DerivedData/`, `*.swiftpm/`, `Packages/`
   - **R**: `.Rproj.user/`, `.Rhistory`, `.RData`, `.Ruserdata`, `*.Rproj`, `packrat/`, `renv/`
   - **Universal**: `.DS_Store`, `Thumbs.db`, `*.tmp`, `*.swp`, `.vscode/`, `.idea/`

   **Padrões Específicos de Ferramenta**:
   - **Docker**: `node_modules/`, `.git/`, `Dockerfile*`, `.dockerignore`, `*.log*`, `.env*`, `coverage/`
   - **ESLint**: `node_modules/`, `dist/`, `build/`, `coverage/`, `*.min.js`
   - **Prettier**: `node_modules/`, `dist/`, `build/`, `coverage/`, `package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`
   - **Terraform**: `.terraform/`, `*.tfstate*`, `*.tfvars`, `.terraform.lock.hcl`
   - **Kubernetes/k8s**: `*.secret.yaml`, `secrets/`, `.kube/`, `kubeconfig*`, `*.key`, `*.crt`

5. Analise estrutura do tasks.md e extraia:
   - **Fases de tarefa**: Setup, Testes, Core, Integração, Polimento
   - **Dependências de tarefa**: Regras de execução sequencial vs paralela
   - **Detalhes de tarefa**: ID, descrição, caminhos de arquivo, marcadores de paralelo [P]
   - **Fluxo de execução**: Ordem e requisitos de dependência

6. Execute implementação seguindo o plano de tarefas:
   - **Execução fase-por-fase**: Complete cada fase antes de mover para a próxima
   - **Respeite dependências**: Execute tarefas sequenciais em ordem, tarefas paralelas [P] podem executar juntas
   - **Siga abordagem TDD**: Execute tarefas de teste antes de suas tarefas de implementação correspondentes
   - **Coordenação baseada em arquivo**: Tarefas afetando os mesmos arquivos devem executar sequencialmente
   - **Checkpoints de validação**: Verifique conclusão de cada fase antes de prosseguir

7. Regras de execução de implementação:
   - **Setup primeiro**: Inicialize estrutura do projeto, dependências, configuração
   - **Testes antes de código**: Se você precisar escrever testes para contratos, entidades, e cenários de integração
   - **Desenvolvimento core**: Implemente modelos, serviços, comandos CLI, endpoints
   - **Trabalho de integração**: Conexões de banco de dados, middleware, logging, serviços externos
   - **Polimento e validação**: Testes unitários, otimização de performance, documentação

8. Rastreamento de progresso e tratamento de erros:
   - Reporte progresso após cada tarefa completada
   - Interrompa execução se qualquer tarefa não-paralela falhar
   - Para tarefas paralelas [P], continue com tarefas bem-sucedidas, reporte as que falharam
   - Forneça mensagens de erro claras com contexto para debugging
   - Sugira próximos passos se implementação não puder prosseguir
   - **IMPORTANTE** Para tarefas completadas, certifique-se de marcar a tarefa como [X] no arquivo tasks.

9. Validação de conclusão:
   - Verifique que todas as tarefas requeridas estão completadas
   - Cheque que funcionalidades implementadas correspondem à especificação original
   - Valide que testes passam e cobertura atende requisitos
   - Confirme que a implementação segue o plano técnico
   - Reporte status final com resumo do trabalho completado

Nota: Este comando assume que uma decomposição completa de tarefas existe em tasks.md. Se tarefas estão incompletas ou faltando, sugira executar `/speckit.tasks` primeiro para regenerar a lista de tarefas.
