# Python Local Instructions

## Stack Autorizada
- Runtime: Python 3.11+
- Framework Web: FastAPI
- Dependências: Poetry ou uv
- Linter: Ruff (Format + Lint)

## Convenções Locais
- Digitação Estrita: Usar Type Hints em 100% das assinaturas. Ferramenta Mypy rola ativada na pipeline (strict=True).
- Pydantic: Toda Request/Response passa forçosamente por modelos Pydantic v2.
- Workers (Assíncrono): Lógicas I/O devem possuir marcação `async def` para rodar no event loop do Uvicorn e liberá-lo.
