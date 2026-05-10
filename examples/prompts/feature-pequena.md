# Prompt — Feature Pequena

**Quando usar:** Funcionalidades com 1-2 cenários claros.
**Modelo:** Sonnet (Mude para Opus no plan se a tarefa se mostrar ambígua).
**Skills relacionadas:** `bdd-scenario-authoring` (se quiser formalizar cenários), `ts-service-implementation` ou `python-worker-implementation`.
**Workflow relacionado:** `pair-implement.md`.

```markdown
[Modelo: Sonnet]
Feature: [descreva o comportamento desejado em 1-2 frases]

1. Mini-plan: liste os arquivos afetados e a abordagem em bullets
   (máx 5 bullets). Don't implement yet — espere meu OK.
2. [após OK] Implemente seguindo o plan. Inclua testes para os
   cenários: [cenário feliz] e [cenário de erro].
3. Self-review: revise se o código segue os padrões de
   copilot-instructions.md e se não há lógica fora do escopo pedido.
```
