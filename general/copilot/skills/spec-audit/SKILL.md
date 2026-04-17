# Spec Audit

## Purpose
Efetuar o re-cruzamento frio e imparcial do código gerado no repositório contra os 3 artefatos base (`FEATURE_SPEC.md`, `BDD.md` e `TEST_PLAN.md`) em busca de "Feature Creep", esquecimentos ou desvios de design.

## When to use
- Antes de gerar PRs. Pelo final do Workflow `feature-spec-driven.md`.

## When not to use
- Se a feature não é ancorada em Specs e Plan. Scripts abertos soltos não carecem de auditoria cruzada documental.

## Expected inputs
- Código e test suites finalizados.
- FEATURE_SPEC, BDD, e TEST_PLAN preenchidos no repo `docs/`.

## Operating steps
1. Leia `FEATURE_SPEC.md`: Cruzar a seção "Fora de Escopo". Alguém programou código para algo que estava fora do escopo ali estipulado?
2. Leia `BDD.md`: Todos os cenários (Happy path, Edges, Falhas) possuem mapeamento nas test suites dos artefatos locais criados pelo Dev?
3. Leia `TEST_PLAN.md`: A orientação de mockar e não cruzar dependências se manteve ou há um cheiro de teste E2E fantasiado de test unitário quebrando a pipeline?

## Quality bar
Auditoria de verificação documental pesada e impiedosa. Rejeita o avanço e levanta as infrações estritas gerando relatorios binários sem margem de interpretação solta.

## Expected outputs
Construção minuciosa do documento `AUDIT.md` na área de docs que carimba o estado "Approved / With Caveats / Failed" daquela execução de Sprint da branch e os motivos do "failed" provando com links pro código gerado onde ele destoou da spec.

## Common failure modes
- Aceitar que um Dev implementou "Coisas úteis bônus que não tavam na spec", permitindo o inchaço inútil daquele artefato ou criando código morto escondido.

## Minimal checklist
- [ ] Leu e revalidou 100% de exclusões de Fora de Escopo contra a Codebase final gerada nesta fase?
- [ ] O relatório aponta desvios e os caminhos/linhas pro fix prático?
