# Plano Part 3 — Prompt Files (Tasks T11–T18)

> Cada prompt file deve ser criado como `.prompt.md` em `general/copilot/prompts/`.

---

## T11: `qa-checklist.prompt.md`

```markdown
---
name: QA Checklist
description: "Checklist de qualidade para testes — força edge cases e cenários adversariais."
---

# QA Checklist

## Argumentos esperados
- `{{test_files}}` — arquivos de teste a validar
- `{{feature_files}}` — cenários BDD de referência

## Instrução
Analise os testes em `{{test_files}}` contra os cenários em `{{feature_files}}` e valide:

### Cobertura de cenários
- [ ] Cada cenário Gherkin tem pelo menos 1 teste correspondente?
- [ ] O teste referencia o cenário via comentário `// Scenario: <nome>`?

### Edge cases obrigatórios
- [ ] Valores nulos/undefined/vazios para cada input
- [ ] Valores no limite (0, -1, MAX_INT, string vazia, string com 10k chars)
- [ ] Inputs maliciosos (SQL injection, XSS, path traversal) — quando aplicável
- [ ] Concorrência (double-submit, race condition) — quando aplicável
- [ ] Timeout/falha de dependência externa

### Qualidade dos asserts
- [ ] Nenhum `expect(true).toBe(true)` ou assert vazio
- [ ] Testa o retorno E os side-effects (ex: chamou o repository?)
- [ ] Testa mensagens de erro específicas, não genéricas
- [ ] Mock setup é realista (não mascara falhas reais)

### Adversarial thinking
- [ ] Se eu removesse a implementação, TODOS os testes falhariam?
- [ ] Se eu mudasse um IF, pelo menos 1 teste quebraria?
- [ ] Os mocks não estão "ajudando demais" o código a passar?

## Output esperado
Lista de gaps encontrados com sugestão de testes adicionais.

## Quando NÃO usar
- Em testes de UI/CSS
- Em testes de infraestrutura (Terraform, Docker)

## Referência
- Skill base: `general/copilot/skills/unit-test-ts/SKILL.md`
- Agent: `general/copilot/agents/test-auditor.agent.md`
```

---

## T12: `security-checklist.prompt.md`

```markdown
---
name: Security Checklist
description: "Checklist de segurança invocado pelo senior-engineer quando código toca dados sensíveis."
---

# Security Checklist

## Argumentos esperados
- `{{changed_files}}` — arquivos modificados a revisar

## Instrução
Analise `{{changed_files}}` aplicando as regras de `general/copilot/instructions/backend-security.instructions.md`:

### Authn/Authz
- [ ] Todo endpoint valida token/sessão antes de executar?
- [ ] Owner scoping: WHERE inclui tenant/user_id do token?
- [ ] Não há IDOR (Insecure Direct Object Reference)?

### Dados sensíveis
- [ ] PII não aparece em logs (nem via JSON.stringify genérico)?
- [ ] Senhas/tokens não são armazenados em plaintext?
- [ ] Masking aplicado antes de enviar a mensageria/logs?

### Inputs
- [ ] Todos os inputs validados na borda (DTO/schema validation)?
- [ ] Sem concatenação de strings em queries (SQL injection)?
- [ ] Rate limiting considerado para endpoints públicos?

### Segredos
- [ ] Credenciais vêm de cofre (Vault/Secrets Manager), não de env solto?
- [ ] Nenhum segredo hardcoded (nem em testes)?

### Idempotência
- [ ] Endpoints de mutação suportam retry sem duplicação?

## Output esperado
Lista de findings: [P0-BLOQUEIA], [P1-CORRIGIR], [P2-MELHORAR].

## Quando NÃO usar
- Mudanças que não tocam auth, dados de usuário, pagamentos, ou endpoints
- CSS, docs, configs de CI

## Referência
- Skill: `general/copilot/skills/security-review/SKILL.md`
- Instructions: `general/copilot/instructions/backend-security.instructions.md`
- Agent: `general/copilot/agents/security-auditor.agent.md`
```

---

## T13: `observability-checklist.prompt.md`

```markdown
---
name: Observability Checklist
description: "Checklist de observabilidade invocado pelo senior-engineer quando código toca I/O ou eventos."
---

# Observability Checklist

## Argumentos esperados
- `{{changed_files}}` — arquivos modificados

## Instrução
Analise `{{changed_files}}` aplicando `general/copilot/instructions/backend-observability.instructions.md`:

### Logging
- [ ] Logs estruturados (JSON) com nível, timestamp, metadata?
- [ ] Sem `console.log` / `fmt.Println` — usa logger do projeto?
- [ ] Sem PII nos logs (ver copilot-instructions.md para masking)?
- [ ] Catch blocks logam stacktrace sem expor ao usuário?

### Tracing
- [ ] Trace ID propagado em chamadas HTTP/gRPC?
- [ ] Trace ID viaja no header/payload de filas assíncronas?
- [ ] Spans criados manualmente em workers/jobs?

### Métricas
- [ ] Métricas de volume/latência/erro nos pontos críticos?
- [ ] Sem IDs de usuário como labels (cardinalidade explode)?
- [ ] Nenhum dado PII em labels de métricas?

## Output esperado
Lista de gaps com severidade e sugestão de correção.

## Quando NÃO usar
- Código puramente lógico sem I/O
- Testes unitários
- Configs/docs

## Referência
- Skill: `general/copilot/skills/observability-review/SKILL.md`
- Instructions: `general/copilot/instructions/backend-observability.instructions.md`
```

---

## T14: `adr-writer.prompt.md`

```markdown
---
name: ADR Writer
description: "Gera Architecture Decision Record usando template padrão."
---

# ADR Writer

## Argumentos esperados
- `{{decision_title}}` — título da decisão
- `{{context}}` — contexto do problema
- `{{options}}` — opções consideradas (opcional)

## Instrução
Gere um ADR seguindo estritamente o template `templates/ADR.md` com:

1. **Status**: Proposto (sempre inicia assim)
2. **Contexto**: Expanda `{{context}}` com detalhes técnicos
3. **Decisão**: Escolha fundamentada entre as opções
4. **Consequências Positivas**: Benefícios concretos
5. **Consequências Negativas**: Custos, complexidades, riscos

### Regras
- Seja específico ao projeto (use nomes reais de serviços, tabelas, filas)
- NÃO escreva ADR genérico — deve refletir a realidade do repo
- Numere sequencialmente (verifique último ADR existente em `docs/decisions/`)
- Máximo 1 página (ADRs longos não são lidos)

## Output esperado
Arquivo `docs/decisions/ADR-NNN-{{decision_title}}.md`

## Quando NÃO usar
- Decisões triviais que não afetam arquitetura
- Escolhas já documentadas em outro ADR

## Referência
- Template: `templates/ADR.md`
```

---

## T15: `contract-diff-analyzer.prompt.md`

```markdown
---
name: Contract Diff Analyzer
description: "Analisa diff de contratos (OpenAPI/Protobuf/eventos) classificando breaking changes."
---

# Contract Diff Analyzer

## Argumentos esperados
- `{{contract_files}}` — arquivos de contrato modificados
- `{{base_branch}}` — branch base para comparação (default: main)

## Instrução
Compare `{{contract_files}}` entre a branch atual e `{{base_branch}}`. Para cada mudança:

### Detecção por tipo de contrato

**OpenAPI/Swagger:**
- Campos adicionados/removidos em request/response
- Tipos alterados (string→integer, etc.)
- Required adicionado/removido
- Enum values adicionados/removidos
- Status codes alterados
- Path/query params modificados
- Headers alterados

**Protobuf/gRPC:**
- Field numbers alterados (SEMPRE breaking)
- Tipos mudados
- Oneof modificado
- Campos deprecated efetivamente removidos
- Services/RPCs removidos

**Eventos assíncronos (Kafka/RabbitMQ/SNS):**
- Schema Avro/JSON Schema alterado
- Headers de mensagem modificados
- Semântica do evento mudada (mesmo nome, comportamento diferente)
- Topic/queue renomeado

### Classificação

Para CADA mudança, classifique:

| Mudança | Classificação |
|---|---|
| Campo opcional adicionado em response | SAFE |
| Enum value adicionado em produtor | SAFE |
| Novo endpoint/RPC | SAFE |
| Campo deprecated mantido | SAFE |
| Novo campo required COM default | NEEDS-COORDINATION |
| Mudança de default value | NEEDS-COORDINATION |
| Novo header obrigatório | NEEDS-COORDINATION |
| Campo removido de response | BREAKING |
| Tipo alterado | BREAKING |
| Enum value removido (consumidor depende) | BREAKING |
| Field number reutilizado (Protobuf) | BREAKING |
| Semântica mudada sem rename | BREAKING |

## Output esperado
Tabela com: mudança | tipo contrato | classificação | impacto estimado.

## Quando NÃO usar
- Mudanças puramente internas (sem contratos expostos)
- Código que não altera schemas/DTOs/tipos públicos

## Referência
- Skill: `general/copilot/skills/api-contract-review/SKILL.md`
```

---

## T16: `downstream-impact.prompt.md`

```markdown
---
name: Downstream Impact
description: "Mapeia consumidores afetados por mudança em contrato."
---

# Downstream Impact

## Argumentos esperados
- `{{contract_name}}` — nome do contrato alterado
- `{{change_type}}` — BREAKING ou NEEDS-COORDINATION

## Instrução
Descubra quem consome `{{contract_name}}` usando estas estratégias (em ordem):

### 1. Busca no workspace multi-root
Se há múltiplos repos abertos, busque imports/referências ao contrato:
- Imports de tipos/DTOs compartilhados
- URLs de endpoints afetados em clients/SDKs
- Nomes de topics/queues em consumers

### 2. Busca em docs/contracts (repo meta)
Se existe um repo meta com `contracts/`, verifique:
- Quais serviços declaram dependência desse contrato
- Versão que cada consumidor espera

### 3. Análise de código
Busque no codebase por:
- `fetch`/`axios`/`http.Get` apontando para o endpoint afetado
- Consumer groups de Kafka que subscrevem ao topic
- Imports de libs internas que expõem o tipo alterado

### 4. Inferência por naming convention
Se não encontrar referência direta:
- Liste serviços que PROVAVELMENTE consomem (baseado em domínio)
- Marque como "inferido — confirmar manualmente"

## Output esperado
- Lista de consumidores confirmados com path do código
- Lista de consumidores inferidos (marcar como incertos)
- Para cada consumidor: impacto estimado e ação necessária

## Quando NÃO usar
- Mudanças classificadas como SAFE
- Contratos internos de um único serviço

## Referência
- Agent: `general/copilot/agents/microservice-reviewer.agent.md`
```

---

## T17: `feature-plan.prompt.md`

```markdown
---
name: Feature Plan
description: "Orquestra a Fase 1 (planejamento) do fluxo Tier 1.5."
---

# Feature Plan (Orquestrador Fase 1)

## Argumentos esperados
- `{{feature_description}}` — descrição da feature
- `{{repos_affected}}` — repositórios afetados (1 ou mais)

## Instrução

### Passo 1: Intake
Analise `{{feature_description}}` e extraia:
- Quem é o ator principal
- Qual o comportamento esperado (happy path)
- Quais os limites (fora de escopo)
- Quais contratos serão impactados

### Passo 2: Slice scoping
Usando princípios de `general/copilot/skills/slice-scoping/SKILL.md`:
- Classifique: é S (simples), E (estrutural), ou R (refactor)?
- Se E ou R e toca contrato → Tier 1.5 obrigatório
- Se S e sem contrato → sugira Tier 1 (fluxo enxuto)

### Passo 3: BDD
Gere cenários Gherkin para cada comportamento usando `general/copilot/skills/bdd-scenario-authoring/SKILL.md`.

### Passo 4: PLAN.md
Gere plano fatiado usando template `templates/PLAN.md` com milestones e stop-and-fix rules.

### Passo 5: Contratos impactados
Liste explicitamente:
- Quais contratos mudam
- Tipo de mudança esperada (campo novo, tipo alterado, etc.)
- Isso alimenta a fase 2c

## ⛔ CHECKPOINT CP1
Pare aqui. Apresente PLAN.md + .feature + lista de contratos. Aguarde OK do humano.

## Output esperado
- `docs/PLAN.md`
- `docs/features/*.feature`
- `docs/decisions/ADR-NNN.md` (se aplicável)
- Lista de contratos impactados (texto no chat)

## Quando NÃO usar
- Tarefas triviais (use Tier 1)
- Bug fixes simples (use template bugfix do fluxo enxuto)
```

---

## T18: `feature-full.prompt.md`

```markdown
---
name: Feature Full
description: "Orquestra o fluxo completo Tier 1.5 com checkpoints entre fases."
---

# Feature Full (Orquestrador Completo)

## Argumentos esperados
- `{{feature_description}}` — descrição da feature

## Instrução

Este prompt guia o fluxo completo. CADA FASE deve ser executada em SESSÃO DE CHAT SEPARADA com o chat mode indicado.

### Fase 1: PLAN
- **Chat mode**: `pm-planner` (Opus)
- **Ação**: Invoke `/feature-plan` com a descrição da feature
- **Output**: PLAN.md + .feature + lista de contratos
- **⛔ CP1**: Humano aprova plan antes de continuar

### Fase 2a: RED
- **Chat mode**: `qa-red-writer` (Sonnet) — NOVA SESSÃO
- **Input manual**: Anexe os .feature + assinaturas públicas
- **Ação**: Escreva testes que falham. Invoke `/qa-checklist`
- **Output**: Testes falhando
- **⛔ CP2**: Humano confirma que testes falham por falta de impl

### Fase 2b: GREEN
- **Chat mode**: `senior-engineer` (Sonnet) — NOVA SESSÃO
- **Input manual**: Anexe testes RED + assinaturas públicas
- **Ação**: Implemente código mínimo. Invoke `/security-checklist` e `/observability-checklist` quando aplicável
- **Output**: Testes passando

### Fase 2c: CONTRACT VALIDATION (se contratos impactados)
- **Chat mode**: `contract-validator` (Sonnet) — NOVA SESSÃO
- **Ação**: Invoke `/contract-diff-analyzer` + `/downstream-impact`
- **Output**: Veredicto (SAFE/BREAKING/NEEDS-COORDINATION)
- **⛔ CP3**: Se BREAKING, humano decide: migrar, refatorar, ou abortar

### Fase 3: REVIEW
- **Chat mode**: `pr-reviewer` (Sonnet, Opus se sec-crítico) — NOVA SESSÃO
- **Input**: Todo o código + artefatos + veredicto da fase 2c
- **Output**: Laudo (APPROVED/CAVEATS/CHANGES REQUESTED)
- **⛔ CP4**: Humano decide merge

### Cross-repo (se >1 repositório)
- **Chat mode**: `cross-repo-reviewer` (Opus)
- **Input**: Workspace multi-root com todos os repos
- **Output**: Ordem de deploy + riscos de rollback

## Quando NÃO usar
- Tarefas do Tier 1 (ver heurística de skip rules)
- Mudanças puramente internas sem contrato
```
