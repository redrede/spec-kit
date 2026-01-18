---
description: Gerar um checklist personalizado para a funcionalidade atual baseado nos requisitos do usuário.
scripts:
  sh: scripts/bash/check-prerequisites.sh --json
  ps: scripts/powershell/check-prerequisites.ps1 -Json
---

## Diretiva de Idioma

**IMPORTANTE**: Gere TODO o conteúdo em Português do Brasil. Isso inclui:
- Itens do checklist
- Categorias e descrições
- Perguntas de esclarecimento
- Relatório de conclusão
- Todas as comunicações com o usuário

Mantenha em inglês apenas: IDs de checklist (CHK001), caminhos de arquivo, identificadores técnicos, e termos técnicos universais quando apropriado.

## Propósito do Checklist: "Testes Unitários para Inglês"

**CONCEITO CRÍTICO**: Checklists são **TESTES UNITÁRIOS PARA ESCRITA DE REQUISITOS** - eles validam a qualidade, clareza, e completude de requisitos em um dado domínio.

**NÃO para verificação/teste**:

- ❌ NÃO "Verificar se o botão clica corretamente"
- ❌ NÃO "Testar se tratamento de erros funciona"
- ❌ NÃO "Confirmar se a API retorna 200"
- ❌ NÃO verificar se código/implementação corresponde à spec

**PARA validação de qualidade de requisitos**:

- ✅ "Requisitos de hierarquia visual estão definidos para todos os tipos de card?" (completude)
- ✅ "'Exibição proeminente' está quantificado com dimensionamento/posicionamento específico?" (clareza)
- ✅ "Requisitos de estado hover estão consistentes através de todos os elementos interativos?" (consistência)
- ✅ "Requisitos de acessibilidade estão definidos para navegação por teclado?" (cobertura)
- ✅ "A spec define o que acontece quando imagem de logo falha ao carregar?" (casos de borda)

**Metáfora**: Se sua spec é código escrito em português, o checklist é sua suíte de testes unitários. Você está testando se os requisitos estão bem escritos, completos, não ambíguos, e prontos para implementação - NÃO se a implementação funciona.

## Entrada do Usuário

```text
$ARGUMENTS
```

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Passos de Execução

1. **Setup**: Execute `{SCRIPT}` da raiz do repositório e analise JSON para FEATURE_DIR e lista AVAILABLE_DOCS.
   - Todos os caminhos de arquivo devem ser absolutos.
   - Para aspas simples em args como "I'm Groot", use sintaxe de escape: ex 'I'\''m Groot' (ou aspas duplas se possível: "I'm Groot").

2. **Esclarecer intenção (dinâmico)**: Derive até TRÊS perguntas iniciais de esclarecimento contextual (sem catálogo pré-definido). Elas DEVEM:
   - Ser geradas do fraseado do usuário + sinais extraídos de spec/plan/tasks
   - Apenas perguntar sobre informação que materialmente muda conteúdo do checklist
   - Ser puladas individualmente se já não ambíguas em `$ARGUMENTS`
   - Preferir precisão sobre amplitude

   Algoritmo de geração:
   1. Extrair sinais: palavras-chave de domínio da funcionalidade (ex., auth, latência, UX, API), indicadores de risco ("crítico", "deve", "conformidade"), dicas de stakeholder ("QA", "revisão", "equipe de segurança"), e entregáveis explícitos ("a11y", "rollback", "contratos").
   2. Agrupar sinais em áreas de foco candidatas (máx 4) ranqueadas por relevância.
   3. Identificar provável audiência & timing (autor, revisor, QA, release) se não explícito.
   4. Detectar dimensões faltantes: amplitude de escopo, profundidade/rigor, ênfase de risco, limites de exclusão, critérios de aceitação mensuráveis.
   5. Formular perguntas escolhidas destes arquétipos:
      - Refinamento de escopo (ex., "Isso deve incluir pontos de integração com X e Y ou permanecer limitado a corretude de módulo local?")
      - Priorização de risco (ex., "Quais destas áreas de risco potencial devem receber verificações de gate obrigatórias?")
      - Calibração de profundidade (ex., "É uma lista de sanidade leve de pré-commit ou um gate formal de release?")
      - Enquadramento de audiência (ex., "Será usado apenas pelo autor ou por pares durante revisão de PR?")
      - Exclusão de limite (ex., "Devemos explicitamente excluir itens de tuning de performance desta rodada?")
      - Lacuna de classe de cenário (ex., "Nenhum fluxo de recuperação detectado—caminhos de rollback / falha parcial estão no escopo?")

   Regras de formatação de perguntas:
   - Se apresentar opções, gere tabela compacta com colunas: Opção | Candidato | Por que Importa
   - Limite a opções A–E máximo; omita tabela se resposta livre for mais clara
   - Nunca peça ao usuário para repetir o que ele já disse
   - Evite categorias especulativas (sem alucinação). Se incerto, pergunte explicitamente: "Confirme se X pertence ao escopo."

   Padrões quando interação impossível:
   - Profundidade: Padrão
   - Audiência: Revisor (PR) se relacionado a código; Autor caso contrário
   - Foco: Top 2 clusters de relevância

   Mostre as perguntas (rotule P1/P2/P3). Após respostas: se ≥2 classes de cenário (Alternativo / Exceção / Recuperação / domínio Não-Funcional) permanecem não claras, você PODE fazer até MAIS DUAS perguntas de acompanhamento direcionadas (P4/P5) com justificativa de uma linha cada (ex., "Risco de caminho de recuperação não resolvido"). Não exceda cinco perguntas totais. Pule escalação se usuário explicitamente recusar mais.

3. **Entender requisição do usuário**: Combine `$ARGUMENTS` + respostas de esclarecimento:
   - Derive tema do checklist (ex., segurança, revisão, deploy, ux)
   - Consolide itens obrigatórios explícitos mencionados pelo usuário
   - Mapeie seleções de foco para scaffolding de categoria
   - Infira qualquer contexto faltante de spec/plan/tasks (NÃO alucine)

4. **Carregar contexto da funcionalidade**: Leia de FEATURE_DIR:
   - spec.md: Requisitos e escopo da funcionalidade
   - plan.md (se existir): Detalhes técnicos, dependências
   - tasks.md (se existir): Tarefas de implementação

   **Estratégia de Carregamento de Contexto**:
   - Carregue apenas porções necessárias relevantes para áreas de foco ativas (evite dumping de arquivo completo)
   - Prefira sumarizar seções longas em bullets concisos de cenário/requisito
   - Use divulgação progressiva: adicione recuperação de acompanhamento apenas se lacunas detectadas
   - Se docs fonte são grandes, gere itens de resumo intermediário ao invés de embutir texto bruto

5. **Gerar checklist** - Crie "Testes Unitários para Requisitos":
   - Crie diretório `FEATURE_DIR/checklists/` se não existir
   - Gere nome de arquivo único para checklist:
     - Use nome curto e descritivo baseado no domínio (ex., `ux.md`, `api.md`, `seguranca.md`)
     - Formato: `[dominio].md`
     - Se arquivo existir, adicione ao arquivo existente
   - Numere itens sequencialmente começando de CHK001
   - Cada execução de `/speckit.checklist` cria um arquivo NOVO (nunca sobrescreve checklists existentes)

   **PRINCÍPIO CENTRAL - Teste os Requisitos, Não a Implementação**:
   Todo item de checklist DEVE avaliar os PRÓPRIOS REQUISITOS para:
   - **Completude**: Todos os requisitos necessários estão presentes?
   - **Clareza**: Requisitos são não ambíguos e específicos?
   - **Consistência**: Requisitos alinham entre si?
   - **Mensurabilidade**: Requisitos podem ser objetivamente verificados?
   - **Cobertura**: Todos os cenários/casos de borda estão endereçados?

   **Estrutura de Categoria** - Agrupe itens por dimensões de qualidade de requisitos:
   - **Completude de Requisitos** (Todos os requisitos necessários estão documentados?)
   - **Clareza de Requisitos** (Requisitos são específicos e não ambíguos?)
   - **Consistência de Requisitos** (Requisitos alinham sem conflitos?)
   - **Qualidade de Critérios de Aceitação** (Critérios de sucesso são mensuráveis?)
   - **Cobertura de Cenários** (Todos os fluxos/casos estão endereçados?)
   - **Cobertura de Casos de Borda** (Condições de limite estão definidas?)
   - **Requisitos Não-Funcionais** (Performance, Segurança, Acessibilidade, etc. - estão especificados?)
   - **Dependências & Suposições** (Estão documentadas e validadas?)
   - **Ambiguidades & Conflitos** (O que precisa esclarecimento?)

   **COMO ESCREVER ITENS DE CHECKLIST - "Testes Unitários para Português"**:

   ❌ **ERRADO** (Testando implementação):
   - "Verificar se landing page exibe 3 cards de episódio"
   - "Testar se estados hover funcionam no desktop"
   - "Confirmar se clique no logo navega para home"

   ✅ **CORRETO** (Testando qualidade de requisitos):
   - "O número exato e layout de episódios em destaque está especificado?" [Completude]
   - "'Exibição proeminente' está quantificado com dimensionamento/posicionamento específico?" [Clareza]
   - "Requisitos de estado hover estão consistentes através de todos os elementos interativos?" [Consistência]
   - "Requisitos de navegação por teclado estão definidos para toda UI interativa?" [Cobertura]
   - "Comportamento de fallback está especificado quando imagem de logo falha ao carregar?" [Casos de Borda]
   - "Estados de carregamento estão definidos para dados de episódio assíncronos?" [Completude]
   - "A spec define hierarquia visual para elementos de UI competindo?" [Clareza]

   **ESTRUTURA DO ITEM**:
   Cada item deve seguir este padrão:
   - Formato de pergunta sobre qualidade do requisito
   - Foco no que está ESCRITO (ou não escrito) na spec/plano
   - Inclua dimensão de qualidade entre colchetes [Completude/Clareza/Consistência/etc.]
   - Referencie seção da spec `[Spec §X.Y]` quando verificando requisitos existentes
   - Use marcador `[Lacuna]` quando verificando requisitos faltantes

   **EXEMPLOS POR DIMENSÃO DE QUALIDADE**:

   Completude:
   - "Requisitos de tratamento de erros estão definidos para todos os modos de falha de API? [Lacuna]"
   - "Requisitos de acessibilidade estão especificados para todos os elementos interativos? [Completude]"
   - "Requisitos de breakpoint mobile estão definidos para layouts responsivos? [Lacuna]"

   Clareza:
   - "'Carregamento rápido' está quantificado com limites de tempo específicos? [Clareza, Spec §RNF-2]"
   - "Critérios de seleção de 'episódios relacionados' estão explicitamente definidos? [Clareza, Spec §RF-5]"
   - "'Proeminente' está definido com propriedades visuais mensuráveis? [Ambiguidade, Spec §RF-4]"

   Consistência:
   - "Requisitos de navegação alinham através de todas as páginas? [Consistência, Spec §RF-10]"
   - "Requisitos de componente de card estão consistentes entre landing e páginas de detalhe? [Consistência]"

   Cobertura:
   - "Requisitos estão definidos para cenários de estado-zero (sem episódios)? [Cobertura, Caso de Borda]"
   - "Cenários de interação de usuário concorrente estão endereçados? [Cobertura, Lacuna]"
   - "Requisitos estão especificados para falhas parciais de carregamento de dados? [Cobertura, Fluxo de Exceção]"

   Mensurabilidade:
   - "Requisitos de hierarquia visual são mensuráveis/testáveis? [Critérios de Aceitação, Spec §RF-1]"
   - "'Peso visual balanceado' pode ser objetivamente verificado? [Mensurabilidade, Spec §RF-2]"

   **Classificação de Cenário & Cobertura** (Foco em Qualidade de Requisitos):
   - Verifique se requisitos existem para: cenários Primário, Alternativo, Exceção/Erro, Recuperação, Não-Funcional
   - Para cada classe de cenário, pergunte: "Requisitos de [tipo de cenário] estão completos, claros, e consistentes?"
   - Se classe de cenário faltando: "Requisitos de [tipo de cenário] estão intencionalmente excluídos ou faltando? [Lacuna]"
   - Inclua resiliência/rollback quando mutação de estado ocorre: "Requisitos de rollback estão definidos para falhas de migração? [Lacuna]"

   **Requisitos de Rastreabilidade**:
   - MÍNIMO: ≥80% dos itens DEVEM incluir pelo menos uma referência de rastreabilidade
   - Cada item deve referenciar: seção da spec `[Spec §X.Y]`, ou usar marcadores: `[Lacuna]`, `[Ambiguidade]`, `[Conflito]`, `[Suposição]`
   - Se nenhum sistema de ID existe: "Esquema de ID de requisito & critérios de aceitação está estabelecido? [Rastreabilidade]"

   **Superficiar & Resolver Problemas** (Problemas de Qualidade de Requisitos):
   Faça perguntas sobre os próprios requisitos:
   - Ambiguidades: "O termo 'rápido' está quantificado com métricas específicas? [Ambiguidade, Spec §RNF-1]"
   - Conflitos: "Requisitos de navegação conflitam entre §RF-10 e §RF-10a? [Conflito]"
   - Suposições: "A suposição de 'API de podcast sempre disponível' está validada? [Suposição]"
   - Dependências: "Requisitos de API externa de podcast estão documentados? [Dependência, Lacuna]"
   - Definições faltantes: "'Hierarquia visual' está definida com critérios mensuráveis? [Lacuna]"

   **Consolidação de Conteúdo**:
   - Soft cap: Se itens candidatos brutos > 40, priorize por risco/impacto
   - Mescle quase-duplicatas verificando o mesmo aspecto de requisito
   - Se >5 casos de borda de baixo impacto, crie um item: "Casos de borda X, Y, Z estão endereçados nos requisitos? [Cobertura]"

   **🚫 ABSOLUTAMENTE PROIBIDO** - Estes fazem um teste de implementação, não de requisitos:
   - ❌ Qualquer item começando com "Verificar", "Testar", "Confirmar", "Checar" + comportamento de implementação
   - ❌ Referências a execução de código, ações de usuário, comportamento de sistema
   - ❌ "Exibe corretamente", "funciona apropriadamente", "funciona como esperado"
   - ❌ "Clicar", "navegar", "renderizar", "carregar", "executar"
   - ❌ Casos de teste, planos de teste, procedimentos de QA
   - ❌ Detalhes de implementação (frameworks, APIs, algoritmos)

   **✅ PADRÕES OBRIGATÓRIOS** - Estes testam qualidade de requisitos:
   - ✅ "[tipo de requisito] está definido/especificado/documentado para [cenário]?"
   - ✅ "[termo vago] está quantificado/esclarecido com critérios específicos?"
   - ✅ "Requisitos estão consistentes entre [seção A] e [seção B]?"
   - ✅ "[requisito] pode ser objetivamente medido/verificado?"
   - ✅ "[casos de borda/cenários] estão endereçados nos requisitos?"
   - ✅ "A spec define [aspecto faltante]?"

6. **Referência de Estrutura**: Gere o checklist seguindo o template canônico em `templates/pt-BR/checklist-template.md` para título, seção meta, cabeçalhos de categoria, e formatação de ID. Se template não disponível, use: título H1, linhas meta de propósito/criado, seções de categoria `##` contendo linhas `- [ ] CHK### <item de requisito>` com IDs incrementando globalmente começando em CHK001.

7. **Reportar**: Saída do caminho completo para checklist criado, contagem de itens, e lembre usuário que cada execução cria um arquivo novo. Sumarize:
   - Áreas de foco selecionadas
   - Nível de profundidade
   - Ator/timing
   - Quaisquer itens obrigatórios especificados pelo usuário incorporados

**Importante**: Cada invocação do comando `/speckit.checklist` cria um arquivo de checklist usando nomes curtos e descritivos a menos que arquivo já exista. Isso permite:

- Múltiplos checklists de diferentes tipos (ex., `ux.md`, `teste.md`, `seguranca.md`)
- Nomes de arquivo simples e memoráveis que indicam propósito do checklist
- Fácil identificação e navegação na pasta `checklists/`

Para evitar desordem, use tipos descritivos e limpe checklists obsoletos quando terminar.
