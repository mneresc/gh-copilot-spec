# Refatoração da Taxonomia de Agentes — OhMyStudy

Auditoria e reorganização completa de Rules, Workflows, Skills e Artifacts seguindo o modelo oficial do Antigravity.

## Resumo do problema

O workspace atual tem:
- **Duplicação perniciosa**: dois ecossistemas paralelos (`.agent/` para Antigravity, `.agents/` para Codex, `AGENTS.md` raiz) com uma política de sincronização pesada que gera mais entropia do que valor.
- **Skills genéricas**: as 7 skills atuais são pacotes de conhecimento passivo (conceitos, diretrizes) em vez de specializações acionáveis. Nomes como `go-learning-mode` ou `firestore-modeling` descrevem áreas de conhecimento, não capacidades de execução.
- **Personas como cargos crus**: `AGENTS.md` e `.agent/agents.md` listam 12 "personas" (Orchestrator, Product Strategist, Backend Go Engineer, etc.) que nunca são realmente ativadas — são texto morto.
- **Rules bem posicionadas mas com nomes fracos**: as 8 rules existentes em `.agent/rules/` estão corretas em tipo e conteúdo, mas usam nomes numerados pouco descritivos.
- **Workflows razoáveis mas incompletos**: os 11 workflows cobrem o ciclo TDD/BDD mas faltam `/audit-security`, `/audit-sre`, `/teach-go-on-slice`, `/artifact-sync`.
- **Artifacts rasos**: docs como `CONSOLIDATION-ARCHITECTURE.md`, `TEST-STRATEGY.md` e `LOCAL-DEV.md` são stubs com menos de 400 bytes.
- **Arquivos avulsos na raiz**: `implementation_plan.md`, `specslice_signup_signin.md`, `tutorial_ciclo_completo.md`, `tutorial_curso_agente.md` estão soltos.

---

## Decisão arquitetural proposta

> [!IMPORTANT]
> **Eliminar o ecossistema Codex** (`.agents/`, `docs/codex-playbooks/`, mapeamentos dual-system) e manter apenas o ecossistema Antigravity (`.agent/`) como fonte única de verdade.
> 
> Justificativa: manter dois ecossistemas sincronizados manualmente é custoso e frágil. O `AGENTS.md` na raiz continuará como user_rules injetado, mas será reescrito para referenciar a nova estrutura sem duplicar conteúdo.

> [!WARNING]
> **Eliminação da política dual-system**: os arquivos `docs/dual-system-sync-policy.md` e `docs/dual-system-mapping.md` serão arquivados em `docs/archived/` e não terão mais vigência ativa. Se você ainda usa Codex ativamente, informe antes da execução.

---

## Diagnóstico completo

### Tabela de reclassificação

| Item atual | Tipo atual | Tipo correto | Ação | Justificativa |
|---|---|---|---|---|
| `.agent/rules/00-project-operating-system.md` | Rule ✅ | Rule | Renomear → `no-big-bang-slices.md` | Nome descritivo |
| `.agent/rules/05-learning-go-first.md` | Rule ✅ | Rule | Renomear → `go-learning-first.md` | Nome descritivo |
| `.agent/rules/10-bdd-tdd-red-green-refactor.md` | Rule ✅ | Rule | Renomear → `bdd-first-tdd-mandatory.md` | Nome descritivo |
| `.agent/rules/15-local-dev-first.md` | Rule ✅ | Rule | Renomear → `local-dev-first.md` | Nome descritivo |
| `.agent/rules/20-architecture-study-platform-greenfield.md` | Rule ✅ | Rule | Renomear → `backend-greenfield.md` | Nome descritivo |
| `.agent/rules/30-openapi-as-reference.md` | Rule ✅ | Rule | Renomear → `openapi-as-reference-not-source-of-truth.md` | Nome descritivo |
| `.agent/rules/40-consolidation-architecture.md` | Rule ✅ | Rule | Renomear → `manifest-and-bucket-pattern.md` | Nome descritivo |
| `.agent/rules/50-docker-and-emulators.md` | Rule ✅ | Rule | Fundir em `local-dev-first.md` | Docker/emuladores fazem parte do princípio local-dev-first |
| Personas em `agents.md` e `AGENTS.md` (12 cargos) | Texto inerte | Eliminar | Remover seção de personas e converter em skills acionáveis | Personas como cargos não influenciam o Antigravity |
| Seção dual-system sync em `agents.md` e `AGENTS.md` | Instrução | Eliminar | Remover | Ecossistema Codex será eliminado |
| `.agent/skills/consolidation-artifacts/` | Skill ✅ | Skill | Renomear → `python-consolidation-worker` + enriquecer | Orientar à capacidade, não ao conceito |
| `.agent/skills/firebase-emulators-local/` | Skill ❌ | Skill | Renomear → `docker-firebase-local-dev` + enriquecer | Unir Docker + Firebase emuladores numa skill só |
| `.agent/skills/firestore-modeling/` | Skill ✅ | Skill | Renomear → `firestore-modeling-review` | Enfatizar capacidade de revisão |
| `.agent/skills/go-learning-mode/` | Skill ❌ | Rule já existe | Fundir em rule `go-learning-first.md` | O conteúdo é regra permanente, não skill sob demanda. Criar skill `senior-go-engineer-implementation` separada |
| `.agent/skills/openapi-gap-analysis/` | Skill ✅ | Skill | Manter + enriquecer | Já é acionável |
| `.agent/skills/slice-classification/` | Skill ✅ | Skill | Renomear → `pm-slice-scoping` | Reclassificar como capacidade de PM |
| `.agent/skills/tdd-go-http/` | Skill ✅ | Skill | Renomear → `tdd-test-design` + generalizar | Ampliar para além de HTTP |
| `.agent/workflows/audit.md` | Workflow ✅ | Workflow | Dividir em `/audit-security` e `/audit-sre`, eliminar `/audit` genérico | Auditorias focadas são mais acionáveis |
| `.agent/workflows/ship.md` | Workflow ✅ | Workflow | Manter | Funciona bem |
| Outros 9 workflows | Workflow ✅ | Workflow | Manter | Conteúdo adequado |
| `docs/dual-system-sync-policy.md` | Doc ativo | Arquivo morto | Mover → `docs/archived/` | Política dual-system eliminada |
| `docs/dual-system-mapping.md` | Doc ativo | Arquivo morto | Mover → `docs/archived/` | Política dual-system eliminada |
| `docs/PROMPTS-EXEMPLO.md` | Doc | Artifact | Mover → `docs/archived/` | Exemplos obsoletos com comandos duplicados |
| `docs/USO-PASSO-A-PASSO.md` | Doc | Artifact | Mover → `docs/archived/` | Referencia estrutura antiga |
| `implementation_plan.md` (raiz) | Avulso | Artifact | Mover → `artifacts/slices/` | Não pertence à raiz |
| `specslice_signup_signin.md` (raiz) | Avulso | Artifact | Mover → `artifacts/slices/` | Não pertence à raiz |
| `tutorial_ciclo_completo.md` (raiz) | Avulso | Artifact | Mover → `docs/GO-LEARNING-NOTES.md` (anexar) ou `docs/archived/` | Material de aprendizado |
| `tutorial_curso_agente.md` (raiz) | Avulso | Artifact | Mover → `docs/archived/` | Tutorial do sistema antigo |
| `.agents/` (Codex skills) | Diretório duplicado | Eliminar | Excluir | Fonte única no `.agent/` |
| `docs/codex-playbooks/` | Diretório duplicado | Eliminar | Excluir | Workflows ficam em `.agent/workflows/` |
| `.codex/` | Config Codex | Eliminar | Excluir | Sem uso ativo |

---

## Novos arquivos a criar

### Novas Skills (7 novas + 7 renomeadas = 14 total)

| Skill | Origem | Descrição |
|---|---|---|
| `pm-slice-scoping` | Renomear `slice-classification` | Scoping, classificação S/E/R e quebra de slices |
| `senior-go-engineer-implementation` | **Nova** | Implementação idiomática em Go com foco didático |
| `security-auditor-review` | **Nova** | Revisão de segurança: auth, IAM, owner scoping, secrets |
| `design-system-contract-review` | **Nova** | Revisão de contratos de API e design system |
| `senior-sre-readiness-review` | **Nova** | Revisão de readiness: observabilidade, custos, runbooks |
| `bdd-scenario-design` | **Nova** | Design de cenários BDD Given/When/Then |
| `tdd-test-design` | Renomear `tdd-go-http` | Estratégia TDD red/green/refactor para Go |
| `firebase-auth-integration` | **Nova** | Integração Firebase Auth: tokens, claims, middleware |
| `firestore-modeling-review` | Renomear `firestore-modeling` | Revisão de modelagem Firestore |
| `docker-firebase-local-dev` | Renomear `firebase-emulators-local` | Setup local Docker + Firebase emulators |
| `python-consolidation-worker` | Renomear `consolidation-artifacts` | Worker Python de consolidação assíncrona |
| `bucket-manifest-versioning` | **Nova** | Versionamento de manifestos e artefatos em bucket |
| `openapi-gap-analysis` | Manter | Gap analysis OpenAPI vs implementação |
| `lovable-contract-alignment` | **Nova** | Alinhamento com contratos Lovable |

### Novos Workflows (3 novos + desmembramento)

| Workflow | Descrição |
|---|---|
| `/audit-security` | Desmembramento do `/audit` — foco em segurança |
| `/audit-sre` | Desmembramento do `/audit` — foco em SRE/ops |
| `/teach-go-on-slice` | Explicação didática de Go no contexto do slice |
| `/artifact-sync` | Verificação de consistência dos artifacts do projeto |

### Novos Artifacts (diretórios + doc)

| Artifact | Descrição |
|---|---|
| `docs/GO-LEARNING-NOTES.md` | Notas de aprendizado de Go |
| `artifacts/slices/` | Slice specs e planos |
| `artifacts/tests/` | Planos de teste |
| `artifacts/adrs/` | ADRs (mover os existentes de `docs/`) |
| `artifacts/reviews/` | Revisões de segurança, SRE, contratos |
| `artifacts/ops/` | Runbooks e artefatos operacionais |

---

## Exclusões / Fusões

| Item | Ação | Justificativa |
|---|---|---|
| `.agents/` (todo o diretório) | Excluir | Duplicação do Codex |
| `.codex/` | Excluir | Config Codex sem uso |
| `docs/codex-playbooks/` | Excluir | Duplicação de workflows |
| `.agent/rules/50-docker-and-emulators.md` | Fundir em `local-dev-first.md` e excluir | Conteúdo complementar |
| `.agent/skills/go-learning-mode/` | Fundir conteúdo na rule + nova skill `senior-go-engineer-implementation` e excluir | Era regra, não skill |
| `docs/dual-system-sync-policy.md` | Mover → `docs/archived/` | Obsoleto |
| `docs/dual-system-mapping.md` | Mover → `docs/archived/` | Obsoleto |
| `docs/PROMPTS-EXEMPLO.md` | Mover → `docs/archived/` | Obsoleto |
| `docs/USO-PASSO-A-PASSO.md` | Mover → `docs/archived/` | Referencia estrutura antiga |
| `/audit` workflow | Substituir por `/audit-security` + `/audit-sre` | Foco |

---

## Reescrita de `AGENTS.md` e `.agent/agents.md`

O `AGENTS.md` (raiz) será reescrito para:
1. Remover personas como cargos
2. Remover política dual-system
3. Referenciar rules em `.agent/rules/`
4. Listar skills com nomes novos
5. Listar workflows com nomes novos
6. Manter-se conciso (referência, não duplicação)

O `.agent/agents.md` será igualmente reescrito com o mesmo conteúdo.

---

## Plano de execução em tasks atômicas

O plano está organizado em tasks numeradas para que um modelo de menor capacidade execute cada uma isoladamente.

### Fase 0: Preparação
- **T01**: Criar diretórios de destino (`docs/archived/`, `artifacts/slices/`, `artifacts/tests/`, `artifacts/adrs/`, `artifacts/reviews/`, `artifacts/ops/`)

### Fase 1: Limpeza de duplicações
- **T02**: Excluir diretório `.agents/` (Codex skills duplicados)
- **T03**: Excluir diretório `docs/codex-playbooks/` (playbooks duplicados)
- **T04**: Excluir diretório `.codex/` (config Codex)
- **T05**: Mover `docs/dual-system-sync-policy.md` → `docs/archived/`
- **T06**: Mover `docs/dual-system-mapping.md` → `docs/archived/`
- **T07**: Mover `docs/PROMPTS-EXEMPLO.md` → `docs/archived/`
- **T08**: Mover `docs/USO-PASSO-A-PASSO.md` → `docs/archived/`

### Fase 2: Organização de arquivos avulsos
- **T09**: Mover `implementation_plan.md` (raiz) → `artifacts/slices/`
- **T10**: Mover `specslice_signup_signin.md` (raiz) → `artifacts/slices/`
- **T11**: Mover `tutorial_ciclo_completo.md` (raiz) → `docs/archived/`
- **T12**: Mover `tutorial_curso_agente.md` (raiz) → `docs/archived/`
- **T13**: Mover `docs/ADR-001-*.md` → `artifacts/adrs/`
- **T14**: Mover `docs/ADR-002-*.md` → `artifacts/adrs/`

### Fase 3: Renomear rules (excluir antigos, criar novos)
- **T15**: Recriar `.agent/rules/backend-greenfield.md` com conteúdo de `20-architecture-study-platform-greenfield.md`
- **T16**: Recriar `.agent/rules/go-learning-first.md` com conteúdo de `05-learning-go-first.md` + extras de `go-learning-mode` SKILL
- **T17**: Recriar `.agent/rules/bdd-first-tdd-mandatory.md` com conteúdo de `10-bdd-tdd-red-green-refactor.md`
- **T18**: Recriar `.agent/rules/local-dev-first.md` com conteúdo de `15-local-dev-first.md` + `50-docker-and-emulators.md`
- **T19**: Recriar `.agent/rules/openapi-as-reference-not-source-of-truth.md` com conteúdo de `30-openapi-as-reference.md`
- **T20**: Recriar `.agent/rules/no-big-bang-slices.md` com conteúdo de `00-project-operating-system.md`
- **T21**: Recriar `.agent/rules/manifest-and-bucket-pattern.md` com conteúdo de `40-consolidation-architecture.md`
- **T22**: Excluir os 8 arquivos antigos de rules (`00-*` a `50-*`)

### Fase 4: Reorganizar skills (excluir antigos, criar novos)
- **T23**: Criar `.agent/skills/pm-slice-scoping/SKILL.md` (baseado em `slice-classification`)
- **T24**: Criar `.agent/skills/senior-go-engineer-implementation/SKILL.md` (novo)
- **T25**: Criar `.agent/skills/security-auditor-review/SKILL.md` (novo)
- **T26**: Criar `.agent/skills/design-system-contract-review/SKILL.md` (novo)
- **T27**: Criar `.agent/skills/senior-sre-readiness-review/SKILL.md` (novo)
- **T28**: Criar `.agent/skills/bdd-scenario-design/SKILL.md` (novo)
- **T29**: Criar `.agent/skills/tdd-test-design/SKILL.md` (baseado em `tdd-go-http`)
- **T30**: Criar `.agent/skills/firebase-auth-integration/SKILL.md` (novo)
- **T31**: Renomear/recriar `.agent/skills/firestore-modeling-review/SKILL.md` (baseado em `firestore-modeling`)
- **T32**: Renomear/recriar `.agent/skills/docker-firebase-local-dev/SKILL.md` (baseado em `firebase-emulators-local`)
- **T33**: Renomear/recriar `.agent/skills/python-consolidation-worker/SKILL.md` (baseado em `consolidation-artifacts`)
- **T34**: Criar `.agent/skills/bucket-manifest-versioning/SKILL.md` (novo)
- **T35**: Manter `.agent/skills/openapi-gap-analysis/SKILL.md` (enriquecer)
- **T36**: Criar `.agent/skills/lovable-contract-alignment/SKILL.md` (novo)
- **T37**: Excluir skills antigos que foram renomeados/substituídos

### Fase 5: Novos workflows
- **T38**: Criar `.agent/workflows/audit-security.md`
- **T39**: Criar `.agent/workflows/audit-sre.md`
- **T40**: Criar `.agent/workflows/teach-go-on-slice.md`
- **T41**: Criar `.agent/workflows/artifact-sync.md`
- **T42**: Excluir `.agent/workflows/audit.md` (substituído pelos dois novos)

### Fase 6: Artifacts docs
- **T43**: Criar `docs/GO-LEARNING-NOTES.md`
- **T44**: Enriquecer `docs/PROJECT-CONTEXT.md` (está raso)
- **T45**: Enriquecer `docs/ARCHITECTURE.md`
- **T46**: Enriquecer `docs/TEST-STRATEGY.md`
- **T47**: Enriquecer `docs/LOCAL-DEV.md`
- **T48**: Enriquecer `docs/CONSOLIDATION-ARCHITECTURE.md`

### Fase 7: Reescrita de AGENTS.md
- **T49**: Reescrever `AGENTS.md` (raiz) sem personas, sem dual-system, com nova taxonomia
- **T50**: Reescrever `.agent/agents.md` alinhado com `AGENTS.md`

### Fase 8: Validação
- **T51**: Verificar que todos os skills têm SKILL.md com frontmatter `name:` e `description:`
- **T52**: Verificar que todos os workflows têm frontmatter `description:`
- **T53**: Verificar que não há arquivos avulsos na raiz do projeto (exceto README.md e AGENTS.md)
- **T54**: Verificar que `.agents/`, `.codex/`, `docs/codex-playbooks/` não existem mais

---

## User Review Required

> [!IMPORTANT]
> **Eliminação do Codex**: o plano propõe excluir `.agents/`, `.codex/`, `docs/codex-playbooks/` e a política dual-system. Se você ainda usa Codex ativamente, preciso saber **antes** de executar.

> [!IMPORTANT]
> **Workflow `/audit` dividido**: o audit genérico será substituído por `/audit-security` e `/audit-sre`. Se preferir manter um `/audit` geral além dos dois específicos, informe.

> [!IMPORTANT]  
> **Workflow `/ship`**: mantido como está. Confirme se deseja mantê-lo ou removê-lo já que não estava na sua lista de workflows esperados.

> [!IMPORTANT]
> **ADRs**: proponho mover `docs/ADR-001-*.md` e `docs/ADR-002-*.md` para `artifacts/adrs/`. Isso quebra links existentes em `docs/ARCHITECTURE.md`. Confirme.

> [!IMPORTANT]
> **Templates**: os templates em `.agent/artifacts/templates/` não estavam na sua lista de preocupações. Proponho mantê-los onde estão. Confirme.

---

## Verificação

### Checklist pós-execução
- [ ] 7 rules em `.agent/rules/` com nomes descritivos
- [ ] 14 skills em `.agent/skills/*/SKILL.md` com frontmatter correto
- [ ] 14 workflows em `.agent/workflows/` com frontmatter correto
- [ ] Nenhum skill com nome de cargo cru
- [ ] Nenhum diretório duplicado (`.agents/`, `.codex/`, `docs/codex-playbooks/`)
- [ ] Nenhum arquivo avulso na raiz (exceto README.md e AGENTS.md)
- [ ] Artifacts organizados em `docs/` e `artifacts/`
- [ ] `AGENTS.md` e `.agent/agents.md` reescritos e alinhados
- [ ] Todos os docs enriquecidos acima de 500 bytes
