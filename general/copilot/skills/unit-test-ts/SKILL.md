# Unit Test TS

## Purpose
Prover as garantias exigidas pelo `TEST_PLAN.md` sem ditar o framework final (Jest, Vitest), entregando testes locais robustos para o TypeScript.

## When to use
- Imediatamente após a finalização de cada *slice* passível de unidade no plano de implementação.

## When not to use
- Em lógicas e2e densas que cruzam rede, cujo lugar são testes de integração.

## Expected inputs
- Códigos alterados em TS.
- Confirmação do framework (ver `copilot-instructions.md`).

## Operating steps
1. Adote o padrão de nomenclatura da "verdade local" (ex: `*.test.ts` vs `*.spec.ts`).
2. Utilize o padrão **Arrange/Act/Assert**.
3. Mocke limites externos e isole puramente a regra em pauta usando as abordagens nativas do Jest ou Vitest (ex. `vi.mock()` ou `jest.mock()`).
4. Force falhas (red phase) para diagnosticar se o assert não é um "falso positivo" bobo garantindo 100% de coverage inútil.

## Quality bar
Testes sem falsos positivos. Sem testes testando se o console.log foi chamado, exceto se essa for a feature principal absoluta do Worker. 

## Expected outputs
Arquivos `.test.ts` / `.spec.ts` prontos e comandos falhos ou de sucesso passando na stack de testes local.

## Common failure modes
- Acoplar o teste ao banco de dados chamando persistência ou criando sujeira de I/O por não mockar os repositórios injetados no controller TS.

## Minimal checklist
- [ ] O teste foca num comportamento e não somente na validação do compiler e tipos primitivos de retorno?

## Stack-specific notes
Suporte livre para Jest ou Vitest, a confirmação virá do boilerplate local.
