# API Contract Review

## Purpose
Analisar as documentações OpenApi (Swaggers, Graphql Schema) cruzando a implementação gerada contra a especificação pré ou pós-aceita visualizando quebras contratuais externas.

## When to use
- Antes de commitar `Controllers` ou `Routers`. Em etapas de PR Readiness para contratos públicos ou SDKs internos.

## When not to use
- Quando o serviço consome filas de topicos sem endpoints expostos externos, usando DTOs internos efêmeros do worker puros.

## Expected inputs
- Contrado ou FEATURE_SPEC.md.
- `.ts/py/openapi` arquivos das rotas.

## Operating steps
1. Compare os `inputs/payload` recebidos: os Tipos mudaram de `string` para `integer` de forma opcional sem versão?
2. Compare os `outputs`: A exclusão de um campo de status vital fará o front-end crashar (Breaking Change)?
3. Observe falhas de rest-design: POST retornando listas nativas ao invez de `{ data: [] }` que impede futuras agregações.

## Quality bar
Zero quebras de contrato de APIs marcadas como Estáveis. Tolerância a deprecations apenas.

## Expected outputs
Relatório binário de Passou/Ou/Quebrou no teste visual do contrato.

## Common failure modes
- Omitir "required properties" nos Schemas que podem explodir validações de null.

## Minimal checklist
- [ ] Breaking changes em respostas da V1 não existem?
- [ ] A API modela estado e não "Ações do Backend"? (Ex: POST /reservations preferido contra POST /saveReservation)
