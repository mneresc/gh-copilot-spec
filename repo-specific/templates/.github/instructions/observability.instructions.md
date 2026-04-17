# Observability Local Instructions

## Stack Autorizada e Regras
- Formato do Log Base: JSON Lines (`{"time": "...", "msg": "...", "level": "info", "ctx": {}}`).
- Exporter: Console out. Nossos containers Docker enviam a saída padrão para o fluent-bit lateral que retransmite ao Datadog.
- Trace de Rede: Adicionado middleware `Datadog APM` (dd-trace). Não gere spans HTTP manuais, eles vêm de graça, apenas span contextuais.
- Máscara Ativa: O Logger varre as lógicas antes do print e remove chaves como `password`, `secret`, `ssn` ou `ccv`. **Nunca** comite uma alteração que desabilite essa trava.
