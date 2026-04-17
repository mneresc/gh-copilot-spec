# Execution Status Update

## Purpose
Manter a rastreabilidade sincrônica na execução de cada slice de código feito pelo Pair Engineer preenchendo o log em `STATUS.md`.

## When to use
- Em cada intersecção ou fechamento de Milestone de código durante o workflow de Spec-Driven feature.

## When not to use
- Trabalhos ad-hoc e reparos de typo ou scripts curtos que não geram artefatos.

## Expected inputs
- Alterações no Git working-directory.
- Acesso à verificação de testes atual do framework de teste da aplicação.

## Operating steps
1. Registre o estado do ciclo ou *slice* na primeira seção.
2. Anote os testes falhos atuais, caso existam, que a equipe ou a IA precise resolver.
3. Elenque explicitamente as "Decisões Tomadas" nas minúcias do código (ex: "Preferi usar Map em vez do Object cru por perfomance na key de busca de IDs vindo do redis"). 
4. Marque com 1 linha o que fará no cenário do próximo *slice*.

## Quality bar
Resumir sem jogar código em bloco. O status resume contexto. Ele é uma bússola de reinício caso o chat se perca no contexto máximo ou caia no fim do expediente do engenheiro.

## Expected outputs
O arquivo `STATUS.md` na raiz técnica da feature com o checkpoint persistente gravado.

## Common failure modes
- Esquecer de registrar os problemas abertos / testes não passantes antes da finalização, entregando o código num estado em que o programador não lembra amanhã.

## Minimal checklist
- [ ] Listou próximo passo?
- [ ] Detalhou impasses achados durante o slice anterior?
