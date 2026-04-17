# BDD Scenario Authoring

## Purpose
Traduzir restrições de negócio brutas e exclusões provindas de specs em cenários precisos usando linguagem Gherkin (Given/When/Then), de forma estritamente legível e testável, sem acoplamento a UI (se o teste for backend).

## When to use
- Antes de qualquer código ser acionado ou gerado. 
- Quando queremos formalizar a expectativa de negócio cruzando com aceitações da Spec.

## When not to use
- Para listar cenários puramente infraestruturais (ex: 'Dado que a AWS não falhe'). BDD testa domínio, não nuvem.

## Expected inputs
- `FEATURE_SPEC.md` ou regras aceitas do negócio.
- Conhecimento do atores do sistema.

## Operating steps
1. Entenda quem opera a ação e quem é o afetado.
2. Formate o Happy Path usando `Given`, `When`, `Then`.
3. Force a extração dos piores casos e cenários marginais (Edge Cases: valores em zero, concorrência).
4. Elenque checagem de Segurança/Autorização no "Given".

## Quality bar
Os arquivos BDD não testam "Clicks de botões" em camadas de backend. Testam domínios de negócio: "Dado um aluguel com 2 dias de atraso..."

## Expected outputs
Cenários catalogados em um artefato `BDD.md`.

## Common failure modes
- Omitir "Quem" pode fazer a instrução. (Falta do "Given Autorização X").
- BDDs atrelados muito próximos à tabelas subjacentes.

## Minimal checklist
- [ ] Possui pelo menos um fluxo feliz mapeado?
- [ ] Abstraiu implementações (não fala de 'tabela tb_cars') mas logica pura?

## Stack-specific notes
Nenhum.
