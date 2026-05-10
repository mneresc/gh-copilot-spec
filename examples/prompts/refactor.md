# Prompt — Refactor

**Quando usar:** Melhorar a estrutura interna de um código já coberto por testes, sem mudar o comportamento externo.
**Modelo:** Sonnet
**Skills relacionadas:** `unit-test-ts` ou `unit-test-python`

```markdown
[Modelo: Sonnet]
Refactor: [descreva o que e por que]

1. Confirme que os testes existentes passam antes de começar
2. Aplique o refactor mantendo a interface pública inalterada
3. Rode os testes — se algum quebrar, corrija o refactor, não o teste
4. Self-review: o diff é menor que ~200 linhas? Se não, quebre em etapas.
```
