# Plano de Testes (Test Plan)

Referência Base: docs/BDD.md

## Estratégia de Qualidade

Para bater na especificação garantindo solidez, adotamos as seguintes camadas para essa feature em específico:

### 1. Testes Unitários Rápido (Lógicos)
Aqui mockaremos tudo referente ao Banco de Dados e AWS. Testaremos a Matemática e o Algoritmo.
- *Classe Alvo*: `ReturnChargeCalculator.ts`
- *O que testar*: 
   - Deve aplicar tolerância de 2 horas e retornar rate 0.
   - Deve cobrar taxa extra de final de semana se cair domingo. 

### 2. Testes de Integração (Banco Acionado)
O objetivo aqui não é refazer as contas de diárias do unitário novamente, mas testar a amarração da arquitetura.
- *Fluxo alvo:* Fluxo HTTP + Salvar Posição Transacional DB.
- *Ações*: Postman rodará / POST returns passando objeto que força late return. A test suite confere no Mongo/Prisma real subido via *Testcontainers* se o contrato mutou o campo `status` para "Billed"

### 3. Exclusões Planejadas (Não vamos Testar)
- A entrega de E-mails via Sendgrid real nunca será testada. Usaremos um FakeSMTP ou mockaremos a Interface EmailService localmente para validar a tentativa apenas.
