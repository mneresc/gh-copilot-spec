# Observability Review

## Purpose
Analisar criticamente instrumentação dos logs, traces, métricas customizadas de uma demanda no momento do audit, prevenindo o despejo massivo de informação lixo ou gargalos visuais operacionais e dados pessoais.

## When to use
- Nas etapas finais de Spec-driven audit de Backend, ou nos hotfixs.

## When not to use
- Em bibliotecas internas isoladas e pacotes NPM privados reutilizaveis puros (que devem não cuspir log, delegando para quem as chama).

## Expected inputs
- Modificações de backend completadas (Código .py, .ts, .go, etc).

## Operating steps
1. Vasculhe chamadas globais de sistema (Logger.info, console.log).
2. Cardinalidade: Verifique se existem labels de usuários ou requests únicos inseridos na emissão de Métricas Temporais (Prometheus Histogram, etc). IDs únicos devem ir apenas nos Traces e Logs, nunca em Métrica.
3. Tratamento Sensível (PII): Procure `Logger.info(req.body)` sem filtros que possa derramar passwords, CPFs, e SSNs.
4. Utilitário Operacional (Diagnóstico): O log que lança na falha do banco do servidor X, possui o motivo/variável causadora além e só estourar "Erro no Sistema"?

## Quality bar
Observability Review barra qualquer Log string não estruturado que polua parsers JSON e veta métricas de alto custo (Cardinality Bombing).

## Expected outputs
Indicações exatas das linhas onde a observabilidade é letal (PII Leakages), inflada (métricas com milhares de IDs rotulados) ou faltante (Silência do try/catch).

## Common failure modes
- Aceitar que um loop dentro de `workers` processe 100 mil itens e cuspa um `log.info` em cada item inflando a conta Datadog em horas.

## Minimal checklist
- [ ] Traces e Correlation-IDs navegam pelo controller até a exception?
- [ ] Dados PII ou senhas estão imune de vazamentos na stdout?
- [ ] Estão utilizando métricas estruturadas?
