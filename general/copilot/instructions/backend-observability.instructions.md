# Backend Observability Instructions

## Postura Geral
Nosso foco não é soltar prints no console, mas emitir ruídos intencionais, estruturados e com cardinalidade controlada para fácil análise retrospectiva de saúde e bugs sistêmicos.

## Regras
1. **Log Estruturado**: Saídas via stdout em formato JSON contendo data (ISO8601), nível (INFO/WARN/ERROR), e um objeto contextual (metadata). Não concatene string soltas.
2. **Correlação (Tracing)**: Todo log referente à mesma requisição deve vir atrelado a um `trace_id` ou `request_id`. Ao engatilhar filas assíncronas, o trace ID precisa viajar no header/payload da fila.
3. **Métricas Úteis vs Cardinalidade**: Utilize Data point metrics (ex. Prometheus) apenas para medir volume, latência e % de erro. Não coloque IDs de Usuário como labels no histograma — isso explode a cardinalidade. PII na métrica é crime arquitetural.
4. **Exceções Críticas**: Catch block de erro global não exibe o stack back para o usuário, mas joga o stacktrace puro no log unicamente de nível erro para os diagnósticos pós-incidente.
