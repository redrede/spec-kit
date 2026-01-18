---

description: "Template de lista de tarefas para implementação de funcionalidade"
---

# Tarefas: [FEATURE NAME]

**Entrada**: Documentos de design de `/specs/[###-feature-name]/`
**Pré-requisitos**: plan.md (obrigatório), spec.md (obrigatório para histórias de usuário), research.md, data-model.md, contracts/

**Testes**: Os exemplos abaixo incluem tarefas de teste. Testes são OPCIONAIS - inclua apenas se explicitamente solicitado na especificação da funcionalidade.

**Organização**: As tarefas são agrupadas por história de usuário para permitir implementação e testes independentes de cada história.

## Formato: `[ID] [P?] [Story] Descrição`

- **[P]**: Pode executar em paralelo (arquivos diferentes, sem dependências)
- **[Story]**: A qual história de usuário esta tarefa pertence (ex., US1, US2, US3)
- Incluir caminhos exatos dos arquivos nas descrições

## Convenções de Caminho

- **Projeto único**: `src/`, `tests/` na raiz do repositório
- **App web**: `backend/src/`, `frontend/src/`
- **Mobile**: `api/src/`, `ios/src/` ou `android/src/`
- Os caminhos mostrados abaixo assumem projeto único - ajuste baseado na estrutura do plan.md

<!--
  ============================================================================
  IMPORTANTE: As tarefas abaixo são TAREFAS DE EXEMPLO apenas para ilustração.

  O comando /speckit.tasks DEVE substituir estas com tarefas reais baseadas em:
  - Histórias de usuário do spec.md (com suas prioridades P1, P2, P3...)
  - Requisitos da funcionalidade do plan.md
  - Entidades do data-model.md
  - Endpoints dos contracts/

  As tarefas DEVEM ser organizadas por história de usuário para que cada história possa ser:
  - Implementada independentemente
  - Testada independentemente
  - Entregue como um incremento MVP

  NÃO mantenha estas tarefas de exemplo no arquivo tasks.md gerado.
  ============================================================================
-->

## Fase 1: Configuração (Infraestrutura Compartilhada)

**Propósito**: Inicialização do projeto e estrutura básica

- [ ] T001 Criar estrutura do projeto conforme plano de implementação
- [ ] T002 Inicializar projeto [linguagem] com dependências [framework]
- [ ] T003 [P] Configurar ferramentas de linting e formatação

---

## Fase 2: Fundação (Pré-requisitos Bloqueantes)

**Propósito**: Infraestrutura central que DEVE estar completa antes de QUALQUER história de usuário ser implementada

**⚠️ CRÍTICO**: Nenhum trabalho de história de usuário pode começar até esta fase estar completa

Exemplos de tarefas fundacionais (ajuste baseado no seu projeto):

- [ ] T004 Configurar schema de banco de dados e framework de migrações
- [ ] T005 [P] Implementar framework de autenticação/autorização
- [ ] T006 [P] Configurar estrutura de rotas e middleware da API
- [ ] T007 Criar modelos/entidades base que todas as histórias dependem
- [ ] T008 Configurar infraestrutura de tratamento de erros e logging
- [ ] T009 Configurar gerenciamento de configuração de ambiente

**Checkpoint**: Fundação pronta - implementação das histórias de usuário pode começar em paralelo

---

## Fase 3: História de Usuário 1 - [Título] (Prioridade: P1) 🎯 MVP

**Objetivo**: [Breve descrição do que esta história entrega]

**Teste Independente**: [Como verificar que esta história funciona por conta própria]

### Testes para História de Usuário 1 (OPCIONAL - apenas se testes solicitados) ⚠️

> **NOTA: Escreva estes testes PRIMEIRO, garanta que FALHEM antes da implementação**

- [ ] T010 [P] [US1] Teste de contrato para [endpoint] em tests/contract/test_[name].py
- [ ] T011 [P] [US1] Teste de integração para [jornada do usuário] em tests/integration/test_[name].py

### Implementação para História de Usuário 1

- [ ] T012 [P] [US1] Criar modelo [Entidade1] em src/models/[entidade1].py
- [ ] T013 [P] [US1] Criar modelo [Entidade2] em src/models/[entidade2].py
- [ ] T014 [US1] Implementar [Serviço] em src/services/[servico].py (depende de T012, T013)
- [ ] T015 [US1] Implementar [endpoint/funcionalidade] em src/[local]/[arquivo].py
- [ ] T016 [US1] Adicionar validação e tratamento de erros
- [ ] T017 [US1] Adicionar logging para operações da história de usuário 1

**Checkpoint**: Neste ponto, História de Usuário 1 deve estar totalmente funcional e testável independentemente

---

## Fase 4: História de Usuário 2 - [Título] (Prioridade: P2)

**Objetivo**: [Breve descrição do que esta história entrega]

**Teste Independente**: [Como verificar que esta história funciona por conta própria]

### Testes para História de Usuário 2 (OPCIONAL - apenas se testes solicitados) ⚠️

- [ ] T018 [P] [US2] Teste de contrato para [endpoint] em tests/contract/test_[name].py
- [ ] T019 [P] [US2] Teste de integração para [jornada do usuário] em tests/integration/test_[name].py

### Implementação para História de Usuário 2

- [ ] T020 [P] [US2] Criar modelo [Entidade] em src/models/[entidade].py
- [ ] T021 [US2] Implementar [Serviço] em src/services/[servico].py
- [ ] T022 [US2] Implementar [endpoint/funcionalidade] em src/[local]/[arquivo].py
- [ ] T023 [US2] Integrar com componentes da História de Usuário 1 (se necessário)

**Checkpoint**: Neste ponto, Histórias de Usuário 1 E 2 devem funcionar independentemente

---

## Fase 5: História de Usuário 3 - [Título] (Prioridade: P3)

**Objetivo**: [Breve descrição do que esta história entrega]

**Teste Independente**: [Como verificar que esta história funciona por conta própria]

### Testes para História de Usuário 3 (OPCIONAL - apenas se testes solicitados) ⚠️

- [ ] T024 [P] [US3] Teste de contrato para [endpoint] em tests/contract/test_[name].py
- [ ] T025 [P] [US3] Teste de integração para [jornada do usuário] em tests/integration/test_[name].py

### Implementação para História de Usuário 3

- [ ] T026 [P] [US3] Criar modelo [Entidade] em src/models/[entidade].py
- [ ] T027 [US3] Implementar [Serviço] em src/services/[servico].py
- [ ] T028 [US3] Implementar [endpoint/funcionalidade] em src/[local]/[arquivo].py

**Checkpoint**: Todas as histórias de usuário devem estar funcionais independentemente agora

---

[Adicione mais fases de história de usuário conforme necessário, seguindo o mesmo padrão]

---

## Fase N: Polimento & Preocupações Transversais

**Propósito**: Melhorias que afetam múltiplas histórias de usuário

- [ ] TXXX [P] Atualizações de documentação em docs/
- [ ] TXXX Limpeza e refatoração de código
- [ ] TXXX Otimização de performance em todas as histórias
- [ ] TXXX [P] Testes unitários adicionais (se solicitados) em tests/unit/
- [ ] TXXX Fortalecimento de segurança
- [ ] TXXX Executar validação do quickstart.md

---

## Dependências & Ordem de Execução

### Dependências de Fase

- **Configuração (Fase 1)**: Sem dependências - pode começar imediatamente
- **Fundação (Fase 2)**: Depende da conclusão da Configuração - BLOQUEIA todas as histórias de usuário
- **Histórias de Usuário (Fase 3+)**: Todas dependem da conclusão da fase Fundação
  - Histórias de usuário podem prosseguir em paralelo (se houver equipe)
  - Ou sequencialmente em ordem de prioridade (P1 → P2 → P3)
- **Polimento (Fase Final)**: Depende de todas as histórias de usuário desejadas estarem completas

### Dependências de História de Usuário

- **História de Usuário 1 (P1)**: Pode começar após Fundação (Fase 2) - Sem dependências de outras histórias
- **História de Usuário 2 (P2)**: Pode começar após Fundação (Fase 2) - Pode integrar com US1 mas deve ser testável independentemente
- **História de Usuário 3 (P3)**: Pode começar após Fundação (Fase 2) - Pode integrar com US1/US2 mas deve ser testável independentemente

### Dentro de Cada História de Usuário

- Testes (se incluídos) DEVEM ser escritos e FALHAR antes da implementação
- Modelos antes de serviços
- Serviços antes de endpoints
- Implementação central antes de integração
- História completa antes de passar para próxima prioridade

### Oportunidades de Paralelismo

- Todas as tarefas de Configuração marcadas [P] podem executar em paralelo
- Todas as tarefas de Fundação marcadas [P] podem executar em paralelo (dentro da Fase 2)
- Uma vez que a fase Fundação complete, todas as histórias de usuário podem começar em paralelo (se capacidade da equipe permitir)
- Todos os testes para uma história de usuário marcados [P] podem executar em paralelo
- Modelos dentro de uma história marcados [P] podem executar em paralelo
- Diferentes histórias de usuário podem ser trabalhadas em paralelo por diferentes membros da equipe

---

## Exemplo de Paralelismo: História de Usuário 1

```bash
# Lançar todos os testes para História de Usuário 1 juntos (se testes solicitados):
Task: "Teste de contrato para [endpoint] em tests/contract/test_[name].py"
Task: "Teste de integração para [jornada do usuário] em tests/integration/test_[name].py"

# Lançar todos os modelos para História de Usuário 1 juntos:
Task: "Criar modelo [Entidade1] em src/models/[entidade1].py"
Task: "Criar modelo [Entidade2] em src/models/[entidade2].py"
```

---

## Estratégia de Implementação

### MVP Primeiro (Apenas História de Usuário 1)

1. Completar Fase 1: Configuração
2. Completar Fase 2: Fundação (CRÍTICO - bloqueia todas as histórias)
3. Completar Fase 3: História de Usuário 1
4. **PARAR e VALIDAR**: Testar História de Usuário 1 independentemente
5. Implantar/demonstrar se pronto

### Entrega Incremental

1. Completar Configuração + Fundação → Fundação pronta
2. Adicionar História de Usuário 1 → Testar independentemente → Implantar/Demo (MVP!)
3. Adicionar História de Usuário 2 → Testar independentemente → Implantar/Demo
4. Adicionar História de Usuário 3 → Testar independentemente → Implantar/Demo
5. Cada história adiciona valor sem quebrar histórias anteriores

### Estratégia de Equipe em Paralelo

Com múltiplos desenvolvedores:

1. Equipe completa Configuração + Fundação juntos
2. Uma vez que Fundação está feita:
   - Desenvolvedor A: História de Usuário 1
   - Desenvolvedor B: História de Usuário 2
   - Desenvolvedor C: História de Usuário 3
3. Histórias completam e integram independentemente

---

## Notas

- Tarefas [P] = arquivos diferentes, sem dependências
- Rótulo [Story] mapeia tarefa para história de usuário específica para rastreabilidade
- Cada história de usuário deve ser completável e testável independentemente
- Verificar que testes falham antes de implementar
- Commit após cada tarefa ou grupo lógico
- Parar em qualquer checkpoint para validar história independentemente
- Evitar: tarefas vagas, conflitos de mesmo arquivo, dependências entre histórias que quebram independência
