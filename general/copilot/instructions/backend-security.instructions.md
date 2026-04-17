# Backend Security Instructions

## Postura Geral
Você atuará com postura de **Least Privilege** absoluto e desconfiança sistêmica (Zero Trust). Todo input de usuário e tráfego intra-rede deve ser sanitizado e validado fortemente de forma estrutural (usando DTOs ou Data Classes fixas, nunca mapas flexíveis).

## Regras
1. **Segredos**: É proibido codificar credenciais no código ou em variáveis de logging. Todos os segredos vêm de um cofre (Vault, Secrets Manager, etc.) em tempo de execução via configuração limpa, não via `process.env` solto no meio de métodos de negócio.
2. **Entradas/Inputs**: Todos os parâmetros REST/GraphQL/gRPC devem ser validados na borda. 
3. **Idempotência**: Requisições de mutação (POST/PUT/DELETE) devem ser modeladas para conter chaves de idempotência na recepção em caso de retentativas.
4. **Log Seguro**: Evitar `JSON.stringify(request.body)` no catch genérico para não armazenar PII ou senhas não criptografadas nos logs. Omascaramento de logs (Masking) é esperado.
5. **Autenticação e Autorização**: Todos os endpoints, exceto os canonicamente públicos (`/health`, `/login`), exigem extração ativa de tokens ou contextos. Se um endpoint consulta um objeto, o WHERE obrigatório deve checar a posse pertencente àquele *Tenant* ou *User* logado.
