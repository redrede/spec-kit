> 🇧🇷 Esta documentação está em Português do Brasil. [English version](../quickstart.md)

# Guia de Início Rápido

Este guia ajudará você a começar com o Desenvolvimento Orientado por Especificação usando o Spec Kit.

> [!NOTE]
> Todos os scripts de automação agora fornecem variantes Bash (`.sh`) e PowerShell (`.ps1`). A CLI `specify` seleciona automaticamente baseado no SO, a menos que você passe `--script sh|ps`.

## O Processo de 6 Passos

> [!TIP]
> **Consciência de Contexto**: Os comandos do Spec Kit detectam automaticamente a funcionalidade ativa baseado na sua branch Git atual (ex., `001-nome-funcionalidade`). Para alternar entre diferentes especificações, simplesmente troque de branch Git.

### Passo 1: Instale o Specify

**No seu terminal**, execute o comando CLI `specify` para inicializar seu projeto:

```bash
# Criar um novo diretório de projeto
uvx --from git+https://github.com/github/spec-kit.git specify init <NOME_DO_PROJETO>

# OU inicializar no diretório atual
uvx --from git+https://github.com/github/spec-kit.git specify init .
```

Escolha o tipo de script explicitamente (opcional):

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <NOME_DO_PROJETO> --script ps  # Forçar PowerShell
uvx --from git+https://github.com/github/spec-kit.git specify init <NOME_DO_PROJETO> --script sh  # Forçar shell POSIX
```

### Passo 2: Defina Sua Constituição

**Na interface de chat do seu Agente de IA**, use o slash command `/speckit.constitution` para estabelecer as regras e princípios centrais do seu projeto. Você deve fornecer os princípios específicos do seu projeto como argumentos.

```markdown
/speckit.constitution Este projeto segue uma abordagem "Library-First". Todas as funcionalidades devem ser implementadas como bibliotecas standalone primeiro. Usamos TDD estritamente. Preferimos padrões de programação funcional.
```

### Passo 3: Crie a Especificação

**No chat**, use o slash command `/speckit.specify` para descrever o que você quer construir. Foque no **o quê** e no **por quê**, não na stack tecnológica.

```markdown
/speckit.specify Construir uma aplicação que pode me ajudar a organizar minhas fotos em álbuns separados. Álbuns são agrupados por data e podem ser reorganizados arrastando e soltando na página principal. Álbuns nunca estão aninhados dentro de outros álbuns. Dentro de cada álbum, as fotos são visualizadas em uma interface de grade.
```

### Passo 4: Refine a Especificação

**No chat**, use o slash command `/speckit.clarify` para identificar e resolver ambiguidades na sua especificação. Você pode fornecer áreas de foco específicas como argumentos.

```bash
/speckit.clarify Foque nos requisitos de segurança e performance.
```

### Passo 5: Crie um Plano de Implementação Técnica

**No chat**, use o slash command `/speckit.plan` para fornecer sua stack tecnológica e escolhas de arquitetura.

```markdown
/speckit.plan A aplicação usa Vite com número mínimo de bibliotecas. Use HTML, CSS e JavaScript vanilla o máximo possível. Imagens não são enviadas para nenhum lugar e os metadados são armazenados em um banco de dados SQLite local.
```

### Passo 6: Divida e Implemente

**No chat**, use o slash command `/speckit.tasks` para criar uma lista de tarefas acionáveis.

```markdown
/speckit.tasks
```

Opcionalmente, valide o plano com `/speckit.analyze`:

```markdown
/speckit.analyze
```

Então, use o slash command `/speckit.implement` para executar o plano.

```markdown
/speckit.implement
```

## Exemplo Detalhado: Construindo o Taskify

Aqui está um exemplo completo de construção de uma plataforma de produtividade de equipe:

### Passo 1: Defina a Constituição

Inicialize a constituição do projeto para definir regras fundamentais:

```markdown
/speckit.constitution Taskify é uma aplicação "Security-First". Todas as entradas de usuário devem ser validadas. Usamos arquitetura de microsserviços. O código deve ser totalmente documentado.
```

### Passo 2: Defina Requisitos com `/speckit.specify`

```text
Desenvolver Taskify, uma plataforma de produtividade de equipe. Deve permitir que usuários criem projetos, adicionem membros da equipe,
atribuam tarefas, comentem e movam tarefas entre quadros em estilo Kanban. Nesta fase inicial para esta funcionalidade,
vamos chamá-la de "Criar Taskify", vamos ter múltiplos usuários mas os usuários serão declarados antecipadamente, predefinidos.
Quero cinco usuários em duas categorias diferentes, um gerente de produto e quatro engenheiros. Vamos criar três
projetos de amostra diferentes. Vamos ter as colunas Kanban padrão para o status de cada tarefa, como "A Fazer",
"Em Progresso", "Em Revisão" e "Concluído". Não haverá login para esta aplicação pois isso é apenas o
primeiro teste para garantir que nossas funcionalidades básicas estão configuradas.
```

### Passo 3: Refine a Especificação

Use o comando `/speckit.clarify` para resolver interativamente quaisquer ambiguidades na sua especificação. Você também pode fornecer detalhes específicos que quer garantir que sejam incluídos.

```bash
/speckit.clarify Quero clarificar os detalhes do card de tarefa. Para cada tarefa na UI para um card de tarefa, você deve poder mudar o status atual da tarefa entre as diferentes colunas no quadro de trabalho Kanban. Você deve poder deixar um número ilimitado de comentários para um card específico. Você deve poder, a partir desse card de tarefa, atribuir um dos usuários válidos.
```

Você pode continuar a refinar a especificação com mais detalhes usando `/speckit.clarify`:

```bash
/speckit.clarify Quando você iniciar o Taskify pela primeira vez, ele vai te dar uma lista dos cinco usuários para escolher. Não será necessária senha. Quando você clicar em um usuário, você vai para a visualização principal, que mostra a lista de projetos. Quando você clica em um projeto, você abre o quadro Kanban para aquele projeto. Você vai ver as colunas. Você poderá arrastar e soltar cards entre diferentes colunas. Você verá qualquer card que está atribuído a você, o usuário atualmente logado, em uma cor diferente de todos os outros, para que você possa rapidamente ver os seus. Você pode editar qualquer comentário que você fez, mas não pode editar comentários que outras pessoas fizeram. Você pode deletar qualquer comentário que você fez, mas não pode deletar comentários que qualquer outra pessoa fez.
```

### Passo 4: Valide a Especificação

Valide a checklist de especificação usando o comando `/speckit.checklist`:

```bash
/speckit.checklist
```

### Passo 5: Gere o Plano Técnico com `/speckit.plan`

Seja específico sobre sua stack tecnológica e requisitos técnicos:

```bash
/speckit.plan Vamos gerar isso usando .NET Aspire, usando Postgres como banco de dados. O frontend deve usar Blazor server com quadros de tarefas drag-and-drop, atualizações em tempo real. Deve haver uma API REST criada com uma API de projetos, API de tarefas e uma API de notificações.
```

### Passo 6: Valide e Implemente

Faça seu agente de IA auditar o plano de implementação usando `/speckit.analyze`:

```bash
/speckit.analyze
```

Finalmente, implemente a solução:

```bash
/speckit.implement
```

## Princípios Chave

- **Seja explícito** sobre o que você está construindo e por quê
- **Não foque na stack tecnológica** durante a fase de especificação
- **Itere e refine** suas especificações antes da implementação
- **Valide** o plano antes de começar a codificar
- **Deixe o agente de IA cuidar** dos detalhes de implementação

## Próximos Passos

- Leia a [metodologia completa](../../spec-driven.pt-BR.md) para orientação aprofundada
- Confira [mais exemplos](../../templates) no repositório
- Explore o [código fonte no GitHub](https://github.com/github/spec-kit)
