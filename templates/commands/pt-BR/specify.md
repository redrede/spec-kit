---
description: Criar ou atualizar a especificação de funcionalidade a partir de uma descrição em linguagem natural.
handoffs:
  - label: Criar Plano Técnico
    agent: speckit.plan
    prompt: Crie um plano para a especificação. Estou construindo com...
  - label: Esclarecer Requisitos da Spec
    agent: speckit.clarify
    prompt: Esclarecer requisitos da especificação
    send: true
scripts:
  sh: scripts/bash/create-new-feature.sh --json "{ARGS}"
  ps: scripts/powershell/create-new-feature.ps1 -Json "{ARGS}"
---

## Diretiva de Idioma

**IMPORTANTE**: Gere TODO o conteúdo em Português do Brasil. Isso inclui:
- Todas as seções da especificação
- Descrições de histórias de usuário
- Requisitos funcionais e não-funcionais
- Critérios de sucesso
- Mensagens de erro e validação
- Comentários e notas

Mantenha em inglês apenas: nomes de branches, IDs de tarefas, placeholders técnicos como `[FEATURE NAME]`, `$ARGUMENTS`, `[DATE]`, e termos técnicos universais quando apropriado.

## Entrada do Usuário

```text
$ARGUMENTS
```

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Roteiro

O texto que o usuário digitou após `/speckit.specify` na mensagem disparadora **é** a descrição da funcionalidade. Assuma que você sempre tem isso disponível nesta conversa mesmo se `{ARGS}` aparecer literalmente abaixo. Não peça ao usuário para repetir a menos que ele tenha fornecido um comando vazio.

Dada essa descrição de funcionalidade, faça isso:

1. **Gere um nome curto conciso** (2-4 palavras) para a branch:
   - Analise a descrição da funcionalidade e extraia as palavras-chave mais significativas
   - Crie um nome curto de 2-4 palavras que capture a essência da funcionalidade
   - Use formato ação-substantivo quando possível (ex., "add-user-auth", "fix-payment-bug")
   - Preserve termos técnicos e acrônimos (OAuth2, API, JWT, etc.)
   - Mantenha conciso mas descritivo o suficiente para entender a funcionalidade rapidamente
   - Exemplos:
     - "Quero adicionar autenticação de usuário" → "user-auth"
     - "Implementar integração OAuth2 para a API" → "oauth2-api-integration"
     - "Criar um dashboard para analytics" → "analytics-dashboard"
     - "Corrigir bug de timeout no processamento de pagamento" → "fix-payment-timeout"

2. **Verifique branches existentes antes de criar nova**:

   a. Primeiro, busque todas as branches remotas para garantir que temos as informações mais recentes:

      ```bash
      git fetch --all --prune
      ```

   b. Encontre o maior número de funcionalidade em todas as fontes para o short-name:
      - Branches remotas: `git ls-remote --heads origin | grep -E 'refs/heads/[0-9]+-<short-name>$'`
      - Branches locais: `git branch | grep -E '^[* ]*[0-9]+-<short-name>$'`
      - Diretórios de specs: Verifique diretórios correspondendo a `specs/[0-9]+-<short-name>`

   c. Determine o próximo número disponível:
      - Extraia todos os números das três fontes
      - Encontre o maior número N
      - Use N+1 para o número da nova branch

   d. Execute o script `{SCRIPT}` com o número calculado e short-name:
      - Passe `--number N+1` e `--short-name "your-short-name"` junto com a descrição da funcionalidade
      - Exemplo Bash: `{SCRIPT} --json --number 5 --short-name "user-auth" "Adicionar autenticação de usuário"`
      - Exemplo PowerShell: `{SCRIPT} -Json -Number 5 -ShortName "user-auth" "Adicionar autenticação de usuário"`

   **IMPORTANTE**:
   - Verifique todas as três fontes (branches remotas, branches locais, diretórios de specs) para encontrar o maior número
   - Apenas corresponda branches/diretórios com o padrão exato de short-name
   - Se nenhuma branch/diretório existente encontrado com este short-name, comece com número 1
   - Você deve executar este script apenas uma vez por funcionalidade
   - O JSON é fornecido no terminal como saída - sempre consulte-o para obter o conteúdo real que você está procurando
   - A saída JSON conterá BRANCH_NAME e caminhos SPEC_FILE
   - Para aspas simples em args como "I'm Groot", use sintaxe de escape: ex 'I'\''m Groot' (ou aspas duplas se possível: "I'm Groot")

3. Carregue `templates/pt-BR/spec-template.md` para entender as seções obrigatórias.

4. Siga este fluxo de execução:

    1. Analise a descrição do usuário da Entrada
       Se vazia: ERRO "Nenhuma descrição de funcionalidade fornecida"
    2. Extraia conceitos-chave da descrição
       Identifique: atores, ações, dados, restrições
    3. Para aspectos não claros:
       - Faça suposições informadas baseadas em contexto e padrões da indústria
       - Apenas marque com [NECESSITA ESCLARECIMENTO: pergunta específica] se:
         - A escolha impacta significativamente o escopo da funcionalidade ou experiência do usuário
         - Múltiplas interpretações razoáveis existem com diferentes implicações
         - Nenhum padrão razoável existe
       - **LIMITE: Máximo 3 marcadores [NECESSITA ESCLARECIMENTO] no total**
       - Priorize esclarecimentos por impacto: escopo > segurança/privacidade > experiência do usuário > detalhes técnicos
    4. Preencha a seção Cenários de Usuário & Testes
       Se nenhum fluxo de usuário claro: ERRO "Não é possível determinar cenários de usuário"
    5. Gere Requisitos Funcionais
       Cada requisito deve ser testável
       Use padrões razoáveis para detalhes não especificados (documente suposições na seção Suposições)
    6. Defina Critérios de Sucesso
       Crie resultados mensuráveis e agnósticos de tecnologia
       Inclua tanto métricas quantitativas (tempo, performance, volume) quanto medidas qualitativas (satisfação do usuário, conclusão de tarefa)
       Cada critério deve ser verificável sem detalhes de implementação
    7. Identifique Entidades Principais (se dados envolvidos)
    8. Retorne: SUCESSO (spec pronta para planejamento)

5. Escreva a especificação em SPEC_FILE usando a estrutura do template, substituindo placeholders com detalhes concretos derivados da descrição da funcionalidade (argumentos) enquanto preserva a ordem das seções e cabeçalhos.

6. **Validação de Qualidade da Especificação**: Após escrever a spec inicial, valide-a contra critérios de qualidade:

   a. **Crie Checklist de Qualidade da Spec**: Gere um arquivo de checklist em `FEATURE_DIR/checklists/requirements.md` usando a estrutura do template de checklist com estes itens de validação:

      ```markdown
      # Checklist de Qualidade da Especificação: [NOME DA FUNCIONALIDADE]

      **Propósito**: Validar completude e qualidade da especificação antes de prosseguir para o planejamento
      **Criado em**: [DATA]
      **Funcionalidade**: [Link para spec.md]

      ## Qualidade do Conteúdo

      - [ ] Sem detalhes de implementação (linguagens, frameworks, APIs)
      - [ ] Focado em valor para o usuário e necessidades de negócio
      - [ ] Escrito para stakeholders não-técnicos
      - [ ] Todas as seções obrigatórias completadas

      ## Completude dos Requisitos

      - [ ] Nenhum marcador [NECESSITA ESCLARECIMENTO] permanece
      - [ ] Requisitos são testáveis e não ambíguos
      - [ ] Critérios de sucesso são mensuráveis
      - [ ] Critérios de sucesso são agnósticos de tecnologia (sem detalhes de implementação)
      - [ ] Todos os cenários de aceitação estão definidos
      - [ ] Casos de borda estão identificados
      - [ ] Escopo está claramente delimitado
      - [ ] Dependências e suposições identificadas

      ## Prontidão da Funcionalidade

      - [ ] Todos os requisitos funcionais têm critérios de aceitação claros
      - [ ] Cenários de usuário cobrem fluxos primários
      - [ ] Funcionalidade atende resultados mensuráveis definidos nos Critérios de Sucesso
      - [ ] Nenhum detalhe de implementação vaza na especificação

      ## Notas

      - Itens marcados como incompletos requerem atualizações da spec antes de `/speckit.clarify` ou `/speckit.plan`
      ```

   b. **Execute Verificação de Validação**: Revise a spec contra cada item do checklist:
      - Para cada item, determine se passa ou falha
      - Documente problemas específicos encontrados (cite seções relevantes da spec)

   c. **Trate Resultados da Validação**:

      - **Se todos os itens passarem**: Marque checklist como completo e prossiga para o passo 6

      - **Se itens falharem (excluindo [NECESSITA ESCLARECIMENTO])**:
        1. Liste os itens que falharam e problemas específicos
        2. Atualize a spec para resolver cada problema
        3. Re-execute validação até todos os itens passarem (máx 3 iterações)
        4. Se ainda falhar após 3 iterações, documente problemas restantes nas notas do checklist e avise o usuário

      - **Se marcadores [NECESSITA ESCLARECIMENTO] permanecerem**:
        1. Extraia todos os marcadores [NECESSITA ESCLARECIMENTO: ...] da spec
        2. **VERIFICAÇÃO DE LIMITE**: Se mais de 3 marcadores existirem, mantenha apenas os 3 mais críticos (por impacto de escopo/segurança/UX) e faça suposições informadas para o resto
        3. Para cada esclarecimento necessário (máx 3), apresente opções ao usuário neste formato:

           ```markdown
           ## Pergunta [N]: [Tópico]

           **Contexto**: [Cite seção relevante da spec]

           **O que precisamos saber**: [Pergunta específica do marcador NECESSITA ESCLARECIMENTO]

           **Respostas Sugeridas**:

           | Opção | Resposta | Implicações |
           |-------|----------|-------------|
           | A     | [Primeira resposta sugerida] | [O que isso significa para a funcionalidade] |
           | B     | [Segunda resposta sugerida] | [O que isso significa para a funcionalidade] |
           | C     | [Terceira resposta sugerida] | [O que isso significa para a funcionalidade] |
           | Personalizado | Forneça sua própria resposta | [Explique como fornecer entrada personalizada] |

           **Sua escolha**: _[Aguarde resposta do usuário]_
           ```

        4. **CRÍTICO - Formatação de Tabela**: Garanta que tabelas markdown estejam formatadas corretamente:
           - Use espaçamento consistente com pipes alinhados
           - Cada célula deve ter espaços ao redor do conteúdo: `| Conteúdo |` não `|Conteúdo|`
           - Separador de cabeçalho deve ter pelo menos 3 traços: `|--------|`
           - Teste que a tabela renderiza corretamente na prévia markdown
        5. Numere perguntas sequencialmente (P1, P2, P3 - máx 3 no total)
        6. Apresente todas as perguntas juntas antes de aguardar respostas
        7. Aguarde o usuário responder com suas escolhas para todas as perguntas (ex., "P1: A, P2: Personalizado - [detalhes], P3: B")
        8. Atualize a spec substituindo cada marcador [NECESSITA ESCLARECIMENTO] com a resposta selecionada ou fornecida pelo usuário
        9. Re-execute validação após todos os esclarecimentos serem resolvidos

   d. **Atualize Checklist**: Após cada iteração de validação, atualize o arquivo de checklist com status atual de passa/falha

7. Relate conclusão com nome da branch, caminho do arquivo spec, resultados do checklist, e prontidão para a próxima fase (`/speckit.clarify` ou `/speckit.plan`).

**NOTA:** O script cria e faz checkout da nova branch e inicializa o arquivo spec antes de escrever.

## Diretrizes Gerais

## Diretrizes Rápidas

- Foque em **O QUE** os usuários precisam e **POR QUÊ**.
- Evite COMO implementar (sem stack tecnológica, APIs, estrutura de código).
- Escrito para stakeholders de negócio, não desenvolvedores.
- NÃO crie nenhum checklist embutido na spec. Isso será um comando separado.

### Requisitos de Seção

- **Seções obrigatórias**: Devem ser completadas para toda funcionalidade
- **Seções opcionais**: Inclua apenas quando relevante para a funcionalidade
- Quando uma seção não se aplica, remova-a completamente (não deixe como "N/A")

### Para Geração por IA

Ao criar esta spec a partir de um prompt do usuário:

1. **Faça suposições informadas**: Use contexto, padrões da indústria e padrões comuns para preencher lacunas
2. **Documente suposições**: Registre padrões razoáveis na seção Suposições
3. **Limite esclarecimentos**: Máximo 3 marcadores [NECESSITA ESCLARECIMENTO] - use apenas para decisões críticas que:
   - Impactam significativamente escopo da funcionalidade ou experiência do usuário
   - Têm múltiplas interpretações razoáveis com diferentes implicações
   - Não têm nenhum padrão razoável
4. **Priorize esclarecimentos**: escopo > segurança/privacidade > experiência do usuário > detalhes técnicos
5. **Pense como um testador**: Todo requisito vago deve falhar no item "testável e não ambíguo" do checklist
6. **Áreas comuns que precisam esclarecimento** (apenas se nenhum padrão razoável existir):
   - Escopo e limites da funcionalidade (incluir/excluir casos de uso específicos)
   - Tipos de usuário e permissões (se múltiplas interpretações conflitantes possíveis)
   - Requisitos de segurança/conformidade (quando legalmente/financeiramente significativo)

**Exemplos de padrões razoáveis** (não pergunte sobre estes):

- Retenção de dados: Práticas padrão da indústria para o domínio
- Metas de performance: Expectativas padrão de app web/mobile a menos que especificado
- Tratamento de erros: Mensagens amigáveis ao usuário com fallbacks apropriados
- Método de autenticação: Padrão baseado em sessão ou OAuth2 para apps web
- Padrões de integração: APIs RESTful a menos que especificado diferente

### Diretrizes de Critérios de Sucesso

Critérios de sucesso devem ser:

1. **Mensuráveis**: Inclua métricas específicas (tempo, porcentagem, contagem, taxa)
2. **Agnósticos de tecnologia**: Sem menção de frameworks, linguagens, bancos de dados ou ferramentas
3. **Focados no usuário**: Descreva resultados da perspectiva do usuário/negócio, não internos do sistema
4. **Verificáveis**: Podem ser testados/validados sem conhecer detalhes de implementação

**Bons exemplos**:

- "Usuários podem completar checkout em menos de 3 minutos"
- "Sistema suporta 10.000 usuários simultâneos"
- "95% das buscas retornam resultados em menos de 1 segundo"
- "Taxa de conclusão de tarefas melhora em 40%"

**Exemplos ruins** (focados em implementação):

- "Tempo de resposta da API é menor que 200ms" (muito técnico, use "Usuários veem resultados instantaneamente")
- "Banco de dados pode lidar com 1000 TPS" (detalhe de implementação, use métrica voltada ao usuário)
- "Componentes React renderizam eficientemente" (específico de framework)
- "Taxa de cache hit do Redis acima de 80%" (específico de tecnologia)
