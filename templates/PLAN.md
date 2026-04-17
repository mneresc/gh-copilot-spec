# Plano de Execução (Plan)

Este arquivo rege rigidamente a sequência de código de uma feature complexa aprovada separando em "Milestones" de parada obrigatória em vez de codar a árvore inteira numa sentada.

## Milestone 1: Modificações Básicas de Tabela e Domínio
- [ ] Alterar o Schema SQL do banco de reservas (`schema.prisma`) inserindo os campos `delayPenaltyApplied` na tabela `Rent`.
- [ ] Gerar migração.
- *Stop and Fix Rule*: Pare. Não codifique os adapters web antes que as Entity tests passem sobre esse novo campo.

## Milestone 2: Lógicas (Service Layer)
- [ ] Construir a calculadora injetada no Service de Finalização `ReturnService.ts`.
- [ ] Cobrir com Testes Unitários de Matemática (como estipulado no plan).
- *Stop and Fix Rule*: O Mock de banco deve permitir assert com coverage em todos IFs do late return (Tolerância zero).

## Milestone 3: Interfaces AWS (Borda) e Controllers HTTP
- [ ] Alterar ou criar Rota da API REST expondo a ação de cálculo para simulação.
- [ ] Atualiza Schema OpenApi Swagger
- [ ] Emitir Fila EventBridge caso haja quebra.
- *Stop and Fix Rule:* Auditores atuam rodando e2e mockando apenas a AWS ou ligando localstack. Rollback de branches se quebrar Swagger de cliente.
