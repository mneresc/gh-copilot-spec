# Unit Test Python

## Purpose
Abordar o controle de qualidade do test runner python escolhido localmente garantindo a execução de testes rápidos de funções e classes em workers Python.

## When to use
- Antes de commitar lógicas e logo apoś o fechamento do slice.

## When not to use
- Se a função é apenas um wrapper vazio em volta de uma chamada HTTP puríssima de SDK de terceiro sem inteligência de domínio embutida localmente a não ser configuração (delegado ao e2e).

## Expected inputs
- Arquivo Python editado.
- Instruções de stack de teste (unittest / pytest / fixtures).

## Operating steps
1. Use `unittest.mock` ou Pytest Fixtures e `monkeypatch` baseando-se no framework local ditado em `copilot-instructions.md`.
2. Se construindo mocks de injeção em generators assíncronos, utilize mock libs robustos específicos para async/await (`AsyncMock`).
3. Monte as suites seguindo o "Given-When-Then" técnico sem cruzar chamadas externas I/O.

## Quality bar
Uso impecável dos asserts específicos do runner. Exemplo: Não use `assert a == b` falhando de forma cega sem dump se o framework oferecer diferenciais melhores de checagem profunda entre dois dicionários/listas.

## Expected outputs
Módulos `test_*.py` limpos para a execução do `pytest/python -m unittest`.

## Common failure modes
- Escrever testes síncronos sobre chamadas Asyncio que acabam rodando sem trigger ou congelam porque os event loops não foram mockados direito.

## Minimal checklist
- [ ] O runner (Pytest/Unittest) está validado como a verdade do framework local do Repositório?
- [ ] As fixtures (se houverem) encerram recursos vazados?

## Stack-specific notes
Não imponha ou assuma um padrão único. Adapte caso a arquitetura use apenas lib clássica enraizando `unittest.TestCase` ou injeções livres de `pytest`.
