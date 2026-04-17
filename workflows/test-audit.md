# Test Audit Workflow

## Purpose
Efetuar o re-cruzamento frio e imparcial do código de testes gerado no repositório contra os artefatos `BDD.md` e `TEST_PLAN.md` impedindo Mocks falsos.

## Entry condition
Testes implementados e teoricamente em "Green pass" local.

## Roles involved
- Test Auditor

## Ordered steps
1. Acionar Test Auditor com a cópia dos Testes Unitários.
2. Cruzar as exigências.
3. Reprovar ou aprovar no laudo, garantindo que "Falsos Positivos" sejam convertidos de volta em tickets do Dev pra refutar logicas não tratadas de Falhas no Banco.
