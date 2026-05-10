# Plano de Adições — MCP, Guia Iniciantes e Gerador de Copilot Instructions

> **INSTRUÇÃO**: Este documento COMPLEMENTA o `implementation_plan_fluxo_enxuto.md` existente. NÃO substitui — adiciona novos arquivos e modifica seções existentes. Uma IA com menor capacidade deve executar cada item isoladamente.

---

## RESUMO DAS ADIÇÕES

| # | O que | Arquivo novo/editado | Por que |
|---|-------|---------------------|---------|
| A1 | Tutorial MCP (instalação + config) | `examples/mcp-setup-guide.md` | **NOVO** — separado, focado em npm |
| A2 | Sugestões MCP nos exemplos de uso | Editar `examples/claude-copilot-workflow.md` (Seção 1) | Integrar MCP no setup inicial |
| A3 | Guia para iniciantes em IA | `examples/guia-iniciantes-ia.md` | **NOVO** — onboarding de quem nunca usou IA |
| A4 | Prompt gerador de copilot-instructions | `examples/prompts/gerar-copilot-instructions.md` | **NOVO** — prompt template reutilizável |
| A5 | Exemplo preenchido com inputs | `examples/copilot-instructions-example.md` | Expandir o já planejado com "antes/depois" |
| A6 | Atualizar README examples | Editar `examples/README.md` | Adicionar links para A1, A3, A4 |
| A7 | Atualizar estrutura na Parte C | Editar `implementation_plan_fluxo_enxuto.md` | Refletir novos arquivos |

---

## A1: `examples/mcp-setup-guide.md` — Tutorial MCP (NOVO)

### Propósito
Guia passo-a-passo para instalar e configurar MCP servers no VSCode usando npm. Público: dev que nunca usou MCP.

### Estrutura do arquivo

```markdown
# Guia de Setup — MCP (Model Context Protocol) no VSCode

## O que é MCP?
[2-3 linhas: protocolo que permite o Copilot acessar ferramentas externas
(GitHub, banco de dados, filesystem) durante o Agent Mode. Sem MCP, o
Copilot só vê os arquivos abertos. Com MCP, ele pode consultar PRs,
buscar na web, ler schemas de banco, etc.]

## Pré-requisitos
- VSCode 1.99+ (ou Insiders)
- Node.js 18+ e npm instalados
- GitHub Copilot ativo com Agent Mode habilitado

## 1. Onde fica a configuração
[Explicar .vscode/mcp.json (workspace) vs MCP: Open User Configuration (global)]
[Recomendação: workspace para compartilhar com o time via git]

## 2. Instalação dos servers recomendados

### 2.1 GitHub (PRs, Issues, Cross-repo)
[Bloco de config JSON com npx -y @modelcontextprotocol/server-github]
[Pré-requisito: criar fine-grained PAT com escopo mínimo]
[Comando para testar: abrir Agent Mode e perguntar "liste os PRs abertos"]

### 2.2 Fetch (Leitura de URLs/Documentação)
[Bloco JSON com @modelcontextprotocol/server-fetch]
[Uso: pedir ao Copilot para ler documentação online]
[Risco: BAIXO — read-only]

### 2.3 Filesystem (Acesso a diretórios fora do workspace)
[Bloco JSON com @modelcontextprotocol/server-filesystem]
[Uso: ler monorepos, acessar specs de outro repo local]
[⚠️ Cuidado: limitar args aos paths necessários]

### 2.4 Memory (Memória persistente entre sessões)
[Bloco JSON com @modelcontextprotocol/server-memory]
[Uso: manter contexto entre sessões de chat]
[Dica: útil para projetos longos]

### 2.5 PostgreSQL / Database (Opcional)
[Bloco JSON com @modelcontextprotocol/server-postgres]
[Uso: consultar schema do banco, gerar queries]
[⚠️ NUNCA apontar para produção — apenas dev/local]

## 3. Servers NÃO recomendados inicialmente
[Tabela: server, por que não, alternativa]
- Schema Registry (Confluent) → output verboso, copie schema localmente
- Datadog/Grafana → output enorme, use links diretos no prompt
- Slack/Email → risco de ação destrutiva, evitar

## 4. Segurança
[Checklist:]
- [ ] Nunca hardcode tokens no mcp.json — use ${env:VARIAVEL}
- [ ] PATs com escopo mínimo (fine-grained)
- [ ] Nunca conecte banco de produção
- [ ] Revise servers instalados trimestralmente

## 5. Verificando se está funcionando
[Passos: abrir Agent Mode → ícone de ferramentas → listar tools disponíveis]
[Troubleshooting: Output panel → selecionar "MCP" no dropdown]

## 6. Exemplo completo de .vscode/mcp.json
[JSON completo com github + fetch + filesystem configurados]
```

### Modelo sugerido para quem for preencher: Sonnet (instrucional, sem ambiguidade)

---

## A2: Sugestões MCP no `claude-copilot-workflow.md`

### O que mudar
Na **Seção 1 (Setup inicial no VSCode + Copilot)** do plano existente, adicionar um sub-item:

```markdown
### MCP — Ferramentas externas para o Agent Mode

O Copilot em Agent Mode pode usar ferramentas externas via MCP
(Model Context Protocol). Isso permite que ele consulte PRs do GitHub,
leia documentação online, acesse schemas de banco, etc.

**Setup rápido**: Veja o [Guia de Setup MCP](mcp-setup-guide.md).

**Servers recomendados para o fluxo enxuto**:
| Server | Quando usar | Exemplo de uso |
|--------|-------------|----------------|
| GitHub | Bug fix / Feature → consultar PRs relacionados | "Há PRs abertos que tocam este arquivo?" |
| Fetch | Design exploration → ler docs de lib | "Leia a doc do Zod v4 e me diga o que mudou" |
| Filesystem | Monorepo → acessar spec de outro serviço | "Leia o openapi.yaml do serviço X" |
| Memory | Sessões longas → manter contexto | Automático entre sessões |

**Nos templates de prompt**: quando um template pode se beneficiar de MCP,
indicar com `[MCP: server-name]` ao lado do `[Modelo: X]`.
```

### Onde inserir no plano existente
Após a linha 126 do `implementation_plan_fluxo_enxuto.md` (fim da Seção 1), antes da Seção 2.

### Impacto nos templates de prompt (Seção 3)
Adicionar tag `[MCP: xxx]` opcional nos templates onde faz sentido:

| Template | MCP sugerido | Justificativa |
|----------|-------------|---------------|
| Bug fix (T2) | `[MCP: github]` | Consultar PRs/issues relacionados ao bug |
| Feature pequena (T3) | `[MCP: github, fetch]` | Ver issues, ler docs de libs |
| Design exploration (T5) | `[MCP: fetch, filesystem]` | Ler docs externas, specs de outros repos |
| Bug sensível (T6) | `[MCP: github]` | Verificar histórico de mudanças na área |

---

## A3: `examples/guia-iniciantes-ia.md` — Guia para Iniciantes (NOVO)

### Propósito
Onboarding para devs que nunca usaram IA no desenvolvimento. Sem jargão, com analogias.

### Estrutura do arquivo

```markdown
# Guia para Iniciantes — Desenvolvimento Assistido por IA

## O que é desenvolvimento assistido por IA?
[Analogia: IA é um par-programmer júnior muito rápido que sabe muita
sintaxe mas não conhece seu projeto. Você é o sênior que guia, revisa e
decide. A IA propõe, você dispõe.]

## Conceitos-chave (glossário visual)

### Copilot
[O que é, como ativar, o que faz: autocomplete + chat + agent mode]

### Prompt
[O que é: a instrução que você dá para a IA. Quanto melhor o prompt,
melhor o resultado. Analogia: é como um ticket de Jira — se for vago,
o resultado será vago.]

### Modelo (Haiku / Sonnet / Opus)
[Tabela simples com analogia:]
| Modelo | Analogia | Quando usar | Custo |
|--------|----------|-------------|-------|
| Haiku | Estagiário rápido | Renomear, mover, CSS | $ |
| Sonnet | Dev pleno | Bug fix, features, testes | $$ |
| Opus | Arquiteto sênior | Design, segurança, debug complexo | $$$ |

### Agent Mode vs Inline Chat vs Chat Panel
[Diferença com analogia:]
- Inline Chat (Ctrl+I) = "edita aqui no arquivo que estou olhando"
- Chat Panel (Ctrl+Shift+I) = "conversa longa, planejamento"
- Agent Mode (Ctrl+Shift+Alt+I) = "modo autônomo: lê, edita, roda terminal"

### MCP (Model Context Protocol)
[Analogia: se o Copilot é um assistente, os MCP servers são as
"ferramentas na mesa dele" — acesso ao GitHub, ao banco, à web.
Sem MCP ele só vê o que você abre no editor.]

### copilot-instructions.md
[Analogia: é o "manual do projeto" que a IA lê antes de trabalhar.
Sem ele, a IA adivinha. Com ele, ela sabe que logger usar, como rodar
testes, e o que NUNCA fazer.]

### Skills, Agents, Workflows
[Explicação progressiva:]
- Skill = uma capacidade específica ("sabe escrever teste unitário")
- Agent = uma persona com várias skills ("é o engenheiro de testes")
- Workflow = receita que encadeia agents/skills ("fluxo de feature completa")

## Primeiros passos (em 10 minutos)

### Passo 1: Configurar o VSCode
[Referência para Seção 1 do claude-copilot-workflow.md]

### Passo 2: Criar seu copilot-instructions.md
[Referência para o prompt gerador: gerar-copilot-instructions.md]
[Explicar que é a coisa mais importante — a IA sem isso é genérica]

### Passo 3: Fazer sua primeira tarefa
[Exemplo hands-on: renomear uma variável usando Haiku]
[Exemplo: corrigir um bug simples usando Sonnet com o template bugfix]

### Passo 4: Instalar MCP (opcional)
[Referência para mcp-setup-guide.md]

## Erros comuns de iniciantes
[Lista com correção:]
1. "Opus para tudo" → comece com Sonnet, suba se precisar
2. "Aceitar tudo sem ler" → SEMPRE revise o que a IA gerou
3. "Prompt de uma palavra" → descreva contexto, sintoma, expectativa
4. "Não criar copilot-instructions.md" → é o #1 em ROI
5. "Ignorar testes" → peça testes junto com a implementação
6. "Copiar código de IA sem entender" → se não entende, peça explicação

## Próximos passos
[Trilha de evolução:]
1. Iniciante → Use templates trivial e bugfix por 1 semana
2. Intermediário → Use feature-pequena e refactor, experimente MCP
3. Avançado → Use design-exploration com Opus, fluxo Tier 1.5
```

### Modelo sugerido para criação: Sonnet

---

## A4: `examples/prompts/gerar-copilot-instructions.md` — Prompt Gerador (NOVO)

### Propósito
Prompt template reutilizável que o dev cola no Chat Panel para a IA analisar qualquer projeto e gerar o `copilot-instructions.md` automaticamente.

### Estrutura do arquivo

```markdown
# Prompt — Gerar Copilot Instructions

## Quando usar
Ao configurar um repositório pela primeira vez com Copilot.
Ou quando o projeto mudou significativamente (novo framework, ORM, etc.)

## Modelo: Sonnet (ou Opus se o projeto for grande/complexo)

## Cerimônia: média (rodar uma vez, revisar output, salvar)

## Pré-requisitos
- Estar com o projeto aberto no VSCode
- Agent Mode ativo (para a IA ler arquivos do projeto)
- MCP: nenhum obrigatório (filesystem ajuda se for monorepo)

## Campos de entrada (o que você precisa saber/preencher)

| Campo | Obrigatório? | Exemplo | Como descobrir |
|-------|-------------|---------|----------------|
| Nome do projeto | ✅ | "API de Gestão de Pedidos" | README.md ou package.json "name" |
| Linguagem principal | ✅ | TypeScript | Extensões dos arquivos em src/ |
| Linguagens secundárias | ❌ | SQL, YAML, Shell | Procurar em scripts/, infra/, etc. |
| Frameworks principais | ✅ | Express, Prisma, Jest | package.json "dependencies" |
| Tipo de aplicação | ✅ | API REST | Presença de routes/, controllers/, endpoints |
| Gerenciador de pacotes | ✅ | npm | Existência de package-lock.json vs yarn.lock vs pnpm-lock.yaml |
| Estrutura de pastas | ✅ | src/domain/, src/infra/, src/api/ | `ls -R src/` ou tree |
| Padrão de nomenclatura | ✅ | kebab-case (arquivos), camelCase (variáveis) | Olhar 5 arquivos |
| Framework de testes | ✅ | Jest | package.json "devDependencies" |
| Linter/formatter | ❌ | ESLint + Prettier | .eslintrc, .prettierrc |
| Logger | ❌ | Pino | grep por "logger" ou "log" em src/ |
| ORM / acesso a banco | ❌ | Prisma | package.json, pasta prisma/ |
| Variáveis de ambiente | ❌ | ver .env.example | Arquivo .env.example ou .env.template |
| Docker | ❌ | sim, docker-compose.yml | Existência do arquivo |
| CI/CD | ❌ | GitHub Actions | .github/workflows/ |

## Exemplos de entrada preenchidos

### Exemplo 1 — API Node.js + TypeScript
[Bloco copiável com todos os campos preenchidos para o projeto fictício
"API de Gestão de Pedidos": Node 20, TypeScript 5.3, Express 4,
Prisma 5, PostgreSQL 16, Jest, Pino, ESLint+Prettier, src/domain +
src/infrastructure + src/api, kebab-case, etc.]

### Exemplo 2 — Backend Go (Microserviço)
[Bloco copiável para: Go 1.22, Gin, GORM, PostgreSQL, go test,
golangci-lint, cmd/ + internal/ + pkg/, snake_case, etc.]

### Exemplo 3 — Frontend React + Next.js
[Bloco copiável para: TypeScript, Next.js 14, React 18, Tailwind,
Zustand, Vitest, app/ + components/ + lib/ + hooks/, PascalCase
componentes, camelCase utils, etc.]

## O Prompt (copiável)

[Bloco de código com o prompt completo que o user forneceu, formatado
como template com placeholders claros onde inserir os valores acima.
Incluir as 5 etapas: Mapeie → Convenções → Padrões → Arquitetura →
Infra, e a estrutura de saída esperada.]

## Exemplo de saída gerada
[Referência: ver examples/copilot-instructions-example.md para
um exemplo completo de output gerado por este prompt]

## Dicas
- Rode com Agent Mode para a IA poder ler os arquivos do projeto
- Se o projeto for grande (>100 arquivos em src/), indique os 5
  arquivos mais representativos no prompt
- Revise SEMPRE o output — a IA pode errar paths ou confundir padrões
- Depois de gerar, adicione regras recorrentes conforme usar o Copilot
```

### Modelo sugerido para criação do arquivo: Sonnet

---

## A5: Expandir `examples/copilot-instructions-example.md`

### O que mudar no plano existente (Arquivo 8, linha 347-353)
Além do exemplo já planejado (projeto fictício "API de Gestão de Pedidos"), adicionar:

1. **Seção "Inputs usados"**: mostrar a tabela de campos preenchida que gerou o exemplo
2. **Seção "O que a IA detectou vs o que o humano ajustou"**: marcar com ✅ (detectado) e ✏️ (ajustado) cada seção
3. **Segundo exemplo menor** (Go microservice) para mostrar que funciona em qualquer stack

---

## A6: Atualizar `examples/README.md`

### Adicionar à tabela de arquivos (plano existente, Arquivo 9, linha 354-359)

| Arquivo | Descrição | Modelo |
|---------|-----------|--------|
| `mcp-setup-guide.md` | Tutorial de instalação MCP no VSCode com npm | Sonnet |
| `guia-iniciantes-ia.md` | Onboarding para devs que nunca usaram IA | — |
| `prompts/gerar-copilot-instructions.md` | Prompt para gerar copilot-instructions.md de qualquer projeto | Sonnet/Opus |

### Adicionar nova seção no README

```markdown
## Para Iniciantes
Se você nunca usou IA para desenvolvimento, comece por:
1. [Guia para Iniciantes](guia-iniciantes-ia.md) — conceitos e primeiros passos
2. [Gerar Copilot Instructions](prompts/gerar-copilot-instructions.md) — configure a IA para seu projeto
3. [Setup MCP](mcp-setup-guide.md) — conecte ferramentas externas (opcional)
```

---

## A7: Atualizar estrutura no `implementation_plan_fluxo_enxuto.md`

### Modificação na Parte C — Estrutura final (linhas 88-101)

Substituir a árvore por:
```text
examples/
├── README.md
├── claude-copilot-workflow.md
├── mcp-setup-guide.md                    ← NOVO
├── guia-iniciantes-ia.md                 ← NOVO
├── prompts/
│   ├── trivial.md
│   ├── bugfix.md
│   ├── feature-pequena.md
│   ├── refactor.md
│   ├── design-exploration.md
│   ├── bugfix-sensivel.md
│   └── gerar-copilot-instructions.md    ← NOVO
└── copilot-instructions-example.md
```

### Modificação na Parte F — Ordem de execução (linhas 399-412)

Inserir após o passo 8 (antes do README):
```
8.1. Criar examples/mcp-setup-guide.md
8.2. Criar examples/guia-iniciantes-ia.md
8.3. Criar examples/prompts/gerar-copilot-instructions.md
```

Ajustar passo 9 (README) para incluir links dos novos arquivos.

---

## RESUMO DE MCP SERVERS RECOMENDADOS (referência para A1 e A2)

| Server | Pacote npm | Uso no fluxo | Risco | Config env |
|--------|-----------|--------------|-------|------------|
| GitHub | `@modelcontextprotocol/server-github` | PRs, issues, cross-repo | MÉDIO | `GITHUB_TOKEN` (fine-grained PAT) |
| Fetch | `@modelcontextprotocol/server-fetch` | Ler docs/URLs | BAIXO | Nenhum |
| Filesystem | `@modelcontextprotocol/server-filesystem` | Ler dirs fora do workspace | MÉDIO | paths nos args |
| Memory | `@modelcontextprotocol/server-memory` | Memória entre sessões | BAIXO | Nenhum |
| PostgreSQL | `@modelcontextprotocol/server-postgres` | Schema e queries dev | ALTO | `DATABASE_URL` (só dev!) |

### Instalação padrão (todos usam npx)
```json
{
  "servers": {
    "<nome>": {
      "command": "npx",
      "args": ["-y", "<pacote-npm>"],
      "env": { "<VAR>": "${env:<VAR>}" }
    }
  }
}
```

---

## ORDEM DE EXECUÇÃO COMPLETA (com adições)

1. ~~Criar `examples/claude-copilot-workflow.md`~~ (existente no plano — **adicionar sub-seção MCP**)
2. Criar `examples/prompts/trivial.md` (sem mudança)
3. Criar `examples/prompts/bugfix.md` (**adicionar tag `[MCP: github]`**)
4. Criar `examples/prompts/feature-pequena.md` (**adicionar tag `[MCP: github, fetch]`**)
5. Criar `examples/prompts/refactor.md` (sem mudança)
6. Criar `examples/prompts/design-exploration.md` (**adicionar tag `[MCP: fetch, filesystem]`**)
7. Criar `examples/prompts/bugfix-sensivel.md` (**adicionar tag `[MCP: github]`**)
8. Criar `examples/copilot-instructions-example.md` (**expandir com inputs**)
9. **NOVO** → Criar `examples/prompts/gerar-copilot-instructions.md`
10. **NOVO** → Criar `examples/mcp-setup-guide.md`
11. **NOVO** → Criar `examples/guia-iniciantes-ia.md`
12. Criar `examples/README.md` (**expandir com novos links + seção iniciantes**)
13. Editar `README.md` principal (3 alterações da Parte E)
