# Plano Part 4 — Tasks VSCode, MCP e copilot-instructions (T19–T21)

---

## T19: `.vscode/tasks.json`

Criar em `repo-specific/templates/.vscode/tasks.json`:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "test:filtered",
      "type": "shell",
      "command": "npm run test -- --reporter=min 2>&1 | head -50",
      "detail": "Roda testes com output filtrado (substitui hook de auto-run)",
      "group": "test",
      "presentation": {
        "reveal": "always",
        "panel": "shared",
        "clear": true
      },
      "problemMatcher": []
    },
    {
      "label": "contract:validate",
      "type": "shell",
      "command": "git diff main -- '*.openapi.yaml' '*.proto' '*.avsc' 'src/**/dto/**' | head -200",
      "detail": "Mostra diff de contratos vs main (pre-commit check). Alimente o output ao contract-validator.",
      "group": "build",
      "presentation": {
        "reveal": "always",
        "panel": "dedicated"
      },
      "problemMatcher": []
    },
    {
      "label": "lint:typecheck",
      "type": "shell",
      "command": "npm run lint && npm run typecheck",
      "detail": "Roda linter + typecheck (substitui hook post-edit)",
      "group": "build",
      "presentation": {
        "reveal": "silent",
        "panel": "shared"
      },
      "problemMatcher": ["$tsc"]
    },
    {
      "label": "adr:check",
      "type": "shell",
      "command": "powershell -Command \"$changes = git diff --name-only main -- '*.openapi.yaml' '*.proto' '*.avsc'; if ($changes) { Write-Host 'CONTRATOS MODIFICADOS - Verifique se há ADR correspondente em docs/decisions/'; git diff --stat main -- '*.openapi.yaml' '*.proto' '*.avsc' } else { Write-Host 'Nenhum contrato modificado.' }\"",
      "detail": "Verifica se mudanças em contratos têm ADR correspondente",
      "group": "build",
      "presentation": {
        "reveal": "always",
        "panel": "shared"
      },
      "problemMatcher": []
    }
  ]
}
```

**Nota para Go (adaptar por repo)**: substituir `npm run test` por `go test ./... -count=1 -short` e `npm run lint` por `golangci-lint run`.

---

## T20: `.vscode/mcp.json`

Criar em `repo-specific/templates/.vscode/mcp.json`:

```json
{
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${env:GITHUB_TOKEN}"
      },
      "metadata": {
        "phases": ["review", "cross-repo"],
        "purpose": "PRs, issues, contexto cross-repo",
        "tokenRisk": "MEDIUM — limitar a repos específicos via GITHUB_TOKEN scope",
        "mitigation": "Usar fine-grained PAT com acesso apenas aos repos do workspace"
      }
    }
  }
}
```

**Nota**: Os servers abaixo são opcionais e dependem da infra do time. Adicione conforme necessário:

```json
{
  "servers": {
    "jira": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-atlassian"],
      "env": {
        "ATLASSIAN_API_TOKEN": "${env:JIRA_TOKEN}",
        "ATLASSIAN_SITE": "${env:JIRA_SITE}"
      },
      "metadata": {
        "phases": ["plan"],
        "purpose": "Leitura de tickets para intake",
        "tokenRisk": "LOW — read-only",
        "mitigation": "Token com permissão read-only em projetos específicos"
      }
    }
  }
}
```

**Servers NÃO recomendados inicialmente** (alto risco de token bloat):
- Schema Registry (Confluent) — output muito verboso, prefira copiar schema localmente
- Datadog/Grafana — output de dashboards é enorme, use links diretos no prompt

---

## T21: Expandir `copilot-instructions.md` template

Editar `repo-specific/templates/.github/copilot-instructions.md` para adicionar seção de contratos. O conteúdo final deve ser (máximo ~80 linhas, seções existentes preservadas):

```markdown
# Instruções Locais (Repositório)

Este arquivo é a "Verdade Local". A camada geral provê comportamentos e agentes; este arquivo dita os detalhes estritos deste repositório.

## 1. Comandos Reais
- Build: `<comando>`
- Linter: `<comando>`
- Typecheck: `<comando>`
- Testes Unitários: `<comando>`
- Testes de Integração: `<comando>`

## 2. Organização de Código
- Camada de Domínio: `<path>`
- Camada de Infra: `<path>`
- Camada de API: `<path>`
- DTOs/Contracts: `<path>`

## 3. Logger Real
<instrução sobre logger do projeto — nunca console.log>

## 4. Métricas e Traces
<instrução sobre OpenTelemetry/Prometheus do projeto>

## 5. Limites e Áreas Sensíveis
<listar entidades com PII e regras de masking>

## 6. Runbooks / Infra Local
<como subir dependências localmente>

## 7. Contratos Publicados (NOVO — crítico para Tier 1.5)
### APIs
- OpenAPI spec: `<path para openapi.yaml>`
- Versionamento: `<v1/v2 ou header-based>`
- Consumidores conhecidos: `<listar serviços>`

### Eventos
- Produtor de: `<listar topics/queues>`
- Consumidor de: `<listar topics/queues>`
- Schema registry: `<URL ou path local>`

### DTOs compartilhados
- Lib interna: `<nome do pacote e path>`
- Consumidores: `<listar serviços>`

### Regra para mudanças em contratos
- SEMPRE rodar task `contract:validate` antes de commit
- SEMPRE usar chat mode `contract-validator` se o diff mostrar mudanças
- Se BREAKING → obrigatório ADR + plano de migração antes do merge

## 8. Regras Recorrentes (capturadas do uso)
<adicionar aqui regras que surgiram de correções repetidas>
```

---

# Plano Part 5 — Contract Validation Deep-Dive (T22–T23)

---

## T22: Estrutura do repo meta

Criar em `repo-specific/templates-meta/`:

```
templates-meta/
├── README.md
├── contracts/
│   ├── openapi/
│   │   └── .gitkeep
│   ├── protobuf/
│   │   └── .gitkeep
│   └── events/
│       └── .gitkeep
├── adrs/
│   └── .gitkeep
└── runbooks/
    └── .gitkeep
```

**README.md do repo meta:**

```markdown
# Repo Meta — Cross-Service Artifacts

Este repositório centraliza artefatos compartilhados entre microserviços.

## Estrutura

### contracts/
Contratos publicados por cada serviço:
- `openapi/` — specs OpenAPI/Swagger por serviço
- `protobuf/` — definições .proto compartilhadas
- `events/` — schemas de eventos (Avro, JSON Schema)

### adrs/
ADRs que afetam múltiplos serviços (ex: mudança de protocolo de comunicação).

### runbooks/
Procedimentos operacionais cross-service.

## Como usar
1. Cada serviço publica seus contratos aqui via CI
2. O `contract-validator` compara contra este repo
3. O `cross-repo-reviewer` usa este repo como fonte de verdade
```

---

## T23: Documento de referência — Contract Validation

Criar em `docs/contract-validation-reference.md` (no repo `instructions/`):

```markdown
# Contract Validation — Referência Completa

## 1. Detecção por tipo de contrato

### OpenAPI/Swagger
| O que verificar | Como detectar | Exemplo |
|---|---|---|
| Campo removido em response | diff do schema | `name` sumiu do UserResponse |
| Tipo alterado | diff de `type:` | `age: string` → `age: integer` |
| Required adicionado | diff de `required:` | `email` virou required |
| Enum value removido | diff de `enum:` | `PENDING` removido de StatusEnum |
| Status code alterado | diff de `responses:` | 200 → 201 em POST |
| Path param renomeado | diff de `paths:` | `/users/{id}` → `/users/{userId}` |

### Protobuf/gRPC
| O que verificar | Risco | Exemplo |
|---|---|---|
| Field number alterado | SEMPRE BREAKING | `string name = 1` → `string name = 2` |
| Tipo mudado | BREAKING | `int32` → `string` |
| Campo em oneof movido | BREAKING | campo sai do oneof |
| Campo deprecated removido | BREAKING se consumidor usa | `reserved 3;` sem o campo |
| Novo campo em message | SAFE (se optional) | adicionar `string nickname = 5` |

### Eventos assíncronos
| O que verificar | Risco | Exemplo |
|---|---|---|
| Schema Avro incompatível | Depende da compat mode | Campo removed em BACKWARD mode |
| Header de mensagem removido | BREAKING | `X-Correlation-Id` sumiu |
| Semântica mudada | SUTIL e PERIGOSO | `OrderCreated` agora significa "order submitted" |
| Topic renomeado | BREAKING | `orders.created` → `orders.submitted` |

## 2. Mapeamento de consumidores

### Estratégias (em ordem de confiabilidade)
1. **Repo meta**: `contracts/` tem registro de quem publica e consome
2. **Imports no código**: buscar referências ao tipo/path modificado em todos os repos
3. **Service registry**: consultar registros de serviço (se disponível via MCP)
4. **Tracing**: consultar Datadog/Jaeger para ver quem chama o endpoint (se MCP disponível)
5. **Inferência**: deduzir por domínio (menos confiável — marcar como "inferido")

### Template de mapeamento
```text
Contrato: <nome>
Tipo: OpenAPI / Protobuf / Evento
Mudança: <descrição>
Classificação: SAFE / NEEDS-COORDINATION / BREAKING

Consumidores confirmados:
- Serviço A (path: src/clients/user-client.ts, linha 42)
- Serviço B (path: src/consumers/order-consumer.go, linha 78)

Consumidores inferidos (confirmar):
- Serviço C (provavelmente consome via API gateway)
```

## 3. Plano de migração (quando BREAKING)

### Template
```text
## Plano de Migração — <nome do contrato>

### Mudança
<descrição exata>

### Classificação
BREAKING

### Consumidores impactados
<lista>

### Estratégia
[ ] Versionamento (v1/v2 paralelo por N dias)
[ ] Deprecation gradual (manter campo antigo + novo)
[ ] Coordinated deploy (todos os serviços ao mesmo tempo)

### Timeline
- Dia 0: Deploy do produtor com backward compat
- Dia 1-N: Consumidores migram para novo contrato
- Dia N+1: Remover contrato antigo

### Comunicação
- [ ] Notificar times dos serviços consumidores
- [ ] ADR documentando a decisão
- [ ] PR coordenado (ou stacked PRs)

### Rollback plan
<o que fazer se der errado>
```

## 4. Edge cases comuns

### Evento já publicado em produção (mensagens em fila)
- Se mudar schema de evento com mensagens já na fila → consumidores vão crashar
- **Mitigação**: consumer deve tolerar schema antigo por N horas/dias
- Schema Registry com BACKWARD ou FULL compatibility mode ajuda

### Schema evolution em Kafka com Schema Registry
- BACKWARD: consumer novo lê mensagens antigas ✅
- FORWARD: consumer antigo lê mensagens novas ✅
- FULL: ambos ✅ (recomendado)
- NONE: sem garantia ❌ (evitar)

### Versionamento de API (v1/v2 paralelo)
- **Quando usar**: mudança estrutural grande, muitos consumidores
- **Quando NÃO usar**: adição de campo opcional (não justifica v2)
- **Regra**: v1 fica ativo por mínimo de 30 dias após v2 estável

### Mudança de comportamento sem mudança de schema
- O mais sutil e perigoso
- Exemplo: endpoint retorna 200 mas agora faz validação diferente internamente
- **Detecção**: difícil automaticamente — depende de review humano
- **Mitigação**: documentar em ADR, notificar consumidores explicitamente

### Quebras em headers HTTP, query params, path params
- Headers removidos/renomeados → BREAKING
- Query param obrigatório adicionado → BREAKING
- Path param renomeado → BREAKING (muda URL)
- **Mitigação**: manter aliases temporários

## 5. Workflow multi-repo

### Workspace multi-root vs janelas separadas
- **Recomendado**: workspace multi-root quando feature toca 2-3 repos
- **Janelas separadas**: quando repos são independentes ou >3
- Configurar: File → Add Folder to Workspace

### contract-validator vendo todos os repos
- No workspace multi-root, o chat mode tem acesso a todos os repos
- Anexar manualmente os contratos de cada repo na sessão
- Ou usar repo meta como fonte centralizada

### ADRs cross-service
- Ficam no repo meta (`templates-meta/adrs/`)
- Referenciados por cada repo específico no PLAN.md

### Estratégia de PRs (coordenados vs stacked)
- **PRs coordenados**: um PR por repo, labels linkando (ex: `coordinated:feature-x`)
  - Mais simples de revisar isoladamente
  - Requer ordem de merge definida
- **Stacked PRs**: um PR depende do outro
  - Mais complexo mas garante ordem
  - Usar quando há dependência forte entre mudanças
- **Recomendação**: PRs coordenados + ordem de deploy documentada no ADR
```

---

## Alterações em arquivos existentes

### T25: `README.md` principal

Adicionar na seção 3 (árvore):
```text
├── examples/tier15/          # Exemplo do fluxo Tier 1.5 para tarefas complexas
```

Adicionar nova seção 12:
```markdown
## 12. Fluxo Tier 1.5 para Tarefas Complexas

Para features médias, críticas, ou que tocam contratos entre serviços, use o **fluxo Tier 1.5** com fases isoladas:

1. **Plan** (chat mode `pm-planner`, Opus) → PLAN.md + .feature
2. **RED** (chat mode `qa-red-writer`, Sonnet) → testes falhando
3. **GREEN** (chat mode `senior-engineer`, Sonnet) → implementação
4. **Contract Validation** (chat mode `contract-validator`, Sonnet) → breaking changes
5. **Review** (chat mode `pr-reviewer`, Sonnet/Opus) → laudo final

Arquivos de configuração em `general/copilot/chatmodes/` e `general/copilot/prompts/`. Orquestre com `/feature-full`.

Use o Tier 1 (seção 11) quando a tarefa NÃO toca contratos e é resolvível em sessão única.
```

### T26: `workflows/feature-tier15-multiphase.md`

Criar novo workflow referenciando o fluxo completo, apontando para os chat modes e prompts como "roles involved".
