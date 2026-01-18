> 🇧🇷 Esta documentação está em Português do Brasil. [English version](./CONTRIBUTING.md)

# Contribuindo para o Spec Kit

Olá! Estamos felizes que você gostaria de contribuir para o Spec Kit. Contribuições para este projeto são [liberadas](https://help.github.com/articles/github-terms-of-service/#6-contributions-under-repository-license) ao público sob a [licença open source do projeto](LICENSE).

Por favor, note que este projeto é lançado com um [Código de Conduta do Contribuidor](CODE_OF_CONDUCT.md). Ao participar deste projeto você concorda em seguir seus termos.

## Pré-requisitos para executar e testar código

Estas são instalações únicas necessárias para poder testar suas alterações localmente como parte do processo de submissão de pull request (PR).

1. Instale [Python 3.11+](https://www.python.org/downloads/)
1. Instale [uv](https://docs.astral.sh/uv/) para gerenciamento de pacotes
1. Instale [Git](https://git-scm.com/downloads)
1. Tenha um [agente de codificação de IA disponível](README.pt-BR.md#-agentes-de-ia-suportados)

<details>
<summary><b>💡 Dica se você está usando <code>VSCode</code> ou <code>GitHub Codespaces</code> como sua IDE</b></summary>

<br>

Desde que você tenha [Docker](https://docker.com) instalado em sua máquina, você pode aproveitar [Dev Containers](https://containers.dev) através desta [extensão do VSCode](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers), para configurar facilmente seu ambiente de desenvolvimento, com as ferramentas mencionadas já instaladas e configuradas, graças ao arquivo `.devcontainer/devcontainer.json` (localizado na raiz do projeto).

Para fazer isso, simplesmente:

- Faça checkout do repositório
- Abra com VSCode
- Abra a [Paleta de Comandos](https://code.visualstudio.com/docs/getstarted/userinterface#_command-palette) e selecione "Dev Containers: Open Folder in Container..."

No [GitHub Codespaces](https://github.com/features/codespaces) é ainda mais simples, pois ele aproveita o `.devcontainer/devcontainer.json` automaticamente ao abrir o codespace.

</details>

## Submetendo um pull request

> [!NOTE]
> Se seu pull request introduz uma grande mudança que impacta materialmente o trabalho da CLI ou do resto do repositório (ex., você está introduzindo novos templates, argumentos ou outras mudanças importantes), certifique-se de que foi **discutido e acordado** pelos mantenedores do projeto. Pull requests com grandes mudanças que não tiveram uma conversa e acordo prévios serão fechados.

1. Faça fork e clone o repositório
1. Configure e instale as dependências: `uv sync`
1. Certifique-se de que a CLI funciona em sua máquina: `uv run specify --help`
1. Crie uma nova branch: `git checkout -b minha-nova-branch`
1. Faça sua alteração, adicione testes e certifique-se de que tudo ainda funciona
1. Teste a funcionalidade da CLI com um projeto de amostra se relevante
1. Faça push para seu fork e submeta um pull request
1. Aguarde seu pull request ser revisado e mesclado.

Aqui estão algumas coisas que você pode fazer que aumentarão a probabilidade de seu pull request ser aceito:

- Siga as convenções de codificação do projeto.
- Escreva testes para novas funcionalidades.
- Atualize a documentação (`README.md`, `spec-driven.md`) se suas alterações afetarem funcionalidades voltadas ao usuário.
- Mantenha sua alteração o mais focada possível. Se há múltiplas alterações que você gostaria de fazer que não são dependentes umas das outras, considere submetê-las como pull requests separados.
- Escreva uma [boa mensagem de commit](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html).
- Teste suas alterações com o fluxo de trabalho de Desenvolvimento Orientado por Especificação para garantir compatibilidade.

## Fluxo de trabalho de desenvolvimento

Ao trabalhar no spec-kit:

1. Teste alterações com os comandos da CLI `specify` (`/speckit.specify`, `/speckit.plan`, `/speckit.tasks`) no seu agente de codificação de escolha
2. Verifique se os templates estão funcionando corretamente no diretório `templates/`
3. Teste a funcionalidade de scripts no diretório `scripts/`
4. Certifique-se de que arquivos de memória (`memory/constitution.md`) são atualizados se grandes mudanças de processo forem feitas

### Testando alterações de templates e comandos localmente

Executar `uv run specify init` baixa pacotes lançados, que não incluirão suas alterações locais.
Para testar seus templates, comandos e outras alterações localmente, siga estes passos:

1. **Crie pacotes de release**

   Execute o seguinte comando para gerar os pacotes locais:

   ```bash
   ./.github/workflows/scripts/create-release-packages.sh v1.0.0
   ```

2. **Copie o pacote relevante para seu projeto de teste**

   ```bash
   cp -r .genreleases/sdd-copilot-package-sh/. <caminho-para-projeto-teste>/
   ```

3. **Abra e teste o agente**

   Navegue até a pasta do seu projeto de teste e abra o agente para verificar sua implementação.

## Contribuições de IA no Spec Kit

> [!IMPORTANT]
>
> Se você está usando **qualquer tipo de assistência de IA** para contribuir com o Spec Kit,
> isso deve ser divulgado no pull request ou issue.

Damos boas-vindas e encorajamos o uso de ferramentas de IA para ajudar a melhorar o Spec Kit! Muitas contribuições valiosas foram aprimoradas com assistência de IA para geração de código, detecção de problemas e definição de funcionalidades.

Dito isso, se você está usando qualquer tipo de assistência de IA (ex., agentes, ChatGPT) enquanto contribui para o Spec Kit,
**isso deve ser divulgado no pull request ou issue**, junto com a extensão em que a assistência de IA foi usada (ex., comentários de documentação vs. geração de código).

Se suas respostas ou comentários de PR estão sendo gerados por uma IA, divulgue isso também.

Como exceção, correções triviais de espaçamento ou erros de digitação não precisam ser divulgadas, desde que as alterações sejam limitadas a pequenas partes do código ou frases curtas.

Um exemplo de divulgação:

> Este PR foi escrito principalmente pelo GitHub Copilot.

Ou uma divulgação mais detalhada:

> Consultei o ChatGPT para entender a base de código, mas a solução
> foi totalmente escrita manualmente por mim.

Não divulgar isso é, primeiro e acima de tudo, rude para os operadores humanos do outro lado do pull request, mas também torna difícil
determinar quanta análise aplicar à contribuição.

Em um mundo perfeito, assistência de IA produziria trabalho de qualidade igual ou superior a qualquer humano. Esse não é o mundo em que vivemos hoje, e na maioria dos casos
onde supervisão ou expertise humana não está no loop, está gerando código que não pode ser razoavelmente mantido ou evoluído.

### O que estamos procurando

Ao submeter contribuições assistidas por IA, por favor garanta que incluem:

- **Divulgação clara do uso de IA** - Você é transparente sobre o uso de IA e o grau em que está usando para a contribuição
- **Compreensão e teste humano** - Você pessoalmente testou as alterações e entende o que elas fazem
- **Justificativa clara** - Você pode explicar por que a alteração é necessária e como ela se encaixa nos objetivos do Spec Kit
- **Evidência concreta** - Inclua casos de teste, cenários ou exemplos que demonstram a melhoria
- **Sua própria análise** - Compartilhe seus pensamentos sobre a experiência de desenvolvedor end-to-end

### O que vamos fechar

Reservamos o direito de fechar contribuições que pareçam ser:

- Alterações não testadas submetidas sem verificação
- Sugestões genéricas que não abordam necessidades específicas do Spec Kit
- Submissões em massa que não mostram revisão ou compreensão humana

### Diretrizes para sucesso

A chave é demonstrar que você entende e validou suas alterações propostas. Se um mantenedor pode facilmente dizer que uma contribuição foi gerada inteiramente por IA sem input ou teste humano, ela provavelmente precisa de mais trabalho antes da submissão.

Contribuidores que consistentemente submetem alterações de baixo esforço geradas por IA podem ser restritos de contribuições futuras a critério dos mantenedores.

Por favor, seja respeitoso com os mantenedores e divulgue assistência de IA.

## Adicionando Novas Traduções de Idioma

O Spec Kit suporta múltiplos idiomas para templates e conteúdo gerado por IA. Para adicionar suporte para um novo idioma:

### 1. Crie Diretórios de Template

Crie os diretórios específicos de idioma seguindo o formato de código de idioma [BCP 47](https://pt.wikipedia.org/wiki/Etiqueta_de_l%C3%ADngua_da_IETF) (ex., `es` para Espanhol, `fr` para Francês, `de` para Alemão, `ja` para Japonês):

```bash
mkdir -p templates/<codigo-idioma>
mkdir -p templates/constitution/<codigo-idioma>
mkdir -p templates/commands/<codigo-idioma>
```

### 2. Traduza Arquivos de Template

Copie e traduza os seguintes arquivos de `templates/en/`:

- `spec-template.md` - Template de especificação de funcionalidade
- `plan-template.md` - Template de plano de implementação
- `tasks-template.md` - Template de lista de tarefas
- `checklist-template.md` - Template de checklist
- `agent-file-template.md` - Template de arquivo de contexto de agente

**Importante**: Preserve todos os tokens de placeholder exatamente como aparecem na versão em inglês:
- `[FEATURE NAME]`, `[DATE]`, `$ARGUMENTS`
- IDs de tarefa como `T001`, `T002`
- Marcadores como `[P]`, `[US1]`, `[US2]`
- Blocos de código e caminhos técnicos

### 3. Traduza o Template de Constituição

Copie e traduza `templates/constitution/en/constitution.md` para o diretório do seu idioma. Mantenha tokens de placeholder como `[PROJECT_NAME]`, `[PRINCIPLE_1_NAME]`, `[CONSTITUTION_VERSION]` inalterados.

### 4. Crie Prompts de Comando

Para cada arquivo em `templates/commands/en/`, crie uma versão traduzida em `templates/commands/<codigo-idioma>/`. Cada arquivo de comando deve incluir uma seção **Language Directive** após o frontmatter YAML:

```markdown
## Language Directive

**IMPORTANTE**: Gere TODO o conteúdo em [Seu Idioma]. Isso inclui:
- Todas as seções de especificação
- Descrições de histórias de usuário
- Requisitos funcionais e não funcionais
- Critérios de sucesso
- Mensagens de erro e validação
- Comentários e notas

Mantenha em inglês apenas: nomes de branch, IDs de tarefa, placeholders técnicos como `[FEATURE NAME]`, `$ARGUMENTS`, `[DATE]`, e termos técnicos universais quando apropriado.
```

### 5. Atualize Escolhas de Idioma

Adicione seu idioma ao dicionário `LANGUAGE_CHOICES` em `src/specify_cli/__init__.py`:

```python
LANGUAGE_CHOICES = {
    "en": "English (Default)",
    "pt-BR": "Português (Brasil)",
    "<seu-codigo>": "<Nome do Seu Idioma>",
}
```

### 6. Teste Sua Tradução

1. Execute `specify init projeto-teste --ai claude --language <seu-codigo>`
2. Verifique se `.specify/config.json` contém o idioma correto
3. Verifique se os templates em `.specify/templates/<seu-codigo>/` estão adequadamente traduzidos
4. Teste o comando `/speckit.specify` para garantir que a IA gera conteúdo no idioma correto

### Diretrizes de Tradução

- Use tom formal/profissional consistente com documentação técnica
- Mantenha termos técnicos que são comumente usados em sua forma original em inglês (ex., "API", "CLI", "OAuth2")
- Garanta que a tradução seja natural e idiomática para falantes nativos
- Considere variações regionais se aplicável (ex., `pt-BR` para Português do Brasil vs `pt-PT` para Português Europeu)

## Recursos

- [Metodologia de Desenvolvimento Orientado por Especificação](./spec-driven.pt-BR.md)
- [Como Contribuir para Open Source](https://opensource.guide/pt/how-to-contribute/)
- [Usando Pull Requests](https://help.github.com/articles/about-pull-requests/)
- [Ajuda do GitHub](https://help.github.com)
