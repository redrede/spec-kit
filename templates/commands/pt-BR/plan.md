---
description: Executar o fluxo de planejamento de implementação usando o template de plano para gerar artefatos de design.
handoffs:
  - label: Criar Tarefas
    agent: speckit.tasks
    prompt: Divida o plano em tarefas
    send: true
  - label: Criar Checklist
    agent: speckit.checklist
    prompt: Crie um checklist para o seguinte domínio...
scripts:
  sh: scripts/bash/setup-plan.sh --json
  ps: scripts/powershell/setup-plan.ps1 -Json
agent_scripts:
  sh: scripts/bash/update-agent-context.sh __AGENT__
  ps: scripts/powershell/update-agent-context.ps1 -AgentType __AGENT__
---

## Diretiva de Idioma

**IMPORTANTE**: Gere TODO o conteúdo em Português do Brasil. Isso inclui:
- Resumo técnico e contexto
- Descrições de pesquisa e decisões
- Modelo de dados e documentação
- Contratos de API (descrições e comentários)
- Cenários de quickstart
- Todas as notas e comentários

Mantenha em inglês apenas: nomes de arquivos, nomes de variáveis em código, identificadores técnicos, e termos técnicos universais quando apropriado.

## Entrada do Usuário

```text
$ARGUMENTS
```

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Roteiro

1. **Setup**: Execute `{SCRIPT}` da raiz do repositório e analise o JSON para FEATURE_SPEC, IMPL_PLAN, SPECS_DIR, BRANCH. Para aspas simples em args como "I'm Groot", use sintaxe de escape: ex 'I'\''m Groot' (ou aspas duplas se possível: "I'm Groot").

2. **Carregar contexto**: Leia FEATURE_SPEC e `/memory/constitution.md`. Carregue template IMPL_PLAN (já copiado).

3. **Executar fluxo de planejamento**: Siga a estrutura no template IMPL_PLAN para:
   - Preencher Contexto Técnico (marque desconhecidos como "NECESSITA ESCLARECIMENTO")
   - Preencher seção de Verificação da Constituição a partir da constituição
   - Avaliar gates (ERRO se violações não justificadas)
   - Fase 0: Gerar research.md (resolver todos NECESSITA ESCLARECIMENTO)
   - Fase 1: Gerar data-model.md, contracts/, quickstart.md
   - Fase 1: Atualizar contexto do agente executando o script do agente
   - Re-avaliar Verificação da Constituição pós-design

4. **Parar e reportar**: Comando termina após planejamento da Fase 2. Reporte branch, caminho IMPL_PLAN, e artefatos gerados.

## Fases

### Fase 0: Esboço & Pesquisa

1. **Extrair desconhecidos do Contexto Técnico** acima:
   - Para cada NECESSITA ESCLARECIMENTO → tarefa de pesquisa
   - Para cada dependência → tarefa de melhores práticas
   - Para cada integração → tarefa de padrões

2. **Gerar e despachar agentes de pesquisa**:

   ```text
   Para cada desconhecido no Contexto Técnico:
     Tarefa: "Pesquisar {desconhecido} para {contexto da funcionalidade}"
   Para cada escolha de tecnologia:
     Tarefa: "Encontrar melhores práticas para {tech} em {domínio}"
   ```

3. **Consolidar descobertas** em `research.md` usando formato:
   - Decisão: [o que foi escolhido]
   - Justificativa: [por que escolhido]
   - Alternativas consideradas: [o que mais foi avaliado]

**Saída**: research.md com todos NECESSITA ESCLARECIMENTO resolvidos

### Fase 1: Design & Contratos

**Pré-requisitos:** `research.md` completo

1. **Extrair entidades da spec da funcionalidade** → `data-model.md`:
   - Nome da entidade, campos, relacionamentos
   - Regras de validação dos requisitos
   - Transições de estado se aplicável

2. **Gerar contratos de API** dos requisitos funcionais:
   - Para cada ação do usuário → endpoint
   - Use padrões REST/GraphQL padrão
   - Saída OpenAPI/GraphQL schema para `/contracts/`

3. **Atualização de contexto do agente**:
   - Execute `{AGENT_SCRIPT}`
   - Estes scripts detectam qual agente IA está em uso
   - Atualizam o arquivo de contexto específico do agente apropriado
   - Adicionam apenas nova tecnologia do plano atual
   - Preservam adições manuais entre marcadores

**Saída**: data-model.md, /contracts/*, quickstart.md, arquivo específico do agente

## Regras principais

- Use caminhos absolutos
- ERRO em falhas de gate ou esclarecimentos não resolvidos
