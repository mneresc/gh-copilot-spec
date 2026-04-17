# Instruções Locais (Repositório)

Este arquivo é a "Verdade Local". A camada geral provê os comportamentos e agentes; este arquivo dita os detalhes estritos deste repositório para evitar que a IA adivinhe stacks.

## 1. Comandos Reais 
*(Substitua os `<placeholders>` com as verdades do seu time)*
- Build: `npm run build`
- Linter: `npm run lint`
- Testes Unitários: `npm run test`
- Testes de Integração: `npm run test:e2e`

## 2. Organização de Código
- Camada de Domínio fica em `src/domain/`.
- Conexões com serviços AWS usando Node estão em `src/infrastructure/aws/`.
- Repositórios acessando o Prisma ficam em `src/infrastructure/repositories/`.

## 3. Logger Real
Utilizamos o **Pino** injetado em roteadores Express. Importação canonica: 
`import { logger } from '@utils/logger'`. Nunca chame `console.log`.

## 4. Métricas e Traces
Traces via OpenTelemetry com injeção automática. Se criar jobs manuais (workers), crie spans manualmente via `@utils/tracer`.

## 5. Limites e Áreas Sensíveis (Boundaries)
Este repositório acessa dados sensíveis dos clientes (PII). A pasta `src/domain/entities/users` possui lógicas que devem ser obrigatoriamente passadas pela lib interna `@crypto/masker` antes contatarem logs ou mensageria externa.

## 6. Runbooks / Infra Local
Para subir dependências (Banco e Redis locais) execute `docker-compose up -d`. Não tente mockar tabelas nos testes E2E, rodamos sempre os containers reais localmente.
