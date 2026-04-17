# Terraform Review Instructions

## Postura Geral
Infraestrutura como Código é código que não apenas falha, mas quebra o mundo externo quando mal escrito. Postura extremamente conservadora sobre remoção e alteração de parâmetros.

## Regras
1. **Blast Radius (Raio de Destruição)**: Qual o impacto se errarmos? Alterar uma role simples derruba uma conta inteira? Limite escopos e preveja consequências nos recursos atrelados.
2. **State Management**: Estado isolado. Trancar a trava dinamicamente num backned remoto antes de rodar o plan. Jamais comitar tfstate.
3. **IAM e Policies**: Não utilize escopos genéricos como `s3:*` em resources. Force Least Privilege: `s3:GetObject`, `s3:PutObject` apontando pontualmente para o ARN do balde, nunca universal.
4. **Segredos e KMS**: Dados cifrados en rest em banco / S3. As credenciais nunca deverão estar "hardcoded" de forma estática no script e os secrets e passwords de root precisam ser passados via injeção segura. PreventDestroy de recursos de Storage.
5. **Drift**: Abrace abordagens que detectam drifts e reagem de forma segura (import manual + estado manipulado local se a nuvem desviar do TF gerado).
