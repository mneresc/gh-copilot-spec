# AI-Assisted Engineering Operating System — Implementation Plan

## Goal

Create a complete repository at `d:\marcelo\Documents\0_projetos\01_desenvolvimento\instructions` that serves as an **engineering operating system for AI-assisted development**. It is NOT a product, library, or application framework — it is a structured collection of documents, agents, skills, workflows, and templates that guide AI copilots and engineering teams through specification-driven development.

## Architectural Decisions

| Decision | Rationale |
|----------|-----------|
| **Two-layer architecture** (general vs repo-specific) | General layer installs into user profile (`~/.copilot/`) and is reusable across all repos. Specific layer lives inside each repo and captures local truth. |
| **Spec-driven development** | All implementation is guided by canonical artifacts (FEATURE_SPEC, BDD, TEST_PLAN, PLAN, STATUS, AUDIT), not chat-driven. |
| **No product code** | The repository only produces documents, agents, skills, workflows, templates, adapters, and specification artifacts. |
| **Vehicle rental domain** as illustrative example | Concrete, relatable domain for all examples — avoids crypto/blockchain. |
| **Windows-first installation paths** | User is on Windows; docs use `C:\Users\<USER>\.copilot\...` paths. |

## Proposed Changes

### Phase A — Scaffold

Create the full directory tree. No content yet — just structure.

```
instructions/
├── README.md
├── general/
│   └── copilot/
│       ├── instructions/
│       ├── skills/
│       │   ├── story-intake/
│       │   ├── slice-scoping/
│       │   ├── spec-authoring/
│       │   ├── bdd-scenario-authoring/
│       │   ├── test-plan-authoring/
│       │   ├── execution-planning/
│       │   ├── execution-status-update/
│       │   ├── ts-service-implementation/
│       │   ├── python-worker-implementation/
│       │   ├── unit-test-ts/
│       │   ├── unit-test-python/
│       │   ├── security-review/
│       │   ├── threat-modeling/
│       │   ├── aws-terraform-review/
│       │   ├── microservice-review/
│       │   ├── api-contract-review/
│       │   ├── observability-review/
│       │   ├── incident-hotfix-review/
│       │   └── spec-audit/
│       └── agents/
├── repo-specific/
│   └── templates/
│       ├── .github/
│       │   └── instructions/
│       └── docs/
├── templates/
├── workflows/
└── docs/
```

---

### Phase B — Fundamental Docs (7 files)

#### [NEW] [README.md](file:///d:/marcelo/Documents/0_projetos/01_desenvolvimento/instructions/README.md)
- Complete, robust README per the specification (sections 6-7 of the user spec)
- Includes: purpose, non-purpose, structure, general vs specific, installation (Windows), daily use, spec-driven flow, step-by-step diagram, "when to use what" section, recommended flows, minimal adoption, vehicle rental examples

#### [NEW] [docs/USAGE.md](file:///d:/marcelo/Documents/0_projetos/01_desenvolvimento/instructions/docs/USAGE.md)
#### [NEW] [docs/WORKFLOW.md](file:///d:/marcelo/Documents/0_projetos/01_desenvolvimento/instructions/docs/WORKFLOW.md)
#### [NEW] [docs/DECISIONS.md](file:///d:/marcelo/Documents/0_projetos/01_desenvolvimento/instructions/docs/DECISIONS.md)
#### [NEW] [docs/ADOPTION-CHECKLIST.md](file:///d:/marcelo/Documents/0_projetos/01_desenvolvimento/instructions/docs/ADOPTION-CHECKLIST.md)
#### [NEW] [docs/GENERAL-VS-REPO-SPECIFIC.md](file:///d:/marcelo/Documents/0_projetos/01_desenvolvimento/instructions/docs/GENERAL-VS-REPO-SPECIFIC.md)
#### [NEW] [docs/SPEC-DRIVEN-DEVELOPMENT.md](file:///d:/marcelo/Documents/0_projetos/01_desenvolvimento/instructions/docs/SPEC-DRIVEN-DEVELOPMENT.md)
#### [NEW] [docs/REVIEW.md](file:///d:/marcelo/Documents/0_projetos/01_desenvolvimento/instructions/docs/REVIEW.md)

---

### Phase C — General Layer (37 files)

#### Instructions (6 files)

| File | Focus |
|------|-------|
| `backend-security.instructions.md` | Least privilege, input validation, secret handling, trust boundaries |
| `backend-observability.instructions.md` | Structured logging, tracing, metrics, PII avoidance, correlation |
| `typescript-service.instructions.md` | TS service conventions, error handling, typing discipline |
| `python-worker.instructions.md` | Python worker conventions, async patterns, error handling |
| `terraform-review.instructions.md` | State management, IAM, encryption, blast radius, drift |
| `microservice-boundaries.instructions.md` | Service boundaries, contracts, data ownership |

#### Skills (18 files)

Each `SKILL.md` follows the canonical format: Purpose, When to use, When not to use, Expected inputs, Operating steps, Quality bar, Expected outputs, Common failure modes, Minimal checklist, Stack-specific notes.

Full list: `story-intake`, `slice-scoping`, `spec-authoring`, `bdd-scenario-authoring`, `test-plan-authoring`, `execution-planning`, `execution-status-update`, `ts-service-implementation`, `python-worker-implementation`, `unit-test-ts`, `unit-test-python`, `security-review`, `threat-modeling`, `aws-terraform-review`, `microservice-review`, `api-contract-review`, `observability-review`, `incident-hotfix-review`, `spec-audit`

> [!IMPORTANT]
> - `unit-test-ts` considers Jest or Vitest — does not impose a single framework
> - `unit-test-python` considers pytest/unittest — does not impose a single pattern
> - `security-review` covers authn/authz, secrets, sensitive logs, trust boundaries, replay/idempotency, CI/CD, dependencies, insecure defaults
> - `aws-terraform-review` covers state, IAM, outputs, KMS, encryption, drift, blast radius, network exposure, observability, rollback
> - `observability-review` covers logs, metrics, traces, cardinality, operational utility, diagnostic capability
> - `spec-audit` compares implementation against explicit artifacts

#### Agents (11 files)

Each agent has: Mission, When to use, When not to use, Scope, Inputs, Outputs, Interaction model, Constraints, Review posture.

Full list: `pm-bdd-manager`, `solution-architect`, `pair-engineer-ts`, `pair-engineer-python`, `security-auditor`, `test-auditor`, `aws-terraform-auditor`, `microservice-reviewer`, `api-contract-reviewer`, `observability-auditor`, `release-readiness-reviewer`

---

### Phase D — Repo-Specific Layer (13 files)

#### Templates for repository setup (6 files)
- `repo-specific/templates/AGENTS.md`
- `repo-specific/templates/.github/copilot-instructions.md`
- `repo-specific/templates/.github/instructions/typescript.instructions.md`
- `repo-specific/templates/.github/instructions/python.instructions.md`
- `repo-specific/templates/.github/instructions/terraform.instructions.md`
- `repo-specific/templates/.github/instructions/observability.instructions.md`

#### Canonical feature artifact templates (7 files)
- `templates/FEATURE_SPEC.md`
- `templates/BDD.md`
- `templates/TEST_PLAN.md`
- `templates/PLAN.md`
- `templates/STATUS.md`
- `templates/AUDIT.md`
- `templates/ADR.md`

---

### Phase E — Workflows (11 files)

| Workflow | Purpose |
|----------|---------|
| `story-to-bdd.md` | Story → intake → slice → BDD |
| `bdd-to-testplan.md` | BDD scenarios → test plan |
| `pair-implement.md` | Guided implementation by slice |
| `unit-test-create.md` | Unit test generation and validation |
| `security-audit.md` | Security review and audit |
| `test-audit.md` | Test coverage and quality audit |
| `terraform-audit.md` | Terraform infrastructure audit |
| `microservice-audit.md` | Microservice architecture audit |
| `pre-merge-readiness.md` | Pre-merge gate validation |
| `incident-hotfix-audit.md` | Incident/hotfix review |
| **`feature-spec-driven.md`** | **Critical**: Full spec-driven feature workflow |

---

### Phase F — Final Review

Write `docs/REVIEW.md` with a self-assessment of the repository.

## File Count Summary

| Category | Count |
|----------|-------|
| README | 1 |
| Docs | 7 |
| General instructions | 6 |
| General skills | 18 (19 SKILL.md files) |
| General agents | 11 |
| Repo-specific templates | 6 |
| Feature artifact templates | 7 |
| Workflows | 11 |
| **Total** | **~67 files** |

## Verification Plan

### Automated
- Verify all files exist via `tree` command
- Verify no product/business code is generated
- Verify general layer contains no repo-specific assumptions

### Manual
- User reviews README for completeness
- User validates that the vehicle rental examples are clear
- User validates installation instructions for Windows

## Open Questions

> [!IMPORTANT]
> 1. **Copilot flavor**: The templates reference `.github/copilot-instructions.md` — this assumes GitHub Copilot. Should I also include structure for other AI assistants (e.g., Cursor, Cline, Windsurf)?
> 2. **Language**: The spec is in Portuguese. Should all file content be written in **Portuguese** or **English**? The file names in the spec are in English. I'll default to **English for all content** unless you prefer Portuguese.
> 3. **Skill count**: The spec lists 18 skills but `spec-audit` makes 19. I'll create all 19.

