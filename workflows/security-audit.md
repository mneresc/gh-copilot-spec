# Security Audit Workflow

## Purpose
Ponto de controle crítico focando a checagem manual/sistematizada nos riscos topológicos de segurança do Pull Request e das rotas.

## Entry condition
- Código escrito, tests completados, e pacote se prepara para PR ou deploy contínuo em ambientes limpos.

## Roles involved
- Security Auditor.

## Ordered steps
1. Odiff é exposto para a IA assumindo o *Security Auditor*.
2. IA invoca a Skill *security-review*, avaliando hardcoded secrets, PII, e Autoralidade faltante.
3. Se um achado vermelho for pego, o dev retorna pro "Pair Implement" focado na correção.

## Escalation conditions
- Descobertas do Security Auditor envolvendo falha estrutural global que força re-escrever o banco de dados. Isso encerra o Sprint do dev e abre ticket alto nível pro arquiteto (Solution Architect).
