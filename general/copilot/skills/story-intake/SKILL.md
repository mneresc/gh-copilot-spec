# Story Intake

## Purpose
Receber as primeiras descrições soltas ou histórias de negócio puras (ex: ingressos JIRA ou card de chat) e digeri-las em um entendimento técnico bruto antes de qualquer corte ou código.

## When to use
- Quando uma nova demanda bruta entra sem contexto arquitetural ou documentação.
- O cenário tem texto confuso pedindo "faça algo como X mas no sistema Y".

## When not to use
- Bug fix com erro rastreável já identificado (Stacktrace).
- Ajustes finos ou refatorações técnicas.

## Expected inputs
- Histórico do Chat do Produto com Product Manager.
- O título provisório da Feature.
- Entendimento prévio do negócio local.

## Operating steps
1. Ingira passivamente os inputs.
2. Identifique os atores principais na história proposta.
3. Elenque os possíveis cenários não ditos (falhas obvias que o negócio não previu, timeouts, rejeições).
4. Elabore um sumário reflexivo perguntando *"Minha interpretação bate com a sua?"* para a aprovação humana.

## Quality bar
A saída deve possuir linguagem profissional neutra, evitando o excesso de "sim, eu entendi". Deve devolver o entendimento digerido em tópicos de maneira sucinta.

## Expected outputs
Um entendimento cru validado contendo escopo geral sobre as partes limitadoras que formaram o artefato.

## Common failure modes
- A IA assumir stack ou detalhamento no primeiro input já escrevendo código de banco de dados por ansiedade (hallucinated forward-planning).

## Minimal checklist
- [ ] O ator do evento está claro?
- [ ] A condição de parada/finalização do desejo ou história existe?

## Stack-specific notes
Nenhum. Agnosticismo puro em relação à linguagem.
