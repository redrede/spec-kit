# Plano de Implementação: [FEATURE]

**Branch**: `[###-feature-name]` | **Data**: [DATE] | **Spec**: [link]
**Entrada**: Especificação de funcionalidade de `/specs/[###-feature-name]/spec.md`

**Nota**: Este template é preenchido pelo comando `/speckit.plan`. Veja `.specify/templates/commands/plan.md` para o fluxo de execução.

## Resumo

[Extrair da especificação: requisito principal + abordagem técnica da pesquisa]

## Contexto Técnico

<!--
  AÇÃO NECESSÁRIA: Substitua o conteúdo nesta seção com os detalhes técnicos
  do projeto. A estrutura aqui é apresentada como orientação para guiar
  o processo de iteração.
-->

**Linguagem/Versão**: [ex., Python 3.11, Swift 5.9, Rust 1.75 ou NECESSITA ESCLARECIMENTO]
**Dependências Principais**: [ex., FastAPI, UIKit, LLVM ou NECESSITA ESCLARECIMENTO]
**Armazenamento**: [se aplicável, ex., PostgreSQL, CoreData, arquivos ou N/A]
**Testes**: [ex., pytest, XCTest, cargo test ou NECESSITA ESCLARECIMENTO]
**Plataforma Alvo**: [ex., servidor Linux, iOS 15+, WASM ou NECESSITA ESCLARECIMENTO]
**Tipo de Projeto**: [único/web/mobile - determina estrutura do código]
**Metas de Performance**: [específico do domínio, ex., 1000 req/s, 10k linhas/seg, 60 fps ou NECESSITA ESCLARECIMENTO]
**Restrições**: [específico do domínio, ex., <200ms p95, <100MB memória, funciona offline ou NECESSITA ESCLARECIMENTO]
**Escala/Escopo**: [específico do domínio, ex., 10k usuários, 1M LOC, 50 telas ou NECESSITA ESCLARECIMENTO]

## Verificação da Constituição

*GATE: Deve passar antes da Fase 0 de pesquisa. Re-verificar após Fase 1 de design.*

[Gates determinados com base no arquivo de constituição]

## Estrutura do Projeto

### Documentação (esta funcionalidade)

```text
specs/[###-feature]/
├── plan.md              # Este arquivo (saída do comando /speckit.plan)
├── research.md          # Saída da Fase 0 (comando /speckit.plan)
├── data-model.md        # Saída da Fase 1 (comando /speckit.plan)
├── quickstart.md        # Saída da Fase 1 (comando /speckit.plan)
├── contracts/           # Saída da Fase 1 (comando /speckit.plan)
└── tasks.md             # Saída da Fase 2 (comando /speckit.tasks - NÃO criado por /speckit.plan)
```

### Código Fonte (raiz do repositório)
<!--
  AÇÃO NECESSÁRIA: Substitua a árvore de exemplo abaixo pelo layout concreto
  para esta funcionalidade. Delete opções não utilizadas e expanda a estrutura
  escolhida com caminhos reais (ex., apps/admin, packages/algo). O plano entregue
  não deve incluir rótulos de Opção.
-->

```text
# [REMOVER SE NÃO USADO] Opção 1: Projeto único (PADRÃO)
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
└── unit/

# [REMOVER SE NÃO USADO] Opção 2: Aplicação web (quando "frontend" + "backend" detectado)
backend/
├── src/
│   ├── models/
│   ├── services/
│   └── api/
└── tests/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   └── services/
└── tests/

# [REMOVER SE NÃO USADO] Opção 3: Mobile + API (quando "iOS/Android" detectado)
api/
└── [mesmo que backend acima]

ios/ ou android/
└── [estrutura específica da plataforma: módulos de funcionalidade, fluxos de UI, testes da plataforma]
```

**Decisão de Estrutura**: [Documente a estrutura selecionada e referencie os
diretórios reais capturados acima]

## Rastreamento de Complexidade

> **Preencher SOMENTE se a Verificação da Constituição tiver violações que precisam ser justificadas**

| Violação | Por que Necessário | Alternativa Mais Simples Rejeitada Porque |
|----------|-------------------|-------------------------------------------|
| [ex., 4º projeto] | [necessidade atual] | [por que 3 projetos são insuficientes] |
| [ex., Padrão Repository] | [problema específico] | [por que acesso direto ao BD é insuficiente] |
