# Python Worker Implementation

## Purpose
Guiar a mão que desenvolve scripts massivos de backend assíncronos e sub-processos contínuos focada puramente na entrega em Python.

## When to use
- Quando a sprint aloca a IA e o desenvolvedor pair-to-pair no desenvolvimento do Milestone ditado num ambiente isolado ou modulo workers.

## When not to use
- Se a alteração não for nos workers ou for num BFF Typecript.

## Expected inputs
- O Milestone atual em `PLAN.md`.
- As especificidades indicadas na `copilot-instructions.md`.

## Operating steps
1. Avaliar os blocos try/except: Nunca apagar e omitir a pilha de exceções, usar explicitamente o padrão de Log descrito universalmente.
2. Escrever a feature respeitando pep-8 e typing estrito (Hints, TypeDicts, Pydantic, etc, validado na infra local).
3. Verificar laços iteradores infinitos ou streams de mensageiria e avaliar vazamentos e fechamento the scopes.

## Quality bar
Código idiomático sem traços de importações circulares e tipado formalmente, livre de `eval()` ou comportamentos de mágic stringing sem consts definidas.

## Expected outputs
Criação dos módulos em Python para resolver a demanda focada num Worker.

## Common failure modes
- Absorção de mensagens na cloud por erro mascarado no catch;
- Ausência total do pattern de dependecy inversion com decorators/di frameworks.

## Minimal checklist
- [ ] Implementou o retry pattern coerente pedido na spec da história?
- [ ] Passou pelo linters com checadores estáticos do projeto na revisão interna?

## Stack-specific notes
Respeitar a instrução `python-worker.instructions.md`.
