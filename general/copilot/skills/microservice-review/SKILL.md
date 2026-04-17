# Microservice Review

## Purpose
Fiscalizar e garantir a fronteira lógica autônoma das unidades e módulos da arquitetura para que sistemas não sucumbam e virem monólitos distribuídos ou teias de dependência inquebráveis.

## When to use
- Apenas nos reviews arquiteturais maiores logo após ou em conjunto ao *Spec Authoring* visando a nova topologia dos módulos. 

## When not to use
- Nas checagens de linters em código síncrono da regra de negócio da aplicação que não chama terceiros.

## Expected inputs
- Artefatos (Spec e Plan).
- Diagrama explicído ou implicitado pelas portas mapeadas na API.

## Operating steps
1. Avalie Pontos de Quebra. Módulo "Locação" trava se o "Faturamento" cair? Evite HTTPs de leitura impiedosos síncronos se possível.
2. Contrato da API. A alteração afeta ou re-organiza DTOs retro-compatíveis velhos destruindo *clients* ou Mobile apps rodando soltos por versões abertas antigas?
3. Dados não-compartilhados. Garanta e sugira Event-broker/Kafka onde se esbarra em Joins perigosos.

## Quality bar
Qualquer dependência estrita forçando travamento duro sem Retry isolado em requisições rede "em corrente" deve ser alarmado prontamente.

## Expected outputs
Levantamento e veto construtivo de design mostrando pontos críticos.

## Common failure modes
- Tolerância ao uso de conexões SQL num repositório a partir de credenciais ou rotas destinadas a sub-contextos alheios.

## Minimal checklist
- [ ] Existe separação lógica física dos bancos do domínio avaliado?
- [ ] O serviço é resiliente se todos os downstream connections pifarem no momento?
