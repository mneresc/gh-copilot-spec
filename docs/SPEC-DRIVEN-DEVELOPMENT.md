# Desenvolvimento Orientado a Especificação (Spec-Driven)

Enquanto muitas equipes adotam IAs de conversação iterativa contínua e não documentadas (Chat-Driven), propomos o uso da documentação como limitador imperativo (**Spec-Driven**).

## O Que é e Por Que Não Bastam Skills?
Desenvolvimento Spec-Driven com IAs exige a materialização do entendimento num arquivo consolidado de Markdown ANTES da digitação das rotinas lógicas.
Skills e Prompts de sistema instruem *como* se comportar (tom de voz, frameworks gerais); porém elas **esquecem** ou se diluem quando o chat estoura o limite de tokens. A especificação não.

A leitura do artefato se torna obrigatória como input primário antes de cada alteração nas classes ou módulos.

## Fluxo de Execução
1. Escrevemos requisitos canônicos que a IA deve respeitar sem espaço para alucinar "funcionalidades bacanas mas desnecessárias".
2. Reduzimos escopo explícito em campos como `Fora de Escopo`.
3. Com isso acordado, partimos para os cenários e planos curtos sem que a IA ou o Desenvolvedor divaguem no chat.

## Artefatos Canônicos
Qualquer demanda sob este regime atravessará:
1. `FEATURE_SPEC.md`: Contém a motivação, escopo, exclusões e design básico.
2. `BDD.md`: Formaliza contratos lógicos GWT.
3. `TEST_PLAN.md`: Diz onde falharíamos se não observarmos x e como auditar isso.
4. `PLAN.md`: Roteirização técnica fatiada no modelo "pare aqui e valide".
5. `STATUS.md`: Arquivo transacional provando o que fizemos em qual passo.
6. `AUDIT.md`: Laudo de checagem contra a `FEATURE_SPEC`.
