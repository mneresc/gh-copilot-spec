# Camada Geral vs. Camada Específica

Para o bom funcionamento de assistentes autônomos baseados em IA, a fragmentação de documentações baseadas em responsabilidades é o que separa um repositório mantível de um emaranhado impossível de operar.

## O Que é a Camada Geral
As práticas comuns e habilidades sistêmicas. Isso fica no computador do desenvolvedor ou compartilhado num repositório unificado corporativo injetado no SO local.

**Exemplos do que VAI aqui:**
- `unit-test-ts.md`: Exige a padronização do arranjo (Arrange/Act/Assert).
- `backend-security.md`: Proíbe gravar segredos em log de produção e exige sanitização de entrada.
- **Workflow genérico**: `slice-scoping` (que tenta pegar a demanda e cortar em passos pequenos independentemente da plataforma).

## O Que é a Camada Específica
A tradução desse workflow genérico para a arquitetura individual mantida naquele diretório `.git`.

**Exemplos do que VAI aqui:**
- *Neste projeto a segurança se faz através do AWS IAM Role "prod-lambda-role".*
- *Nossos testes são testados apenas usando `npm run test:vitest`.*
- *Não lidamos com multitenant neste banco de dados.*
- *Nossos logs utilizam Pino com roteamento para Datadog.*

## Erros Comuns de Mistura
1. **Atrelar Stack Específica nas Skills Globais**: Colocar em `security-review` a instrução *"valide sempre os tokens pelo Auth0 no middleware de `src/auth.ts`"* quebra sua fundação. Daqui a 2 meses um projeto pode usar Cognito na `src/middlewares/auth.py`.
2. **Atrelar Conceitos Universais na Camada Específica**: Se o repositório possuir 3 arquivos sobre como usar a notação AAA nos testes, há sobreposição manual não escalável correndo risco de quebrar regras universais. A IA vai ficar confusa lendo dois documentos divergentes sobre o que é uma boa prática.
