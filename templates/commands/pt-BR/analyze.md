---
description: Realizar análise não-destrutiva de consistência e qualidade entre artefatos através de spec.md, plan.md, e tasks.md após geração de tarefas.
scripts:
  sh: scripts/bash/check-prerequisites.sh --json --require-tasks --include-tasks
  ps: scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks
---

## Diretiva de Idioma

**IMPORTANTE**: Gere TODO o conteúdo em Português do Brasil. Isso inclui:
- Relatório de análise
- Descobertas e recomendações
- Tabelas de cobertura
- Próximas ações
- Todas as comunicações com o usuário

Mantenha em inglês apenas: IDs de descobertas (A1, D1), caminhos de arquivo, identificadores técnicos, e termos técnicos universais quando apropriado.

## Entrada do Usuário

```text
$ARGUMENTS
```

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Objetivo

Identificar inconsistências, duplicações, ambiguidades, e itens subespecificados através dos três artefatos principais (`spec.md`, `plan.md`, `tasks.md`) antes da implementação. Este comando DEVE executar apenas após `/speckit.tasks` ter produzido com sucesso um `tasks.md` completo.

## Restrições de Operação

**ESTRITAMENTE SOMENTE-LEITURA**: **NÃO** modifique nenhum arquivo. Produza um relatório de análise estruturado. Ofereça um plano de remediação opcional (usuário deve aprovar explicitamente antes que quaisquer comandos de edição de acompanhamento sejam invocados manualmente).

**Autoridade da Constituição**: A constituição do projeto (`/memory/constitution.md`) é **não-negociável** dentro deste escopo de análise. Conflitos com a constituição são automaticamente CRÍTICOS e requerem ajuste da spec, plano, ou tarefas—não diluição, reinterpretação, ou ignorar silencioso do princípio. Se um princípio precisa mudar, isso deve ocorrer em uma atualização de constituição separada e explícita fora de `/speckit.analyze`.

## Passos de Execução

### 1. Inicializar Contexto de Análise

Execute `{SCRIPT}` uma vez da raiz do repositório e analise JSON para FEATURE_DIR e AVAILABLE_DOCS. Derive caminhos absolutos:

- SPEC = FEATURE_DIR/spec.md
- PLAN = FEATURE_DIR/plan.md
- TASKS = FEATURE_DIR/tasks.md

Aborte com mensagem de erro se qualquer arquivo requerido estiver faltando (instrua usuário a executar comando de pré-requisito faltante).
Para aspas simples em args como "I'm Groot", use sintaxe de escape: ex 'I'\''m Groot' (ou aspas duplas se possível: "I'm Groot").

### 2. Carregar Artefatos (Divulgação Progressiva)

Carregue apenas o contexto mínimo necessário de cada artefato:

**De spec.md:**

- Visão Geral/Contexto
- Requisitos Funcionais
- Requisitos Não-Funcionais
- Histórias de Usuário
- Casos de Borda (se presente)

**De plan.md:**

- Escolhas de Arquitetura/stack
- Referências de Modelo de Dados
- Fases
- Restrições técnicas

**De tasks.md:**

- IDs de Tarefas
- Descrições
- Agrupamento de Fases
- Marcadores de paralelo [P]
- Caminhos de arquivo referenciados

**Da constituição:**

- Carregue `/memory/constitution.md` para validação de princípios

### 3. Construir Modelos Semânticos

Crie representações internas (não inclua artefatos brutos na saída):

- **Inventário de requisitos**: Cada requisito funcional + não-funcional com uma chave estável (derive slug baseado em frase imperativa; ex., "Usuário pode fazer upload de arquivo" → `usuario-pode-fazer-upload-arquivo`)
- **Inventário de história/ação de usuário**: Ações discretas de usuário com critérios de aceitação
- **Mapeamento de cobertura de tarefas**: Mapeie cada tarefa para um ou mais requisitos ou histórias (inferência por palavra-chave / padrões de referência explícita como IDs ou frases-chave)
- **Conjunto de regras da constituição**: Extraia nomes de princípios e declarações normativas MUST/SHOULD

### 4. Passes de Detecção (Análise Eficiente em Tokens)

Foque em descobertas de alto sinal. Limite a 50 descobertas no total; agregue resto em resumo de overflow.

#### A. Detecção de Duplicação

- Identifique requisitos quase-duplicados
- Marque fraseado de menor qualidade para consolidação

#### B. Detecção de Ambiguidade

- Sinalize adjetivos vagos (rápido, escalável, seguro, intuitivo, robusto) sem critérios mensuráveis
- Sinalize placeholders não resolvidos (TODO, TKTK, ???, `<placeholder>`, etc.)

#### C. Subespecificação

- Requisitos com verbos mas sem objeto ou resultado mensurável
- Histórias de usuário sem alinhamento de critérios de aceitação
- Tarefas referenciando arquivos ou componentes não definidos em spec/plano

#### D. Alinhamento com Constituição

- Qualquer requisito ou elemento de plano conflitando com princípio MUST
- Seções obrigatórias ou gates de qualidade faltantes da constituição

#### E. Lacunas de Cobertura

- Requisitos com zero tarefas associadas
- Tarefas sem requisito/história mapeado
- Requisitos não-funcionais não refletidos em tarefas (ex., performance, segurança)

#### F. Inconsistência

- Deriva de terminologia (mesmo conceito nomeado diferentemente através de arquivos)
- Entidades de dados referenciadas no plano mas ausentes na spec (ou vice-versa)
- Contradições de ordenação de tarefas (ex., tarefas de integração antes de tarefas de setup fundamental sem nota de dependência)
- Requisitos conflitantes (ex., um requer Next.js enquanto outro especifica Vue)

### 5. Atribuição de Severidade

Use esta heurística para priorizar descobertas:

- **CRÍTICO**: Viola MUST da constituição, artefato de spec principal faltando, ou requisito com zero cobertura que bloqueia funcionalidade baseline
- **ALTO**: Requisito duplicado ou conflitante, atributo de segurança/performance ambíguo, critério de aceitação não testável
- **MÉDIO**: Deriva de terminologia, cobertura de tarefa não-funcional faltante, caso de borda subespecificado
- **BAIXO**: Melhorias de estilo/redação, redundância menor não afetando ordem de execução

### 6. Produzir Relatório de Análise Compacto

Saída de relatório Markdown (sem escritas de arquivo) com a seguinte estrutura:

## Relatório de Análise de Especificação

| ID | Categoria | Severidade | Localização(ões) | Resumo | Recomendação |
|----|-----------|------------|------------------|--------|--------------|
| A1 | Duplicação | ALTO | spec.md:L120-134 | Dois requisitos similares ... | Mescle fraseado; mantenha versão mais clara |

(Adicione uma linha por descoberta; gere IDs estáveis prefixados por inicial da categoria.)

**Tabela de Resumo de Cobertura:**

| Chave do Requisito | Tem Tarefa? | IDs de Tarefa | Notas |
|--------------------|-------------|---------------|-------|

**Problemas de Alinhamento com Constituição:** (se houver)

**Tarefas Não Mapeadas:** (se houver)

**Métricas:**

- Total de Requisitos
- Total de Tarefas
- Cobertura % (requisitos com >=1 tarefa)
- Contagem de Ambiguidades
- Contagem de Duplicações
- Contagem de Problemas Críticos

### 7. Fornecer Próximas Ações

No final do relatório, mostre bloco de Próximas Ações conciso:

- Se problemas CRÍTICOS existem: Recomende resolver antes de `/speckit.implement`
- Se apenas BAIXO/MÉDIO: Usuário pode prosseguir, mas forneça sugestões de melhoria
- Forneça sugestões de comando explícitas: ex., "Execute /speckit.specify com refinamento", "Execute /speckit.plan para ajustar arquitetura", "Edite manualmente tasks.md para adicionar cobertura para 'metricas-de-performance'"

### 8. Oferecer Remediação

Pergunte ao usuário: "Você gostaria que eu sugerisse edições concretas de remediação para os top N problemas?" (NÃO aplique-as automaticamente.)

## Princípios de Operação

### Eficiência de Contexto

- **Tokens mínimos de alto sinal**: Foque em descobertas acionáveis, não documentação exaustiva
- **Divulgação progressiva**: Carregue artefatos incrementalmente; não despeje todo conteúdo na análise
- **Saída eficiente em tokens**: Limite tabela de descobertas a 50 linhas; sumarize overflow
- **Resultados determinísticos**: Re-executar sem mudanças deve produzir IDs e contagens consistentes

### Diretrizes de Análise

- **NUNCA modifique arquivos** (esta é análise somente-leitura)
- **NUNCA alucine seções faltantes** (se ausentes, reporte-as precisamente)
- **Priorize violações de constituição** (estas são sempre CRÍTICAS)
- **Use exemplos ao invés de regras exaustivas** (cite instâncias específicas, não padrões genéricos)
- **Reporte zero problemas graciosamente** (emita relatório de sucesso com estatísticas de cobertura)

## Contexto

{ARGS}
