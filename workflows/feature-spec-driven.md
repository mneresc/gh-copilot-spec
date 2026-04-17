# Feature Spec-Driven Workflow

## Purpose
Seria irresponsável guiar o desenvolvimento de dezenas de dias de esforço humano unicamente via Prompts avulsos ("chat-driven"). Este Workflow força o pilar "Specification-Driven", sendo o workflow master e crítico que abriga ponta a ponta o Delivery seguro de software orquestrado por múltiplos IAs focados.

## Entry condition
Apenas quando uma história crua for priorizada na board, ou uma grande ideia for chancelada para início de código.

## Inputs
- Um Ticket (Jira, Linear, Trello).
- Conhecimento do repo e dos domínios macro.

## Roles involved
- PM BDD Manager
- Solution Architect
- Pair Engineer
- Auditors (Security, Test, Microservices)
- Release Readiness Reviewer

## Ordered steps
1. **Story Intake & Slice Scoping**: Ocorre o refinamento. Demanda gigante vira cortada. (Skill: *story-intake*).
2. **Spec Authoring**: O acordo mútuo gravado em pedra do que PODE e NÃO PODE ser feito (Gera `FEATURE_SPEC.md`).
3. **BDD Authoring**: Comportamento mapeado antes do DB existir. (Gera `BDD.md`).
4. **Test Plan Authoring**: Arquitetura da Qualidade (Gera `TEST_PLAN.md`).
5. **Execution Planning**: Fatiamento de Commits para os IAs em blocos e marcos não destrutivos parciais (Gera `PLAN.md`).
6. **Implementation by slice (Pair-Engineer)**: Código gerado em blocos rígidos atualizando transacionalmente o status (`STATUS.md`).
7. **Spec-aware audit**: Code completado mas antes do commit final onde uma bateria fria checa furos contra os 4 artefatos. (`AUDIT.md`).
8. **Merge gate**: Autorização para PR pela agregação central (Release Readiness).

## Decision points
- Se as Specs do BDD falharem na auditoria o processo não avança pro Merge. A Pipeline interrompe no final e o Dev volta para o passo 6.

## Outputs
- Código funcional aderente.
- Suite documental sincronizada contendo: `FEATURE_SPEC.md`, `BDD.md`, `TEST_PLAN.md`, `PLAN.md`, `STATUS.md`, e o `AUDIT.md`. Entregues e atualizados.

## Quality checks
- O artefato é quem governa; IA que obedece código ignorando o que ele assinou na Spec será corrigida ou expurgada via Prompts restritivos.
