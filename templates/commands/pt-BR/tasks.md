---
description: Gerar um tasks.md acionável e ordenado por dependências para a funcionalidade baseado nos artefatos de design disponíveis.
handoffs:
  - label: Analisar Consistência
    agent: speckit.analyze
    prompt: Execute uma análise de consistência do projeto
    send: true
  - label: Implementar Projeto
    agent: speckit.implement
    prompt: Inicie a implementação em fases
    send: true
scripts:
  sh: scripts/bash/check-prerequisites.sh --json
  ps: scripts/powershell/check-prerequisites.ps1 -Json
---

## Diretiva de Idioma

**IMPORTANTE**: Gere TODO o conteúdo em Português do Brasil. Isso inclui:
- Descrições de tarefas
- Nomes de fases
- Notas e comentários
- Instruções de execução
- Checkpoints e validações

Mantenha em inglês apenas: IDs de tarefas (T001, T002), marcadores [P], [US1], nomes de arquivos, e termos técnicos universais quando apropriado.

## Entrada do Usuário

```text
$ARGUMENTS
```

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Roteiro

1. **Setup**: Execute `{SCRIPT}` da raiz do repositório e analise FEATURE_DIR e lista AVAILABLE_DOCS. Todos os caminhos devem ser absolutos. Para aspas simples em args como "I'm Groot", use sintaxe de escape: ex 'I'\''m Groot' (ou aspas duplas se possível: "I'm Groot").

2. **Carregar documentos de design**: Leia de FEATURE_DIR:
   - **Obrigatório**: plan.md (stack tecnológica, bibliotecas, estrutura), spec.md (histórias de usuário com prioridades)
   - **Opcional**: data-model.md (entidades), contracts/ (endpoints de API), research.md (decisões), quickstart.md (cenários de teste)
   - Nota: Nem todos os projetos têm todos os documentos. Gere tarefas baseadas no que está disponível.

3. **Executar fluxo de geração de tarefas**:
   - Carregue plan.md e extraia stack tecnológica, bibliotecas, estrutura do projeto
   - Carregue spec.md e extraia histórias de usuário com suas prioridades (P1, P2, P3, etc.)
   - Se data-model.md existir: Extraia entidades e mapeie para histórias de usuário
   - Se contracts/ existir: Mapeie endpoints para histórias de usuário
   - Se research.md existir: Extraia decisões para tarefas de setup
   - Gere tarefas organizadas por história de usuário (veja Regras de Geração de Tarefas abaixo)
   - Gere grafo de dependências mostrando ordem de conclusão de histórias de usuário
   - Crie exemplos de execução paralela por história de usuário
   - Valide completude das tarefas (cada história de usuário tem todas as tarefas necessárias, testável independentemente)

4. **Gerar tasks.md**: Use `templates/pt-BR/tasks-template.md` como estrutura, preencha com:
   - Nome correto da funcionalidade do plan.md
   - Fase 1: Tarefas de setup (inicialização do projeto)
   - Fase 2: Tarefas fundamentais (pré-requisitos bloqueantes para todas as histórias de usuário)
   - Fase 3+: Uma fase por história de usuário (em ordem de prioridade do spec.md)
   - Cada fase inclui: objetivo da história, critérios de teste independente, testes (se solicitados), tarefas de implementação
   - Fase Final: Polimento & preocupações transversais
   - Todas as tarefas devem seguir o formato estrito de checklist (veja Regras de Geração de Tarefas abaixo)
   - Caminhos de arquivo claros para cada tarefa
   - Seção de dependências mostrando ordem de conclusão de histórias
   - Exemplos de execução paralela por história
   - Seção de estratégia de implementação (MVP primeiro, entrega incremental)

5. **Reportar**: Saída do caminho para tasks.md gerado e resumo:
   - Contagem total de tarefas
   - Contagem de tarefas por história de usuário
   - Oportunidades de paralelismo identificadas
   - Critérios de teste independente para cada história
   - Escopo sugerido do MVP (tipicamente apenas História de Usuário 1)
   - Validação de formato: Confirme que TODAS as tarefas seguem o formato de checklist (checkbox, ID, labels, caminhos de arquivo)

Contexto para geração de tarefas: {ARGS}

O tasks.md deve ser imediatamente executável - cada tarefa deve ser específica o suficiente para que um LLM possa completá-la sem contexto adicional.

## Regras de Geração de Tarefas

**CRÍTICO**: Tarefas DEVEM ser organizadas por história de usuário para permitir implementação e teste independentes.

**Testes são OPCIONAIS**: Apenas gere tarefas de teste se explicitamente solicitado na especificação da funcionalidade ou se o usuário solicitar abordagem TDD.

### Formato de Checklist (OBRIGATÓRIO)

Toda tarefa DEVE seguir estritamente este formato:

```text
- [ ] [TaskID] [P?] [Story?] Descrição com caminho do arquivo
```

**Componentes do Formato**:

1. **Checkbox**: SEMPRE comece com `- [ ]` (checkbox markdown)
2. **Task ID**: Número sequencial (T001, T002, T003...) em ordem de execução
3. **Marcador [P]**: Inclua APENAS se a tarefa é paralelizável (arquivos diferentes, sem dependências em tarefas incompletas)
4. **Label [Story]**: OBRIGATÓRIO apenas para tarefas de fases de história de usuário
   - Formato: [US1], [US2], [US3], etc. (mapeia para histórias de usuário do spec.md)
   - Fase de Setup: SEM label de história
   - Fase Fundamental: SEM label de história
   - Fases de História de Usuário: DEVE ter label de história
   - Fase de Polimento: SEM label de história
5. **Descrição**: Ação clara com caminho exato do arquivo

**Exemplos**:

- ✅ CORRETO: `- [ ] T001 Criar estrutura do projeto conforme plano de implementação`
- ✅ CORRETO: `- [ ] T005 [P] Implementar middleware de autenticação em src/middleware/auth.py`
- ✅ CORRETO: `- [ ] T012 [P] [US1] Criar modelo User em src/models/user.py`
- ✅ CORRETO: `- [ ] T014 [US1] Implementar UserService em src/services/user_service.py`
- ❌ ERRADO: `- [ ] Criar modelo User` (falta ID e label de Story)
- ❌ ERRADO: `T001 [US1] Criar modelo` (falta checkbox)
- ❌ ERRADO: `- [ ] [US1] Criar modelo User` (falta Task ID)
- ❌ ERRADO: `- [ ] T001 [US1] Criar modelo` (falta caminho do arquivo)

### Organização de Tarefas

1. **Das Histórias de Usuário (spec.md)** - ORGANIZAÇÃO PRIMÁRIA:
   - Cada história de usuário (P1, P2, P3...) recebe sua própria fase
   - Mapeie todos os componentes relacionados para sua história:
     - Modelos necessários para aquela história
     - Serviços necessários para aquela história
     - Endpoints/UI necessários para aquela história
     - Se testes solicitados: Testes específicos para aquela história
   - Marque dependências de história (maioria das histórias deve ser independente)

2. **Dos Contratos**:
   - Mapeie cada contrato/endpoint → para a história de usuário que ele serve
   - Se testes solicitados: Cada contrato → tarefa de teste de contrato [P] antes da implementação na fase daquela história

3. **Do Modelo de Dados**:
   - Mapeie cada entidade para a(s) história(s) de usuário que precisam dela
   - Se entidade serve múltiplas histórias: Coloque na história mais antiga ou fase de Setup
   - Relacionamentos → tarefas de camada de serviço na fase de história apropriada

4. **De Setup/Infraestrutura**:
   - Infraestrutura compartilhada → fase de Setup (Fase 1)
   - Tarefas fundamentais/bloqueantes → fase Fundamental (Fase 2)
   - Setup específico de história → dentro da fase daquela história

### Estrutura de Fases

- **Fase 1**: Setup (inicialização do projeto)
- **Fase 2**: Fundamental (pré-requisitos bloqueantes - DEVE completar antes das histórias de usuário)
- **Fase 3+**: Histórias de Usuário em ordem de prioridade (P1, P2, P3...)
  - Dentro de cada história: Testes (se solicitados) → Modelos → Serviços → Endpoints → Integração
  - Cada fase deve ser um incremento completo e testável independentemente
- **Fase Final**: Polimento & Preocupações Transversais
