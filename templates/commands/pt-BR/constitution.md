---
description: Criar ou atualizar a constituição do projeto a partir de entradas de princípios interativas ou fornecidas, garantindo que todos os templates dependentes permaneçam sincronizados.
handoffs:
  - label: Criar Especificação
    agent: speckit.specify
    prompt: Implemente a especificação da funcionalidade baseada na constituição atualizada. Quero construir...
---

## Diretiva de Idioma

**IMPORTANTE**: Gere TODO o conteúdo em Português do Brasil. Isso inclui:
- Nomes e descrições de princípios
- Regras não negociáveis
- Justificativas
- Seções de governança
- Padrões de qualidade
- Relatório de impacto de sincronização

Mantenha em inglês apenas: placeholders como `[PROJECT_NAME]`, `[PRINCIPLE_1_NAME]`, versões semânticas, e termos técnicos universais quando apropriado.

## Entrada do Usuário

```text
$ARGUMENTS
```

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Roteiro

Você está atualizando a constituição do projeto em `/memory/constitution.md`. Este arquivo é um TEMPLATE contendo tokens de placeholder em colchetes (ex. `[PROJECT_NAME]`, `[PRINCIPLE_1_NAME]`). Seu trabalho é (a) coletar/derivar valores concretos, (b) preencher o template precisamente, e (c) propagar quaisquer emendas através de artefatos dependentes.

Siga este fluxo de execução:

1. Carregue o template de constituição existente em `/memory/constitution.md`.
   - Identifique cada token placeholder da forma `[ALL_CAPS_IDENTIFIER]`.
   **IMPORTANTE**: O usuário pode requerer menos ou mais princípios do que os usados no template. Se um número for especificado, respeite isso - siga o template geral. Você atualizará o doc de acordo.

2. Colete/derive valores para placeholders:
   - Se entrada do usuário (conversa) fornece um valor, use-o.
   - Caso contrário infira do contexto do repositório existente (README, docs, versões anteriores da constituição se embutidas).
   - Para datas de governança: `RATIFICATION_DATE` é a data de adoção original (se desconhecida pergunte ou marque TODO), `LAST_AMENDED_DATE` é hoje se mudanças forem feitas, caso contrário mantenha anterior.
   - `CONSTITUTION_VERSION` deve incrementar de acordo com regras de versionamento semântico:
     - MAJOR: Remoções ou redefinições de governança/princípios incompatíveis com versões anteriores.
     - MINOR: Novo princípio/seção adicionado ou orientação materialmente expandida.
     - PATCH: Esclarecimentos, redação, correções de typo, refinamentos não semânticos.
   - Se tipo de bump de versão ambíguo, proponha raciocínio antes de finalizar.

3. Rascunhe o conteúdo da constituição atualizada:
   - Substitua cada placeholder com texto concreto (nenhum token entre colchetes restante exceto slots de template intencionalmente retidos que o projeto escolheu não definir ainda—justifique explicitamente qualquer um deixado).
   - Preserve hierarquia de cabeçalhos e comentários podem ser removidos uma vez substituídos a menos que ainda adicionem orientação esclarecedora.
   - Garanta que cada seção de Princípio: linha de nome sucinta, parágrafo (ou lista de bullets) capturando regras não-negociáveis, justificativa explícita se não óbvia.
   - Garanta que seção de Governança lista procedimento de emenda, política de versionamento, e expectativas de revisão de conformidade.

4. Checklist de propagação de consistência (converta checklist anterior em validações ativas):
   - Leia `/templates/plan-template.md` e garanta que qualquer "Verificação da Constituição" ou regras alinham com princípios atualizados.
   - Leia `/templates/spec-template.md` para alinhamento de escopo/requisitos—atualize se constituição adiciona/remove seções ou restrições obrigatórias.
   - Leia `/templates/tasks-template.md` e garanta que categorização de tarefas reflete novos ou removidos tipos de tarefa orientados por princípios (ex., observabilidade, versionamento, disciplina de testes).
   - Leia cada arquivo de comando em `/templates/commands/*.md` (incluindo este) para verificar que nenhuma referência desatualizada (nomes específicos de agente como CLAUDE apenas) permanece quando orientação genérica é requerida.
   - Leia quaisquer docs de orientação de runtime (ex., `README.md`, `docs/quickstart.md`, ou arquivos de orientação específicos de agente se presentes). Atualize referências a princípios alterados.

5. Produza um Relatório de Impacto de Sincronização (adicione como comentário HTML no topo do arquivo de constituição após atualização):
   - Mudança de versão: antiga → nova
   - Lista de princípios modificados (título antigo → título novo se renomeado)
   - Seções adicionadas
   - Seções removidas
   - Templates requerendo atualizações (✅ atualizado / ⚠ pendente) com caminhos de arquivo
   - TODOs de acompanhamento se quaisquer placeholders intencionalmente diferidos.

6. Validação antes da saída final:
   - Nenhum token entre colchetes não explicado restante.
   - Linha de versão corresponde ao relatório.
   - Datas em formato ISO YYYY-MM-DD.
   - Princípios são declarativos, testáveis, e livres de linguagem vaga ("should" → substitua com justificativa MUST/SHOULD onde apropriado).

7. Escreva a constituição completada de volta para `/memory/constitution.md` (sobrescreva).

8. Saída de resumo final para o usuário com:
   - Nova versão e justificativa do bump.
   - Quaisquer arquivos sinalizados para acompanhamento manual.
   - Mensagem de commit sugerida (ex., `docs: emenda constituição para vX.Y.Z (adições de princípios + atualização de governança)`).

Requisitos de Formatação & Estilo:

- Use cabeçalhos Markdown exatamente como no template (não rebaixe/promova níveis).
- Quebre linhas longas de justificativa para manter legibilidade (<100 chars idealmente) mas não force com quebras estranhas.
- Mantenha uma única linha em branco entre seções.
- Evite espaços em branco no final.

Se o usuário fornece atualizações parciais (ex., apenas uma revisão de princípio), ainda realize passos de validação e decisão de versão.

Se informação crítica faltando (ex., data de ratificação verdadeiramente desconhecida), insira `TODO(<FIELD_NAME>): explicação` e inclua no Relatório de Impacto de Sincronização sob itens diferidos.

Não crie um novo template; sempre opere no arquivo `/memory/constitution.md` existente.
