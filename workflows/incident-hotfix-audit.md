# Incident Hotfix Workflow

## Purpose
Estabelece a esteira enxuta para resoluções de problemas Críticos (P1/P0) com o Pair-Engineer na emergência sem burocracia do fluxo BDD longo, mas com Quality Gate cirúrgico de Auditoria ao fim.

## Entry condition
Tickets de incidentes reportados pela Plataforma Observabilidade ou Sustentação onde as SLA determinam velocidade máxima na nuvem de produção.

## Inputs
- Logs e Traces de Produção ou Stack trace da exceção.

## Roles involved
- Pair Engineer
- Incident Hotfix Reviewer

## Ordered steps
1. A Ação de Descoberta é entregue na Thread pura cruzando a exceção com a codebase local.
2. O Pair Engineer escreve o `Fix Minimal` (A Menor correção de arquivo isolada). Sem refatorações estéticas.
3. O Agent corre de forma acelerada na Skill de Teste Unitário.
4. O `Incident Hotfix Reviewer` revisa pra comprovar escopo restrito de P0 e autoriza Pull Request limpo.

## Escalation conditions
- IA tenta trocar a biblioteca base da requisição por outra achando ser "melhor". Hotfix barra essa intenção, focando em restaurar a funcionalidade falha restritamente.
