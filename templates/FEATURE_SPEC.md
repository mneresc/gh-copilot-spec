# Feature Spec: [Título da Feature]

**Status**: Rascunho / Aprovado
**Data**: YYYY-MM-DD
**Autor Responsável (PM/Arquiteto)**: 

## 1. Motivação do Negócio
*(Ex: O cliente não devolve os veículos no prazo na locadora porque a penalidade administrativa é apenas cobrada em balcão).*

## 2. Escopo
- O que o sistema vai fazer: *(Gerar cobrança no cartão cadastrado caso atrase 24h).*
- Módulos Impactados: *(Billing, Reservation).*

## 3. Fora de Escopo
*(Isso é o mais importante. Liste o que NÂO será feito sob pena de auditoria repulsiva).*
- Não faremos bloqueio do carro via satélite (Telemetria) nesta feature.
- Não iremos ligar automaticamente pro cliente. Envio via e-mail e pushNotification bastam.

## 4. Contratos Inter-Serviços
O Microsserviço de Reservas vai mandar um evento assíncrono para o de Cobranças.
- Payload sugerida: `{ eventId: UUID, rentId: string, dalayDays: int }`

## 5. Riscos Abertos e Mitigações
- Risco: O cartão na devolução pode estar bloqueado ou expirar (Chargeback).
- Mitigação: Em caso de erro na cobrança automática geramos um titulo pendente bloqueando faturamento do CPF no Serasa. 

## 6. Critérios de Conclusão (Boilerplate)
- [ ] BDD escrito e aceito.
- [ ] Implementação de Testes E2E sem falsos positivos.
- [ ] Logs sensíveis prevenidos.
