# AWS Terraform Review

## Purpose
Proteger arquivos `.tf` operando ativamente na postura de cinto cego e redução agressiva na margem explosiva (blast radius) de alterações mal formatadas nos recursos Cloud. 

## When to use
- Durante revisões de infraestrutura antes dos PRs com extensões Terraform, em especial, AWS. 

## When not to use
- Na arquitetura de software contida em contêineres e lambdas na aplicação não ligada às instâncias ou declarativas puras.

## Expected inputs
- Arquivos `*.tf` impactados na infra.
- Contexto de plano (`terraform plan`).

## Operating steps
1. Repassar fortemente IAM: Procurar wildcards (`*`) em Policies e Actions que destroem o *Least Privilege*.
2. Verificar Drift e State Management: Existe hard-code de backends no state que prejudicam travas?
3. Blast Radius: O isolamento network existe (Security Groups abertos)? 
4. Encryption/KMS e Observabilidade: Todo S3 SQS, RDS está com as Flags KMS ativas pra dados sensíveis base? Log Groups estão definidos com retenção decente e não infinitas?

## Quality bar
Terraform é volátil e impiedoso na nuvem. Nenhuma ação será ignorada se levantar bandeira AWS perigosa em Network Exposure ou Rollback difíceis.

## Expected outputs
Relatório auditor sobre recursos perigosos sem Rollback prático expostos na rede de forma global, com reescrita corretiva imediata.

## Common failure modes
- Esquecer de checar Outputs críticos abertos com dados vindo sensíveis de RDS (SENHAS na interface do Terraform!).
- Ignorar de validar as VPCs na definição de instâncias ou lambdas.

## Minimal checklist
- [ ] O Estado (State) está isolado devidamente com locks remotos?
- [ ] As políticas IAM não carregam permissões em branco ou wildcards totalitários (*)?
- [ ] Os recursos expostos requerem KMS e a criptografia está habilitada flag-level?
