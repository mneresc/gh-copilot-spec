# TypeScript Local Instructions

## Stack Autorizada
- Runtime: Node `v20.x`
- Framework: NestJS
- ORM: Prisma ORM
- Linter: ESLint + Prettier (Flat config local)

## Convenções Locais
- Interfaces devem começar sem o `I` prefixado (Ex: `UserService`, não `IUserService`).
- Data Transfer Objects (DTO) devem terminar com `Dto` (Ex: `CreateRentDto`) e possuir decorators do `class-validator`.
- Falhas lógicas de negócio devem invocar `BadRequestException` nativa do Nest encapsulando os domínios. 
- Controllers NUNCA devem chamar `@Inject(PrismaService)`. Quem orquestra I/O é a camada Service.
