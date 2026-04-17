# Agentes e Funções Locais Aprovadas

A IA tem autorização e conhecimento para assumir as seguintes personas dentro deste repositório, ativando as `instructions` adequadas:

| Agent | Responsabilidade e Escopo Local |
|-------|-------------------------------|
| `@pair-engineer-ts` | Entende Express e injeção do Prisma no repositório. Lida com features de produto. |
| `@test-auditor` | Checagem de cenários Jest. Baseia-se no setup em `jest.config.js`. |
| `@security-auditor` | Checa PII baseando-se especificamente nas entidades de cartões salvas e mascaradas em banco local. |

## Como usar
Quando iniciar prompts na raiz deste projeto, anexe (mention) primeiramente a respectiva role com a intenção clara. Ex:
`@pair-engineer-ts crie os arquivos contidos no Milestone 1 de docs/PLAN.md`.
