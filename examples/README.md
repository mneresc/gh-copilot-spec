# Fluxo Enxuto e Exemplos Práticos

**Propósito:** Exemplos práticos de uso das skills deste repositório num fluxo enxuto para VSCode + Copilot.

## Índice de Conteúdo

| Arquivo | Descrição | Modelo Recomendado |
|---------|-----------|--------------------|
| [`claude-copilot-workflow.md`](claude-copilot-workflow.md) | **O Workflow Enxuto (Ponto de Entrada)**: Setup, matriz de decisão e regras do dia a dia. | - |
| [`prompts/trivial.md`](prompts/trivial.md) | Prompt para tarefas mecânicas e simples. | Haiku |
| [`prompts/bugfix.md`](prompts/bugfix.md) | Prompt para bugs com causa-raiz clara. | Sonnet |
| [`prompts/feature-pequena.md`](prompts/feature-pequena.md) | Prompt para features com 1-2 cenários. | Sonnet |
| [`prompts/refactor.md`](prompts/refactor.md) | Prompt para refatorações sem mudança de comportamento. | Sonnet |
| [`prompts/design-exploration.md`](prompts/design-exploration.md) | Prompt para planejamento puro e decisões arquiteturais. | Opus |
| [`prompts/bugfix-sensivel.md`](prompts/bugfix-sensivel.md) | Prompt para bugs em áreas críticas (auth, pagamentos). | Opus/Sonnet |
| [`copilot-instructions-example.md`](copilot-instructions-example.md) | Exemplo preenchido da verdade local de um repositório. | - |

## Quando usar o Fluxo Enxuto vs Fluxo Completo

Para decidir entre o fluxo enxuto (descrito nesta pasta) e o fluxo completo guiado por especificações, consulte o [fluxograma de decisão no README principal](../README.md#8-fluxograma-de-decisão-quando-usar-o-quê).

O fluxo completo é ideal quando a tarefa tocar múltiplos serviços, exigir alinhamento com stakeholders, ou envolver mudanças irreversíveis. Para o dia a dia, comece pelo fluxo enxuto.
