# Incident Hotfix Review

## Purpose
Prover uma salvaguarda emergencial com regras excepcionais de velocidade em tempos críticos (P1s) para mitigar falha em produção preservando mínimo de qualidade e evitando consertos perigosos ("band-aids" com OOM-Kills ou Memory Leaks de desespero).

## When to use
- Quando operando em um pipeline ou ciclo restrito de **Hotfix workflow** após de uma queda reportada no ambiente.

## When not to use
- Adição de features planejadas.
- Sprints correntes não urgências.

## Expected inputs
- StackTrace, Logs ou descrições rudes do Erro Operacional.
- Solução Corretiva "Mínima" isolada em poucas linhas pelo mantenedor.

## Operating steps
1. Avaliar Tamanho: É estritamente a menor correção viável ou o Humano resolveu "Aproveitar pra Refatorar o componente"?
2. Revisão Focada em Degradação Extra: Esse conserto tapa o erro do Array fora do index, mas impõe um `while` que travaria toda o Node.js Event Loop assíncrono? O Banco não usa Índice sobre esse fix na query?
3. Indicação de Fix Definitivo: Gere um report dizendo, "O Band-aid serve agora para PR rápido, mas criem ticket para resolver X".

## Quality bar
O fix tem que ser O Menor Possível. Rejeitar refatorações amplas em Hotfix ou qualquer sujestão que introduza libs sistêmicas novas dependentes em meio de tiroteio.

## Expected outputs
Revisão cirurgica atestando "Pass" para consertos enxutos limpos que param o derramamento de sangue sem causar novos rombos invisíveis.

## Common failure modes
- O Agente "sugerir grandes modificações algoritmicas elegantes" num controlador do que um IF emergencial pragmático de 1 linha.

## Minimal checklist
- [ ] O código modificado só afeta a patologia listada?
- [ ] Refatoramentos desnecessários cosméticos foram evitados e o diff está sub-20 linhas e restrito a poucos módulos funcionais?
