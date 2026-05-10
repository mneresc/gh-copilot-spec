# Plano de Execução — Workflow Tier 1.5 (Tarefas Complexas)

> **INSTRUÇÃO**: Este plano está dividido em partes. Uma IA com menor capacidade deve executar cada task isoladamente. NÃO improvise — siga o plano. Cada parte está em arquivo separado.

> **PARTES**: `plan_part1_overview.md` (este) → `plan_part2_chatmodes.md` → `plan_part3_prompts.md` → `plan_part4_tasks_mcp.md` → `plan_part5_contracts.md`

---

## PARTE A — CONTEXTO E FILOSOFIA

### O que é o Tier 1.5
Fluxo multi-fase com isolamento via chat modes e prompt files do Copilot/VSCode para features médias e críticas em microserviços. Usa Plan → RED → GREEN → Contract Validation → Review com BDD embutido.

### Diferença do Tier 1 (enxuto, já existente)
O Tier 1 (em `examples/claude-copilot-workflow.md`) resolve em Plan→Implement→Review num turno. O Tier 1.5 adiciona:
- Isolamento de contexto por fase (chat modes separados)
- RED/GREEN com escritores distintos
- Contract validation como fase dedicada (fase 2c)
- Prompt files consultivos (security, observability)
- Checkpoints de aprovação humana

### Capacidades reais do Copilot/VSCode usadas
| Necessidade | Solução Copilot/VSCode |
|---|---|
| Sub-agent isolado | Chat mode + nova sessão de chat |
| Skill consultiva | Prompt file invocado sob demanda |
| Hook automático | Task do VSCode + regra no copilot-instructions |
| Slash command | Prompt file com argumentos |

---

## PARTE B — MAPEAMENTO DE ASSETS EXISTENTES

### O que já existe e será REUTILIZADO (não duplicar)

| Asset existente | Path no repo | Uso no Tier 1.5 |
|---|---|---|
| `pm-bdd-manager` agent | `general/copilot/agents/pm-bdd-manager.agent.md` | Base para chat mode `pm-planner` |
| `security-auditor` agent | `general/copilot/agents/security-auditor.agent.md` | Base para prompt `security-checklist` |
| `test-auditor` agent | `general/copilot/agents/test-auditor.agent.md` | Base para chat mode `qa-red-writer` |
| `api-contract-reviewer` agent | `general/copilot/agents/api-contract-reviewer.agent.md` | Base para chat mode `contract-validator` |
| `observability-auditor` agent | `general/copilot/agents/observability-auditor.agent.md` | Base para prompt `observability-checklist` |
| `release-readiness-reviewer` agent | `general/copilot/agents/release-readiness-reviewer.agent.md` | Base para chat mode `pr-reviewer` |
| `microservice-reviewer` agent | `general/copilot/agents/microservice-reviewer.agent.md` | Base para chat mode `cross-repo-reviewer` |
| `solution-architect` agent | `general/copilot/agents/solution-architect.agent.md` | Referência no `pm-planner` |
| `bdd-scenario-authoring` skill | `general/copilot/skills/bdd-scenario-authoring/` | Referenciado no `pm-planner` |
| `api-contract-review` skill | `general/copilot/skills/api-contract-review/` | Base para `contract-diff-analyzer` prompt |
| `security-review` skill | `general/copilot/skills/security-review/` | Base para `security-checklist` prompt |
| `observability-review` skill | `general/copilot/skills/observability-review/` | Base para `observability-checklist` prompt |
| `spec-authoring` skill | `general/copilot/skills/spec-authoring/` | Referenciado no `feature-plan` prompt |
| `execution-planning` skill | `general/copilot/skills/execution-planning/` | Referenciado no `feature-plan` prompt |
| `backend-security.instructions` | `general/copilot/instructions/backend-security.instructions.md` | Anexado pelo `security-checklist` |
| `backend-observability.instructions` | `general/copilot/instructions/backend-observability.instructions.md` | Anexado pelo `observability-checklist` |
| Templates PLAN/BDD/ADR | `templates/*.md` | Referenciados nos prompts |
| `copilot-instructions.md` template | `repo-specific/templates/.github/copilot-instructions.md` | Base para novo template expandido |
| `feature-spec-driven` workflow | `workflows/feature-spec-driven.md` | Referência conceitual do fluxo |

### O que NÃO existe e precisa ser CRIADO

Novos artefatos do Copilot Chat que o repo atual não tem:
1. **Chat modes** (`.chatmode.md`) — conceito novo, repo usa `.agent.md`
2. **Prompt files** (`.prompt.md`) — conceito novo, repo usa `SKILL.md`
3. **VSCode tasks** (`.vscode/tasks.json`) — não existe
4. **MCP config** (`.vscode/mcp.json`) — não existe
5. **Orquestradores** (`feature-plan.prompt.md`, `feature-full.prompt.md`)

---

## PARTE C — SEPARAÇÃO GERAL vs REPO-SPECIFIC

### Camada GERAL (vai no repo `instructions/` → instalada na máquina do dev)

```
general/copilot/
├── chatmodes/                          # NOVO — chat modes reutilizáveis
│   ├── pm-planner.chatmode.md
│   ├── qa-red-writer.chatmode.md
│   ├── senior-engineer.chatmode.md
│   ├── contract-validator.chatmode.md
│   ├── pr-reviewer.chatmode.md
│   └── cross-repo-reviewer.chatmode.md
├── prompts/                            # NOVO — prompt files reutilizáveis
│   ├── qa-checklist.prompt.md
│   ├── security-checklist.prompt.md
│   ├── observability-checklist.prompt.md
│   ├── adr-writer.prompt.md
│   ├── contract-diff-analyzer.prompt.md
│   ├── downstream-impact.prompt.md
│   ├── feature-plan.prompt.md
│   └── feature-full.prompt.md
├── agents/                             # JÁ EXISTE — manter como referência
├── skills/                             # JÁ EXISTE — manter como referência
└── instructions/                       # JÁ EXISTE — manter como referência
```

### Camada REPO-SPECIFIC (template copiado para cada microserviço)

```
repo-specific/templates/
├── .github/
│   ├── copilot-instructions.md         # JÁ EXISTE — expandir com seção contratos
│   └── instructions/                   # JÁ EXISTE
├── .vscode/
│   ├── tasks.json                      # NOVO — tasks substitutos de hooks
│   └── mcp.json                        # NOVO — MCP servers config
├── docs/
│   └── contracts/                      # NOVO — onde ficam OpenAPI/Protobuf locais
└── AGENTS.md                           # JÁ EXISTE — expandir
```

### Repo META (cross-service) — NOVO conceito

```
repo-specific/templates-meta/           # NOVO — template para repo meta
├── contracts/                          # Contratos compartilhados entre serviços
│   ├── openapi/
│   ├── protobuf/
│   └── events/
├── adrs/                               # ADRs cross-service
├── runbooks/                           # Runbooks operacionais
└── README.md
```

---

## PARTE D — MATRIZ DE MODELOS POR FASE

| Fase | Chat Mode | Modelo | Justificativa |
|---|---|---|---|
| 1. Plan | `pm-planner` | **Opus** | Decisões de arquitetura e scoping exigem raciocínio profundo |
| 2a. RED | `qa-red-writer` | **Sonnet** | Escrita de testes é mecânica-criativa, não precisa de Opus |
| 2b. GREEN | `senior-engineer` | **Sonnet** | Implementação guiada por testes, equilíbrio custo/qualidade |
| 2c. Contract | `contract-validator` | **Sonnet** | Análise de diff estruturada, regras claras |
| 3. Review | `pr-reviewer` | **Sonnet** (Opus se sec) | Sonnet suficiente para review padrão; Opus se toca auth/PII |
| Cross-repo | `cross-repo-reviewer` | **Opus** | Precisa raciocinar sobre impacto em múltiplos serviços |

### Quando usar Haiku no Tier 1.5
- **Nunca como fase principal** — o Tier 1.5 é para tarefas que justificam cerimônia
- Se a tarefa cabe em Haiku → use o Tier 1 (fluxo enxuto)

---

## PARTE E — SKIP RULES E INTEGRAÇÃO COM TIER 1

### Heurística de decisão (30 segundos)

| Pergunta | Se sim → |
|---|---|
| Toca contrato publicado (OpenAPI, Protobuf, evento)? | **SEMPRE Tier 1.5** com fase 2c obrigatória |
| Toca mais de 2 serviços? | Tier 1.5 com cross-repo-reviewer |
| Mudança irreversível (schema, migração)? | Tier 1.5 |
| Feature com >3 cenários BDD? | Tier 1.5 |
| Área sensível (auth, pagamentos, PII)? | Tier 1.5 (Opus no review) |
| Tudo "não"? | **Tier 1** (fluxo enxuto) |

### Migrar de Tier 1 → 1.5 no meio do trabalho
Se durante o Tier 1 descobrir que mexe em contrato:
1. PARE a implementação
2. Salve o que tem como rascunho
3. Abra nova sessão com `pm-planner`
4. Inclua o rascunho como contexto no plan
5. Siga o fluxo 1.5 completo

---

## PARTE F — CHECKPOINTS DE APROVAÇÃO HUMANA

| Checkpoint | Após fase | O que o humano valida | Como implementar |
|---|---|---|---|
| CP1 | Fase 1 (Plan) | PLAN.md + .feature estão corretos? | Guard no prompt: "PARE e espere meu OK" |
| CP2 | Fase 2a (RED) | Testes cobrem cenários relevantes? | Humano roda testes, confirma que falham |
| CP3 | Fase 2c (Contract) | Breaking change é aceitável? | Veredicto do validator + decisão humana |
| CP4 | Antes do merge | Review completo aprovado? | pr-reviewer gera laudo, humano decide |

Implementação em prompt files: cada prompt de orquestração (`feature-full`) inclui `## ⛔ CHECKPOINT` com instrução explícita de parar.

---

## PARTE G — MANUTENÇÃO E EVOLUÇÃO

### Capturar correções recorrentes
- Se corrigiu o Claude 3+ vezes sobre a mesma coisa → vira regra no `copilot-instructions.md`
- Se a correção é universal (não do repo) → vira instrução em `general/copilot/instructions/`

### Quando criar novo prompt file vs ampliar existente
- **Novo**: se o escopo é diferente (ex: `grpc-contract-analyzer` separado do `contract-diff-analyzer`)
- **Ampliar**: se é variação do mesmo tema (ex: adicionar Avro ao `contract-diff-analyzer`)

### Métricas de saúde
- Número de vezes que o humano corrige output do chat mode → alto = prompt ruim
- Frequência de uso de cada prompt file → 0 em 30 dias = candidato a remoção
- Quantidade de breaking changes detectadas na fase 2c vs encontradas em produção

### Versionamento entre repos
- Chat modes e prompts ficam no repo `instructions/` (centralizado)
- Cada repo de microserviço referencia via path de instalação (`~/.copilot/`)
- Versão = commit do repo `instructions/`

---

## PARTE H — ORDEM DE EXECUÇÃO DAS TASKS

### Fase 0: Preparação
- **T01**: Criar diretório `general/copilot/chatmodes/`
- **T02**: Criar diretório `general/copilot/prompts/`
- **T03**: Criar diretório `repo-specific/templates/.vscode/`
- **T04**: Criar diretório `repo-specific/templates-meta/`

### Fase 1: Chat modes (6 arquivos) → ver `plan_part2_chatmodes.md`
- **T05**: Criar `pm-planner.chatmode.md`
- **T06**: Criar `qa-red-writer.chatmode.md`
- **T07**: Criar `senior-engineer.chatmode.md`
- **T08**: Criar `contract-validator.chatmode.md`
- **T09**: Criar `pr-reviewer.chatmode.md`
- **T10**: Criar `cross-repo-reviewer.chatmode.md`

### Fase 2: Prompt files (8 arquivos) → ver `plan_part3_prompts.md`
- **T11**: Criar `qa-checklist.prompt.md`
- **T12**: Criar `security-checklist.prompt.md`
- **T13**: Criar `observability-checklist.prompt.md`
- **T14**: Criar `adr-writer.prompt.md`
- **T15**: Criar `contract-diff-analyzer.prompt.md`
- **T16**: Criar `downstream-impact.prompt.md`
- **T17**: Criar `feature-plan.prompt.md`
- **T18**: Criar `feature-full.prompt.md`

### Fase 3: Tasks VSCode e MCP → ver `plan_part4_tasks_mcp.md`
- **T19**: Criar `.vscode/tasks.json`
- **T20**: Criar `.vscode/mcp.json`
- **T21**: Expandir `copilot-instructions.md` template

### Fase 4: Breaking changes docs → ver `plan_part5_contracts.md`
- **T22**: Criar `repo-specific/templates-meta/` estrutura
- **T23**: Criar doc de referência sobre contract validation

### Fase 5: Integração
- **T24**: Criar `examples/tier15/` com exemplo de uso do fluxo completo
- **T25**: Atualizar `README.md` principal com seção Tier 1.5
- **T26**: Criar `workflows/feature-tier15-multiphase.md` workflow

### Validação
- **T27**: Verificar que todos os chat modes referenciam agents/skills existentes
- **T28**: Verificar que todos os prompt files referenciam templates existentes
- **T29**: Verificar separação geral vs repo-specific está correta
