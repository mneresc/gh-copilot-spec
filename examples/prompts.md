# Prompts — Templates por Tipo de Tarefa

> Copie o bloco inteiro no Copilot Chat. Troque o modelo ANTES de enviar.

---

## 1. Tarefa Trivial

> Renomear, mover, ajustar imports, trocar constantes, CSS pontual.

```
[Modelo: Haiku]
Renomeie/mova/ajuste [descreva o que]. Mantenha as importações consistentes.
```

---

## 2. Bug Fix Simples

> Bug com causa-raiz clara, sem impacto em segurança.

```
[Modelo: Sonnet]
Bug: [descreva sintoma e onde ocorre]

1. Analise a causa raiz
2. Escreva um teste que reproduza o bug (red)
3. Implemente o fix mínimo para o teste passar (green)
4. Self-review: verifique se o fix não quebra comportamentos adjacentes
   e se o teste realmente falharia sem o fix
```

**Skills:** `unit-test-ts` · `unit-test-python`
**Workflow:** `incident-hotfix-audit.md` (se P0/P1)

---

## 3. Feature Pequena

> Funcionalidade com 1-2 cenários claros.

```
[Modelo: Sonnet]
Feature: [descreva o comportamento desejado em 1-2 frases]

1. Mini-plan: liste os arquivos afetados e a abordagem em bullets
   (máx 5 bullets). Don't implement yet — espere meu OK.
2. [após OK] Implemente seguindo o plan. Inclua testes para os
   cenários: [cenário feliz] e [cenário de erro].
3. Self-review: revise se o código segue os padrões de
   copilot-instructions.md e se não há lógica fora do escopo pedido.
```

> ⚠️ Se o mini-plan revelar complexidade → troque para **Opus** no plan.

**Skills:** `bdd-scenario-authoring` · `ts-service-implementation` · `python-worker-implementation`
**Workflow:** `pair-implement.md`

---

## 4. Refactor

> Melhorar estrutura interna sem mudar comportamento externo.

```
[Modelo: Sonnet]
Refactor: [descreva o que e por que]

1. Confirme que os testes existentes passam antes de começar
2. Aplique o refactor mantendo a interface pública inalterada
3. Rode os testes — se algum quebrar, corrija o refactor, não o teste
4. Self-review: o diff é menor que ~200 linhas? Se não, quebre em etapas.
```

**Skills:** `unit-test-ts` · `unit-test-python`

---

## 5. Design Exploration (Planejamento Puro)

> Decisão arquitetural, tradeoffs, sem gerar código.

```
[Modelo: Opus]
Preciso decidir como implementar: [descreva o problema/decisão]

Contexto: [stack, restrições, o que já existe]

**DON'T IMPLEMENT YET.** Apenas:
1. Liste 2-3 abordagens possíveis com prós/contras
2. Recomende uma com justificativa
3. Identifique riscos e o que ficaria fora de escopo
4. Indique quais arquivos seriam afetados

Formato: bullets curtos, sem código.
```

**Skills:** `slice-scoping` · `story-intake`

---

## 6. Bug em Código Sensível

> Auth, pagamentos, dados pessoais — qualquer área com risco de segurança.

### Fase 1 — Diagnóstico
```
[Modelo: Opus]
Bug em área sensível ([auth/pagamentos/dados]):
[descreva o bug e o impacto potencial]

1. Analise a superfície de risco: que dados/fluxos podem ser
   afetados por esse bug?
2. DON'T IMPLEMENT YET — espere meu OK no diagnóstico.
```

### Fase 2 — Implementação (após OK)
```
[Modelo: Sonnet]
Implemente o fix mínimo com teste conforme o diagnóstico aprovado.
```

### Fase 3 — Review de Segurança
```
[Modelo: Opus]
Review de segurança do fix acima:
1. O fix abre novas superfícies de ataque?
2. Inputs são sanitizados corretamente?
3. Logs vazam dados sensíveis?
```

**Skills:** `incident-hotfix-review` · `security-review`
**Agent:** `@security-auditor`
