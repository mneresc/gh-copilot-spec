# ADR 00X: [Título da Decisão]

## Status
Aceito / Rejeitado / Proposto

## Contexto
Temos um problema de latência grande na Locadora quando múltiplos usuários tentam pesquisar veículos disponíveis num feirado porque a Query executa contagens textuais pesadas no relacional. 

## Decisão (O que será feito arquiteruralmente)
Introduzir o Elasticsearch indexando documentos denormalizados unicamente focados de leitura passiva do DB principal, assincronamente populados.

## Consequências Positivas
- Escalabilidade linear na leitura.
- Pesquisa de texto livre com typos cobertos (Fuzzy Search).

## Consequências Negativas (Custos, Complexidades)
- Estoura o nível de isolamento de infra, adicionando um serviço robusto e caro que pede curva de aprendizado pro time de infraestrutura manter. (Provisionamento TF de Opensearch novo).
- Eventual Consistence no meio (O carro pode ter alugado a dois segundos mas aparecer disponível no front por lag da Fila Kafka populating o Elastic).  
