# Security Review

## Purpose
Skill fundamental transversa exigida em momentos chave do delivery para garantir que as portas não fiquem escancaradas, revisando a arquitetura codada num prisma de desconfiança pura (zero-trust).

## When to use
- Em PR Gates, após a execução ou concepção da feature.
- Em code-reviews.
- Recomenda-se acionar durante o workflow se algo for crítico com PII ou senhas.

## When not to use
- Tarefas não funcionais como edição de CSS de botão do dashboard ou lints de indentação.

## Expected inputs
- As lógicas do *Diff* de PR ou pastas a serem revistas.

## Operating steps
1. **Identificar Autenticação/Autorização (Authn/Authz)**: O recurso valida quem chama antes de fazer o ato? Checa escopos transversais e Tenants para evitar IDOR (Insecure Direct Object Reference)?
2. **Segredos e Logs Sensíveis**: O arquivo envia dados na stdout? O objeto é despejado nu ignorando a criptografia de campos como SSN, Tokens e Cartões de Crédito?
3. **Trust Boundaries e PII**: Exposição de dados ou injestão desenfreada de dados sem limpeza. (Input Validators).
4. **Resiliência e Idempotência (Replays)**: O end-point tolera "double-clicks"? (Ex: 2 transferências de 50 reais ao invés de uma se o client travar).
5. **Configurações Default Inseguras / Dependências**: Verificação básica de imports perigosos recentes inseridos no diff.

## Quality bar
Toda checagem será tratada não como "sugestão polida", mas obrigando um "Veto" duro caso algo perigoso escale até produção.

## Expected outputs
Relatório pontual de anomalias encontradas e as linhas corretivas onde falhas podem causar explorações.

## Common failure modes
- Reclamar de "falta de SSL" em um contêiner Docker rodando em K8s atrás de um Load Balancer que já faz SSL Termination (a skill de Segurança ignora as vezes o quadro maior infra-wise, devendo focar no *código/diff*).

## Minimal checklist
- [ ] O novo código impede alteração de objetos baseados por IDs sequenciais previsiveis sem forte check de owner?
- [ ] Segredos são referenciados via cofres ou variáveis imutáveis de pipeline e nunca strings in hardcoded tests e logs?
