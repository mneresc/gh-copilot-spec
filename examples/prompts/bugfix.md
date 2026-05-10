# Prompt — Bug Fix

**Quando usar:** Corrigir bugs simples com causa-raiz clara.
**Modelo:** Sonnet
**Skills relacionadas:** `unit-test-ts` ou `unit-test-python`
**Workflow relacionado:** `incident-hotfix-audit.md` se for P0/P1

```markdown
[Modelo: Sonnet]
Bug: [descreva sintoma e onde ocorre]

1. Analise a causa raiz
2. Escreva um teste que reproduza o bug (red)
3. Implemente o fix mínimo para o teste passar (green)
4. Self-review: verifique se o fix não quebra comportamentos adjacentes
   e se o teste realmente falharia sem o fix
```
