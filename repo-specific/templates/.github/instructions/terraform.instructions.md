# Terraform Local Instructions

## Stack Autorizada
- Tool: Terraform `1.5.x` ou OpenTofu.
- Provider: `hashicorp/aws` `v5.0+`
- TF State: Remote Backend no bucket S3 central `corp-terraform-states-sa-east-1` utilizando locking via DynamoDB na tabela `terraform-locks`.

## Convenções Locais
- Todos os recursos devem ter uma Tag obrigatória de `BillingCode` e `Environment`.
- Diretórios são quebrados por ambientes: `environments/dev/` e `environments/prod/`.
- Repositório não gera credenciais locais (`terraform.tfvars` desconsiderado no git); elas vêm injetadas via OIDC e GH Actions Secrets.
