# Terraform Audit Workflow

## Purpose
Workflow isolado ou agregado ao fim do PR que impede destruições globais atuando antes da pipeline `terraform apply`.

## Entry condition
- Infra As Code modificado (`.tf` edits ou gerado `plan.out`).

## Roles involved
- Terraform Auditor

## Ordered steps
1. Carregamento dos "Inputs" em plan text para leitura da IA.
2. IAM Policies e State Blocks validados sob *aws-terraform-review*.
3. Retorno estrito pass/fail.

## Stop conditions
- Vazamentos explícitos de "VPC Peering" ou recursos Data caindo Fora da política da Organização na AWS.
