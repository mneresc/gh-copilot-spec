# Unit Test Create Workflow

## Purpose
Operar ativamente a geração dos arquivos de test suites unitários logo após um slice ou milestone ter suas classes base instanciadas com lógicas a serem desafiadas.

## Entry condition
- Uma classe nova (Controller/Worker/Domain) implementada sob ordens do plano atual.
- Dependências da framework test run (Jest/Pytest) local testadas.

## Inputs
- `.py` ou `.ts` modificados.
- `TEST_PLAN.md`.

## Roles involved
- Pair Engineer.
- Test Auditor.

## Ordered steps
1. IA e humano geram o shell do Spec usando a convenção de testes do repositório (Skill *unit-test-ts/py*).
2. O framework é rodado verificando a Falha local (Red).
3. IA injeta asserts e lógicas mockadas seguindo o Plan.
4. Teste local passa (Green/Refatoração leve).

## Stop conditions
- "Falsos positivos" sendo criados puramente por pressa, onde a IA zera cobertura `100%` através de asserts de Console log inúteis.
