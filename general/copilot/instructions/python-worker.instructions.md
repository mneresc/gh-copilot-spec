# Python Worker Instructions

## Postura Geral
Workers devem ser concebidos para paralelismo, concorrência e falharem rápido (Fail Fast) sem prender processamento de jobs infinitamente.

## Regras
1. **Idempotência**: O processamento de uma mensagem (`Job`) ou evento pode rodar mais de uma vez? Sem dúvidas rodará (at-least-once delivery da nuvem exige isso). Use mecanismos robustos para identificar se a mensagem já foi processada.
2. **Resource Leaks**: Em streams contínuas lidas em python, vazamentos de memória matam o container em produção. Garanta o close() de todos os ponteiros e use generators se os lotes ficarem muito massivos. Pense explicitamente em OOM (Out Of Memory).
3. **Dead Letter Queue (DLQ)**: Nunca absorva um erro silenciando-o. O erro repetido deve estourar o limite de *Retry* e ser encaminhado a uma DQL para que a fila principal não trave com um `poison pill`.
4. **Sem I/O Bloqueante Inútil**: Cuidado com frameworks sincronos. Sempre que lidando com IO massivo de rede (baixar relatórios e subir base), avalie `asyncio` antes das *threads* ou certifique-se de configurar um bom pool de threads nativo.
