# Checklist de Decisão — Nova Tarefa

> Percorra de cima para baixo. Pare na primeira resposta "Sim".

---

## Passo 1: Qual modelo?

| # | Pergunta | Sim → | Não → |
|---|----------|-------|-------|
| 1 | Sei exatamente o que fazer e é <20 linhas? | **Haiku** | ↓ |
| 2 | É implementação padrão sem decisão de design? | **Sonnet** | ↓ |
| 3 | Envolve tradeoffs, arquitetura ou segurança? | **Opus** | **Sonnet** (default) |

---

## Passo 2: Qual template de prompt?

| Tipo de tarefa | Template | Modelo |
|----------------|----------|--------|
| Renomear, mover, ajustar imports | `trivial` | Haiku |
| Bug simples | `bugfix` | Sonnet |
| Feature 1-2 cenários | `feature-pequena` | Sonnet |
| Refatoração | `refactor` | Sonnet |
| Decisão de arquitetura | `design-exploration` | Opus |
| Bug em auth/pagamentos/dados | `bugfix-sensivel` | Opus → Sonnet → Opus |

---

## Passo 3: Fluxo enxuto ou estruturado?

| # | Pergunta | Sim = Estruturado |
|---|----------|--------------------|
| 1 | Toca mais de 3 serviços/módulos? | ◻ |
| 2 | Precisa de alinhamento com PM/stakeholder? | ◻ |
| 3 | Mudança irreversível (migração, schema)? | ◻ |
| 4 | Estimativa >4h de trabalho? | ◻ |

**Todos "Não"?** → Fique no fluxo enxuto.
**Algum "Sim"?** → Use o fluxo `feature-spec-driven.md`.

---

## Passo 4: Preciso escrever artefatos em disco?

| Artefato | Quando vale |
|----------|-------------|
| `PLAN.md` | >2 milestones ou >1 dia de trabalho |
| `BDD.md` / `.feature` | Comportamento novo que precisa de aceite |
| `ADR.md` | Decisão arquitetural que outros devs precisam entender |
| **Nenhum** | Tarefas triviais, bug fixes, refactors simples |

---

## Passo 5: Sinais para trocar de modelo no meio

| Sinal | Ação |
|-------|------|
| Haiku e a tarefa revelou ambiguidade | → **Sonnet** |
| Sonnet dá respostas rasas sobre arquitetura | → **Opus** |
| Opus e percebeu que é implementação mecânica | → **Sonnet** |
| Sonnet e é só boilerplate/rename | → **Haiku** |
