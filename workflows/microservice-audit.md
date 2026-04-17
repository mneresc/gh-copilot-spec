# Microservice Audit Workflow

## Purpose
Workflow restrito para validação arquitetural visando checar interdependências acopladas erradas e garantência da SLA síncrona.

## Entry condition
Modificação agressiva nos imports raiz cruzando domínio de pastas locais ou criação de Rotas HTTP intra-rede novas.

## Roles involved
- Microservice Reviewer Agent

## Ordered steps
1. Avaliar os diagramas ou os arquivos alterados do core.
2. Identificar se um Micro-serviço está invadindo Tabela relacional ou Collection exclusiva de Módulo Irmão.
3. Reprovar ou aprovar o acoplamento indicando padrão de Eventos em Mensageria na recusa.

## Quality checks
- Impede firmemente falhas arquiteturais monóliticas passantes.
