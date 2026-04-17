# BDD & Critérios de Aceite

Referência Base: docs/FEATURE_SPEC.md

## Cenário 1: Devolução com Atraso Maior que 2 Horas (Happy Path Multa)
**Given** que a reserva de carro "XYZ-192" do usuário ID 99 previa entrega para 2024-05-10 12:00
**Given** as chaves de autorização de admin ou sistema batch de varredura ativa.
**When** a devolução for efetivada em 2024-05-10 15:30
**Then** uma multa de 5% sobre a diária somado ao valor de 1 diária cheia extra deve ser invocada na fatura 
**And** um evento "LateReturn_Charged" deve ser atirado no Kafka.

## Cenário 2: Falso Positivo (Tolerância Contratual)
**Given** a mesma reserva
**When** a devolução for efetivada em 2024-05-10 13:30 (dentro da janela de 2h de tolerância)
**Then** não invocar multas operacionais
**And** fechar o contrato normalmente como "On-Time".

## Cenário 3: Falha de Autorização (IDOR bypass)
**Given** uma reserva alheia
**When** tentarem invocar o hook POST `/returns/charge` interceptando uma chamada externa ou sem token System.
**Then** retornar HTTP 403 (Zero-trust) bloqueando a injeção falsa de multas por front-end.
