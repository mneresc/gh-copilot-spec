# Copilot Instructions — Regras Gerais

> Cole este arquivo em `.github/copilot-instructions.md` de cada projeto.
> Preencha a seção "Verdade Local" com os dados específicos do repositório.

---

## Princípios

- **Plan → Implement → Review** em cada tarefa significativa.
- Sempre escreva testes antes ou junto com a implementação.
- Fix mínimo: não refatore código fora do escopo solicitado.
- Self-review embutido: revise seu próprio output antes de apresentar.
- Nunca adivinhe a verdade local — use apenas o que está documentado abaixo.

## Qualidade de Código

- Funções curtas (< 40 linhas). Se passar, extraia.
- Early return sempre que possível. Evite aninhamento > 3 níveis.
- Nomeie variáveis e funções de forma descritiva — sem abreviações crípticas.
- Não deixe `TODO`, `FIXME` ou código comentado sem justificativa.
- Trate todos os erros explicitamente. Nunca engula exceções silenciosamente.
- Imports organizados: stdlib → terceiros → internos, com linha em branco entre grupos.

## Testes

- Todo bug fix deve incluir um teste que reproduza o bug (red → green).
- Todo comportamento novo deve ter pelo menos: cenário feliz + cenário de erro.
- Testes devem ser independentes entre si — sem estado compartilhado.
- Nomes de teste descritivos: `deve retornar 404 quando usuário não existe`.
- Mocks apenas para dependências externas (DB, APIs, filesystem).

## Segurança

- Nunca logue dados sensíveis (tokens, senhas, PII).
- Valide e sanitize todo input externo na borda (controller/handler).
- Use parametrized queries — nunca concatene strings em queries SQL.
- Secrets vêm de variáveis de ambiente, nunca hardcoded.
- Em caso de dúvida sobre impacto de segurança, pare e peça revisão humana.

## Git & PRs

- Commits atômicos: uma mudança lógica por commit.
- Mensagens no formato: `tipo(escopo): descrição` (ex: `fix(auth): validate token expiry`).
- PRs devem ter descrição do que mudou, por que, e como testar.

## Comunicação

- Responda em português quando o prompt for em português.
- Use inglês para nomes de variáveis, funções, classes e commits.
- Seja conciso — bullets > parágrafos.

---

## Verdade Local (PREENCHA POR PROJETO)

```yaml
# Stack
linguagem:        # ex: TypeScript, Go, Python
framework:        # ex: Express, Gin, FastAPI
banco:            # ex: PostgreSQL, Firestore
orm:              # ex: Prisma, GORM, SQLAlchemy
testes:           # ex: Jest, Go testing, Pytest
logger:           # ex: Pino, Zap, structlog

# Estrutura
src_principal:    # ex: src/, cmd/, app/
testes_dir:       # ex: __tests__/, *_test.go
config_dir:       # ex: .env, config/

# Comandos
rodar_testes:     # ex: npm run test, go test ./...
rodar_lint:       # ex: npm run lint, golangci-lint run
rodar_local:      # ex: docker compose up, npm run dev
gerar_tipos:      # ex: npx prisma generate, go generate

# Padrões específicos
regras_extras:
  # - "DTOs sempre com Zod schema"
  # - "Nunca use console.log, use o logger injetado"
  # - "Handlers recebem dependências via struct, não global"
```
