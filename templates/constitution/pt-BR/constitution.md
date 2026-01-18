# Constituição [PROJECT_NAME]

## Princípios Fundamentais

### I. [PRINCIPLE_1_NAME]

[PRINCIPLE_1_DESCRIPTION]

**Regras não negociáveis:**

- [PRINCIPLE_1_RULE_1]
- [PRINCIPLE_1_RULE_2]
- [PRINCIPLE_1_RULE_3]

**Justificativa:** [PRINCIPLE_1_RATIONALE]

### II. [PRINCIPLE_2_NAME]

[PRINCIPLE_2_DESCRIPTION]

**Regras não negociáveis:**

- [PRINCIPLE_2_RULE_1]
- [PRINCIPLE_2_RULE_2]
- [PRINCIPLE_2_RULE_3]

**Justificativa:** [PRINCIPLE_2_RATIONALE]

### III. [PRINCIPLE_3_NAME]

[PRINCIPLE_3_DESCRIPTION]

**Regras não negociáveis:**

- [PRINCIPLE_3_RULE_1]
- [PRINCIPLE_3_RULE_2]
- [PRINCIPLE_3_RULE_3]

**Justificativa:** [PRINCIPLE_3_RATIONALE]

<!--
  Adicione mais princípios conforme necessário seguindo o mesmo padrão:
  ### IV. [PRINCIPLE_N_NAME]
  [PRINCIPLE_N_DESCRIPTION]
  **Regras não negociáveis:**
  - [RULE_1]
  **Justificativa:** [RATIONALE]
-->

## Fluxo de Desenvolvimento

### Fases do Fluxo de Trabalho

O desenvolvimento DEVE seguir estas fases ordenadas:

1. **Constituição** (`/speckit.constitution`): Estabelecer ou revisar princípios do projeto
2. **Especificação** (`/speckit.specify`): Definir O QUE construir e POR QUÊ
3. **Esclarecimento** (`/speckit.clarify`): Resolver ambiguidades antes do planejamento
4. **Planejamento** (`/speckit.plan`): Definir COMO construir com escolhas tecnológicas
5. **Geração de Tarefas** (`/speckit.tasks`): Criar tarefas acionáveis e ordenadas por dependência
6. **Análise** (`/speckit.analyze`): Validar consistência entre artefatos
7. **Implementação** (`/speckit.implement`): Executar tarefas de acordo com o plano

### Hierarquia de Artefatos

```text
.specify/
├── memory/
│   └── constitution.md          # Este arquivo - princípios governantes
├── specs/
│   └── [###-nome-funcionalidade]/
│       ├── spec.md              # Especificação da funcionalidade (Fase 2)
│       ├── plan.md              # Plano de implementação (Fase 4)
│       ├── research.md          # Pesquisa técnica (Fase 4)
│       ├── data-model.md        # Design do modelo de dados (Fase 4)
│       ├── quickstart.md        # Cenários de validação (Fase 4)
│       ├── contracts/           # Contratos de API (Fase 4)
│       └── tasks.md             # Tarefas executáveis (Fase 5)
└── templates/                   # Templates para todos os artefatos
```

### Estratégia de Branches

- Cada funcionalidade DEVE ser desenvolvida em sua própria branch
- Nomenclatura de branch DEVE seguir o padrão: `[###-nome-funcionalidade]` (ex., `001-albuns-fotos`)
- Números de funcionalidades DEVEM ser sequenciais e auto-incrementados

## Padrões de Qualidade

### Qualidade da Especificação

- [ ] Nenhum marcador `[NECESSITA ESCLARECIMENTO]` permanece sem resolução
- [ ] Todos os requisitos são testáveis e não ambíguos
- [ ] Critérios de sucesso são mensuráveis
- [ ] Histórias de usuário são priorizadas e entregáveis independentemente
- [ ] Casos de borda estão documentados

### Qualidade do Plano

- [ ] Verificação da Constituição passa em todos os gates
- [ ] Contexto técnico está completo (linguagem, dependências, armazenamento, testes, plataforma)
- [ ] Estrutura do projeto está documentada com caminhos concretos
- [ ] Complexidade está justificada na tabela de rastreamento (se gates violados)

### Qualidade das Tarefas

- [ ] Tarefas são organizadas por história de usuário para entrega independente
- [ ] Dependências são explícitas e respeitadas
- [ ] Tarefas paralelas são marcadas com `[P]`
- [ ] Cada fase tem checkpoints claros para validação

## Governança

### Autoridade

Esta constituição substitui todas as outras práticas de desenvolvimento dentro de projetos [PROJECT_NAME]. Quando conflitos surgirem, princípios da constituição têm precedência.

### Conformidade

- Todos os pull requests DEVEM verificar conformidade com princípios da constituição
- Complexidade DEVE ser justificada; padrão é simplicidade
- Violações de gate DEVEM ser documentadas com justificativa

### Processo de Emenda

Modificações nesta constituição requerem:

1. Documentação explícita da justificativa para a mudança
2. Incremento de versão seguindo versionamento semântico:
   - MAJOR: Mudanças de governança/princípios incompatíveis com versões anteriores
   - MINOR: Novo princípio/seção adicionado ou orientação materialmente expandida
   - PATCH: Esclarecimentos, redação, refinamentos não semânticos
3. Atualização de `LAST_AMENDED_DATE` para refletir a data da mudança
4. Verificação de propagação em templates dependentes

### Orientação em Tempo de Execução

Para orientação detalhada de desenvolvimento específica para agentes de IA, consulte os arquivos de configuração do agente (ex., `CLAUDE.md`, `.cursorrules`) que fornecem instruções operacionais alinhadas com esta constituição.

**Versão**: [CONSTITUTION_VERSION] | **Ratificado**: [RATIFICATION_DATE] | **Última Emenda**: [LAST_AMENDED_DATE]
