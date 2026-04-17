# Test Plan Authoring

## Purpose
Explicitar de forma pragmática e técnica como os cenários BDD serão cobrados ou ignorados durante a fase de engenharia, criando o esqueleto que guiará as test bags sem escrever a implementação do assert na hora.

## When to use
- Após a aprovação do arquivo de BDD.
- Quando times precisarem de limites: O que será um unit test rápido e o que necessita de integração pesada (banco).

## When not to use
- Em puros e simples hotfixes de interface pontual sem mutação algorítmica ou em migrações cruas de dados em batch.

## Expected inputs
- `BDD.md` completo.
- Restrições técnicas do repositório (ele possui integração fácil? TestContainers?).

## Operating steps
1. Agrupe os BDDs pela camada do funil (Unitário ou E2E/Integração?).
2. Dite quais regressões precisam rodar antes de aprovar.
3. Deixe claro se existe um cenário ignorado intencionalmente e o porquê.

## Quality bar
Diferenciar fortemente "Mockamos o provider Y" contra "Subimos o container X" e onde.

## Expected outputs
Um `TEST_PLAN.md` que diz extamente *quais* classes ou fluxos devem ter tests criados pelo agente engenheiro sem que esse agente sugue tokens decidindo essas diretivas arquiteturais em voo.

## Common failure modes
- Mandar testar de forma e2e lógicas que pertenceriam unitariamente apenas a uma class util.

## Minimal checklist
- [ ] Especificou o que é unitário vs integração?
- [ ] Abordou restrições de falsos positivos? (Mockar vs Não Mockar)
