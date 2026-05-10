# Exemplo: `.github/copilot-instructions.md`

Este é um exemplo preenchido baseado no [template principal](../repo-specific/templates/.github/copilot-instructions.md). 
Aqui simulamos um projeto fictício chamado "API de Gestão de Pedidos".

```markdown
# Contexto do Projeto: API de Gestão de Pedidos

## Stack Tecnológica
- **Backend**: Node.js + TypeScript
- **Framework**: Express
- **Banco de Dados**: PostgreSQL
- **ORM**: Prisma
- **Testes**: Jest
- **Logger**: Pino

## Estrutura de Diretórios
- `src/domain/`: Regras de negócio, entidades e interfaces independentes de tecnologia.
- `src/infrastructure/`: Implementação de repositórios (Prisma), integrações externas.
- `src/api/`: Controladores Express, rotas, middlewares e validação de input.

## Regras e Padrões (Verdade Local)
- NUNCA use `console.log`. Use sempre a instância do `logger` injetada (Pino).
- Validação de entrada na API DEVE usar Zod schemas. Não implemente validação manual no controller.
- Funções assíncronas no controller devem estar sempre envolvidas pelo middleware `asyncHandler`.
- Testes unitários devem seguir o padrão `describe`, `it`, e o nome do arquivo deve ser `*.spec.ts`.
- Evite aninhamento excessivo em `if/else`. Retorne early sempre que possível.

## Comandos Essenciais
- **Rodar testes**: `npm run test` ou `npm run test:watch`
- **Gerar Prisma Client**: `npx prisma generate`
- **Linter**: `npm run lint`
```
