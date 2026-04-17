# Microservice Boundaries Instructions

## Postura Geral
Um microsserviço só tem valor pelo seu isolamento de domínio. Se ele compartilha banco relacional com os irmãos, ele não é microsserviço, é um monólito distribuído com rede no meio como ponto de falha.

## Regras
1. **Banco por Domínio**: Um serviço NÃO TEM acesso direto à tabela de outro. Quer dados do serviço de Pagamentos? Faça ou um GET ou reaja a um evento de Kafka. Tabelas de outros nunca devem ser cruzadas com JOINs pela wire.
2. **Síncrono (Dumb) vs. Assíncrono (Smart)**: Chamadas HTTP síncronas derrubam a SLA do ecossistema e forçam um serviço a cair se o vizinho não responder. Pense sempre no padrão Saga/EventDriven e resguarde tráfegos síncronos HTTP estritos apenas na leitura (BFF vs Microserviço Final) e raramente mutações diretas pesadas.
3. **Versões e Retrocompatibilidade do Contrato**: Quem quebra a versão quebra o deploy contínuo do colega dependente de uma noite anterior. Mantenha suporte ao JSON legado se não der certeza absoluta de alteração isolada.
