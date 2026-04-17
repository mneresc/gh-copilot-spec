# BDD to Test Plan Workflow

## Purpose
Estabelecer rigidamente o guia de testes ("como a qualidade será provada") transformando os BDDs canônicos antes aceitos em Test Bags executáveis ou test suites.

## Entry condition
BDD.md escrito e aceitado (Feature/Slice).

## Inputs
- `BDD.md`

## Roles involved
- Solution Architect
- Test Auditor (Auxílio)

## Ordered steps
1. Avaliar os blocos de *Given/When/Then* do BDD atual.
2. Invocar *test-plan-authoring* pra arquitetar e definir o limite (Mock x Integração).
3. Assinar o `TEST_PLAN.md` como aceito limitando que nenhum passo ali deixará de ser cumprido.
