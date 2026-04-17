# TypeScript Service Instructions

## Postura Geral
Serviços em TS devem aproveitar o compilador em 100%. Use tipos estáticos rigorosos, evite `any` radicalmente e favoreça imutabilidade.

## Regras
1. **Sem "Any"**: Utilize `unknown` se não souber o que entra, realizando Type Guards robustos depois (ex. bibliotecas como Zod).
2. **Domain-Driven Context**: Toda lógica de aplicação fica fora dos controllers HTTP. Controladores não salvam coisas em banco de dados; instanciam "use cases" que orquestram a ação.
3. **Tratamento de Exceções**: Retorne falhas na assinatura sempre que possível (Result pattern, e.g. `type Result<T, E> = ...`) em vez de simplesmente lançar Throws assustadores para cima, dificultando rastreio. Se lançar Throwable padrão do Node, faça uso de classes instanciáveis legíveis como `NotFoundError`.
4. **Injeção de Dependências**: Sem hard-coded classes usando o `new Repository` em todo canto. Aceite as dependências no constructor para que os testes fiquem simples.
