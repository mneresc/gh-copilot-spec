# Slice Scoping

## Purpose
Pegar um entendimento bruto já analisado e fatiar a história verticalmente limitando estritamente até onde cada parte atinge. Definir o que entra e, principalmente, o que fica FORA de escopo.

## When to use
- Após uma demanda já possuir Intake prévio limpo e nós estarmos com um monte de coisas a fazer simultaneamente na cabeça.
- Ao identificar riscos de inflacionar demandas ("feature creep").

## When not to use
- Se a tarefa for uma correção micro ou pontual, como 2 linhas de CSS ou de regex.

## Expected inputs
- Demanda ou História validada.
- Lista ou diretório dos principais serviços que já cobrem o modelo subjacente.

## Operating steps
1. Leia o requerimento e escreva tudo que ele *NÃO PODE FAZER*.
2. Delimite as fatias técnicas como "Módulo 1: O Serviço Externo", "Módulo 2: O Banco de Dados", etc.
3. Se o slice for muito grande ainda, corte ao meio.

## Quality bar
Cada fatia (slice) deve ser testável finalizável separadamente. Não escreva um slice como "implementar e no fim veremos".

## Expected outputs
Uma lista estrutural indicando limites estritos de onde uma funcionalidade que termina começa a outra. E uma lista do que "NÃO VAMOS FAZER NESSE CICLO".

## Common failure modes
- Criar camadas gigantes de banco sem os endpoints correlacionados.
- Não declarar o fora de escopo.

## Minimal checklist
- [ ] O limite explícito está traçado?
- [ ] A feature caberia num pull request se codada sem o próximo item?

## Stack-specific notes
Linguagem agnóstica.
