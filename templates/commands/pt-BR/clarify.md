---
description: Identificar áreas subespecificadas na spec atual da funcionalidade fazendo até 5 perguntas de esclarecimento altamente direcionadas e codificando respostas de volta na spec.
handoffs:
  - label: Criar Plano Técnico
    agent: speckit.plan
    prompt: Crie um plano para a spec. Estou construindo com...
scripts:
   sh: scripts/bash/check-prerequisites.sh --json --paths-only
   ps: scripts/powershell/check-prerequisites.ps1 -Json -PathsOnly
---

## Diretiva de Idioma

**IMPORTANTE**: Gere TODO o conteúdo em Português do Brasil. Isso inclui:
- Perguntas de esclarecimento
- Opções e descrições
- Atualizações da especificação
- Relatórios de cobertura
- Todas as comunicações com o usuário

Mantenha em inglês apenas: caminhos de arquivo, identificadores técnicos, e termos técnicos universais quando apropriado.

## Entrada do Usuário

```text
$ARGUMENTS
```

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Roteiro

Objetivo: Detectar e reduzir ambiguidade ou pontos de decisão faltantes na especificação de funcionalidade ativa e registrar os esclarecimentos diretamente no arquivo spec.

Nota: Este fluxo de esclarecimento é esperado para rodar (e ser completado) ANTES de invocar `/speckit.plan`. Se o usuário explicitamente declara que está pulando esclarecimento (ex., spike exploratório), você pode prosseguir, mas deve avisar que risco de retrabalho downstream aumenta.

Passos de execução:

1. Execute `{SCRIPT}` da raiz do repositório **uma vez** (modo combinado `--json --paths-only` / `-Json -PathsOnly`). Analise campos mínimos do payload JSON:
   - `FEATURE_DIR`
   - `FEATURE_SPEC`
   - (Opcionalmente capture `IMPL_PLAN`, `TASKS` para fluxos encadeados futuros.)
   - Se análise JSON falhar, aborte e instrua usuário a re-executar `/speckit.specify` ou verificar ambiente da branch de funcionalidade.
   - Para aspas simples em args como "I'm Groot", use sintaxe de escape: ex 'I'\''m Groot' (ou aspas duplas se possível: "I'm Groot").

2. Carregue o arquivo spec atual. Realize um scan estruturado de ambiguidade & cobertura usando esta taxonomia. Para cada categoria, marque status: Claro / Parcial / Faltando. Produza um mapa de cobertura interno usado para priorização (não mostre mapa bruto a menos que nenhuma pergunta será feita).

   Escopo Funcional & Comportamento:
   - Objetivos principais do usuário & critérios de sucesso
   - Declarações explícitas de fora-de-escopo
   - Diferenciação de papéis de usuário / personas

   Domínio & Modelo de Dados:
   - Entidades, atributos, relacionamentos
   - Regras de identidade & unicidade
   - Transições de ciclo de vida/estado
   - Suposições de volume de dados / escala

   Interação & Fluxo de UX:
   - Jornadas de usuário / sequências críticas
   - Estados de erro/vazio/carregamento
   - Notas de acessibilidade ou localização

   Atributos de Qualidade Não-Funcional:
   - Performance (metas de latência, throughput)
   - Escalabilidade (horizontal/vertical, limites)
   - Confiabilidade & disponibilidade (uptime, expectativas de recuperação)
   - Observabilidade (sinais de logging, métricas, tracing)
   - Segurança & privacidade (authN/Z, proteção de dados, suposições de ameaças)
   - Restrições de conformidade / regulatórias (se houver)

   Integração & Dependências Externas:
   - Serviços/APIs externos e modos de falha
   - Formatos de importação/exportação de dados
   - Suposições de protocolo/versionamento

   Casos de Borda & Tratamento de Falhas:
   - Cenários negativos
   - Rate limiting / throttling
   - Resolução de conflitos (ex., edições concorrentes)

   Restrições & Tradeoffs:
   - Restrições técnicas (linguagem, storage, hosting)
   - Tradeoffs explícitos ou alternativas rejeitadas

   Terminologia & Consistência:
   - Termos canônicos do glossário
   - Sinônimos evitados / termos deprecados

   Sinais de Conclusão:
   - Testabilidade dos critérios de aceitação
   - Indicadores de estilo de Definição de Feito mensurável

   Misc / Placeholders:
   - Marcadores TODO / decisões não resolvidas
   - Adjetivos ambíguos ("robusto", "intuitivo") sem quantificação

   Para cada categoria com status Parcial ou Faltando, adicione uma oportunidade de pergunta candidata a menos que:
   - Esclarecimento não mudaria materialmente estratégia de implementação ou validação
   - Informação é melhor diferida para fase de planejamento (note internamente)

3. Gere (internamente) uma fila priorizada de perguntas de esclarecimento candidatas (máximo 5). NÃO as mostre todas de uma vez. Aplique estas restrições:
    - Máximo de 10 perguntas totais através de toda a sessão.
    - Cada pergunta deve ser respondível com QUALQUER UM:
       - Uma seleção curta de múltipla escolha (2–5 opções distintas, mutuamente exclusivas), OU
       - Uma resposta de uma palavra / frase curta (explicitamente restrinja: "Responda em <=5 palavras").
    - Apenas inclua perguntas cujas respostas impactam materialmente arquitetura, modelagem de dados, decomposição de tarefas, design de testes, comportamento de UX, prontidão operacional, ou validação de conformidade.
    - Garanta balanço de cobertura de categoria: tente cobrir as categorias não resolvidas de maior impacto primeiro; evite fazer duas perguntas de baixo impacto quando uma única área de alto impacto (ex., postura de segurança) está não resolvida.
    - Exclua perguntas já respondidas, preferências estilísticas triviais, ou detalhes de execução a nível de plano (a menos que bloqueie corretude).
    - Favoreça esclarecimentos que reduzem risco de retrabalho downstream ou previnem testes de aceitação desalinhados.
    - Se mais de 5 categorias permanecem não resolvidas, selecione as top 5 por heurística (Impacto * Incerteza).

4. Loop de questionamento sequencial (interativo):
    - Apresente EXATAMENTE UMA pergunta por vez.
    - Para perguntas de múltipla escolha:
       - **Analise todas as opções** e determine a **opção mais adequada** baseada em:
          - Melhores práticas para o tipo de projeto
          - Padrões comuns em implementações similares
          - Redução de risco (segurança, performance, manutenibilidade)
          - Alinhamento com quaisquer objetivos ou restrições explícitas do projeto visíveis na spec
       - Apresente sua **opção recomendada proeminentemente** no topo com raciocínio claro (1-2 frases explicando por que esta é a melhor escolha).
       - Formate como: `**Recomendado:** Opção [X] - <raciocínio>`
       - Então renderize todas as opções como tabela Markdown:

       | Opção | Descrição |
       |-------|-----------|
       | A | <Descrição da Opção A> |
       | B | <Descrição da Opção B> |
       | C | <Descrição da Opção C> (adicione D/E conforme necessário até 5) |
       | Curta | Forneça uma resposta curta diferente (<=5 palavras) (Inclua apenas se alternativa livre for apropriada) |

       - Após a tabela, adicione: `Você pode responder com a letra da opção (ex., "A"), aceitar a recomendação dizendo "sim" ou "recomendado", ou fornecer sua própria resposta curta.`
    - Para estilo de resposta curta (sem opções discretas significativas):
       - Forneça sua **resposta sugerida** baseada em melhores práticas e contexto.
       - Formate como: `**Sugerido:** <sua resposta proposta> - <raciocínio breve>`
       - Então mostre: `Formato: Resposta curta (<=5 palavras). Você pode aceitar a sugestão dizendo "sim" ou "sugerido", ou fornecer sua própria resposta.`
    - Após o usuário responder:
       - Se o usuário responde com "sim", "recomendado", ou "sugerido", use sua recomendação/sugestão declarada anteriormente como a resposta.
       - Caso contrário, valide que a resposta mapeia para uma opção ou cabe na restrição de <=5 palavras.
       - Se ambíguo, peça uma desambiguação rápida (contagem ainda pertence à mesma pergunta; não avance).
       - Uma vez satisfatório, registre em memória de trabalho (não escreva em disco ainda) e mova para próxima pergunta da fila.
    - Pare de fazer mais perguntas quando:
       - Todas as ambiguidades críticas resolvidas cedo (itens restantes da fila tornam-se desnecessários), OU
       - Usuário sinaliza conclusão ("feito", "bom", "sem mais"), OU
       - Você atinge 5 perguntas feitas.
    - Nunca revele perguntas futuras da fila antecipadamente.
    - Se nenhuma pergunta válida existe no início, imediatamente reporte nenhuma ambiguidade crítica.

5. Integração após CADA resposta aceita (abordagem de atualização incremental):
    - Mantenha representação em memória da spec (carregada uma vez no início) mais conteúdos brutos do arquivo.
    - Para a primeira resposta integrada nesta sessão:
       - Garanta que seção `## Esclarecimentos` existe (crie-a logo após a seção contextual/visão geral de mais alto nível conforme template da spec se faltando).
       - Sob ela, crie (se não presente) um subcabeçalho `### Sessão YYYY-MM-DD` para hoje.
    - Adicione uma linha de bullet imediatamente após aceitação: `- P: <pergunta> → R: <resposta final>`.
    - Então imediatamente aplique o esclarecimento à(s) seção(ões) mais apropriada(s):
       - Ambiguidade funcional → Atualize ou adicione bullet em Requisitos Funcionais.
       - Interação do usuário / distinção de ator → Atualize Histórias de Usuário ou subseção Atores (se presente) com papel, restrição, ou cenário esclarecido.
       - Forma de dados / entidades → Atualize Modelo de Dados (adicione campos, tipos, relacionamentos) preservando ordenação; note restrições adicionadas sucintamente.
       - Restrição não-funcional → Adicione/modifique critérios mensuráveis na seção Não-Funcional / Atributos de Qualidade (converta adjetivo vago para métrica ou alvo explícito).
       - Caso de borda / fluxo negativo → Adicione novo bullet sob Casos de Borda / Tratamento de Erros (ou crie tal subseção se template fornece placeholder para isso).
       - Conflito de terminologia → Normalize termo através da spec; retenha original apenas se necessário adicionando `(anteriormente referido como "X")` uma vez.
    - Se o esclarecimento invalida uma declaração ambígua anterior, substitua aquela declaração ao invés de duplicar; não deixe texto contraditório obsoleto.
    - Salve o arquivo spec APÓS cada integração para minimizar risco de perda de contexto (sobrescrita atômica).
    - Preserve formatação: não reordene seções não relacionadas; mantenha hierarquia de cabeçalhos intacta.
    - Mantenha cada esclarecimento inserido mínimo e testável (evite deriva narrativa).

6. Validação (realizada após CADA escrita mais passe final):
   - Sessão de esclarecimentos contém exatamente um bullet por resposta aceita (sem duplicatas).
   - Total de perguntas feitas (aceitas) ≤ 5.
   - Seções atualizadas não contêm placeholders vagos persistentes que a nova resposta deveria resolver.
   - Nenhuma declaração anterior contraditória permanece (scan para escolhas alternativas agora-inválidas removidas).
   - Estrutura Markdown válida; únicos novos cabeçalhos permitidos: `## Esclarecimentos`, `### Sessão YYYY-MM-DD`.
   - Consistência de terminologia: mesmo termo canônico usado através de todas as seções atualizadas.

7. Escreva a spec atualizada de volta para `FEATURE_SPEC`.

8. Reporte conclusão (após loop de questionamento terminar ou término antecipado):
   - Número de perguntas feitas & respondidas.
   - Caminho para spec atualizada.
   - Seções tocadas (liste nomes).
   - Tabela de resumo de cobertura listando cada categoria de taxonomia com Status: Resolvido (era Parcial/Faltando e abordado), Diferido (excede quota de perguntas ou melhor adequado para planejamento), Claro (já suficiente), Pendente (ainda Parcial/Faltando mas baixo impacto).
   - Se algum Pendente ou Diferido permanecer, recomende se proceder para `/speckit.plan` ou executar `/speckit.clarify` novamente mais tarde pós-plano.
   - Comando próximo sugerido.

Regras de comportamento:

- Se nenhuma ambiguidade significativa encontrada (ou todas as perguntas potenciais seriam baixo-impacto), responda: "Nenhuma ambiguidade crítica detectada que valha esclarecimento formal." e sugira prosseguir.
- Se arquivo spec faltando, instrua usuário a executar `/speckit.specify` primeiro (não crie nova spec aqui).
- Nunca exceda 5 perguntas totais feitas (tentativas de esclarecimento para uma única pergunta não contam como novas perguntas).
- Evite perguntas especulativas de stack tecnológica a menos que ausência bloqueie clareza funcional.
- Respeite sinais de término antecipado do usuário ("parar", "feito", "prosseguir").
- Se nenhuma pergunta feita devido a cobertura total, mostre resumo compacto de cobertura (todas categorias Claras) então sugira avançar.
- Se quota atingida com categorias de alto-impacto não resolvidas restantes, explicitamente sinalize-as sob Diferido com justificativa.

Contexto para priorização: {ARGS}
