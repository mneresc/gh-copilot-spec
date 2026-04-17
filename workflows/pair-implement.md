# Pair Implement Workflow

## Purpose
Guiar local e estritamente curtos períodos de pareamento intenso entre o Dev e a LLM gerando código isolado de forma coesa focado no slice.

## Entry condition
O Planejamento em Milestones já possuir os Stop and Fix engatilhados ou for uma micro tarefa onde `PLAN` não se julga necessário formalizar.

## Roles involved
- Pair Engineer (TS ou Python)

## Ordered steps
1. Humano carrega os domínios (Tabelas Base e Controllers a serem ativados) para o contexto local da Thread.
2. IA age em Micro-Slices preenchendo as lógicas com validações usando as skills TS ou Python worker.
3. Fim do Slice atualiza o arquivo `STATUS.md` pra fechar o rastro.

## Stop conditions
- Se uma Exception ou compilação barrar o caminho na máquina local do engenheiro.
- A IA desviar do `PLAN.md` fatiado para tentar escrever outro milestone escondido na refatoração.
