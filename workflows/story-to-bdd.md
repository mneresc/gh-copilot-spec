# Story to BDD Workflow

## Purpose
Converter uma fatia ou estória vaga que entra (uma linha solta do slack) numa base transacional rígida de aceitação (Given/When/Then), limpando os achismos operacionais para algo factível.

## Entry condition
História da feature foi selecionada para iniciar Discovery. 

## Inputs
- Desejo de Manutenção Evolutiva.
- User Story pura.

## Roles involved
- PM BDD Manager

## Ordered steps
1. Ingira a História com *Story-intake* skill delimitando os atores reais (Tirando foco de "sistema faz isso" para "locadora de devolução cobra x").
2. Execute o Scoping da Feature excluindo as coisas fantasiadas.
3. Atire as conclusões finais purificadas com a *bdd-scenario-authoring* skill gravando os *GWT* do `BDD.md`.

## Outputs
- O arquivo limpo `BDD.md`.

## Escalation conditions
- Negócio (Product Manger real) se recusar a bater o martelo sobre furos lógicos descobertos via elaboração dos BDDs paralelos.
