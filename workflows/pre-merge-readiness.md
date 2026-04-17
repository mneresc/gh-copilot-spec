# Pre-Merge Readiness Workflow

## Purpose
Passo definitivo de chancela, gerando a resposta binária Final aglutinando toda auditoria antes de soltar um botão de Merge no Repositório Remoto.

## Entry condition
- Feature completada.
- Audições individuais (Sec, Test, Obs) executadas previamente.

## Roles involved
- Release Readiness Reviewer

## Ordered steps
1. O Agente é convocado a ler o sumário contendo `AUDIT.md` consolidado e o `STATUS.md` final.
2. A test suite real automatizada da GitHub Action é passada para garantir zero divergência IA x Real.
3. Agente libera veredito: "Blocked" x "Ready with Caveatos" x "Ready".
