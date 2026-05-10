# Matriz de Modelos — Referência Rápida

## Por Tipo de Tarefa × Fase

| Tipo de tarefa | Plan | Implement | Review |
|----------------|:----:|:---------:|:------:|
| Tarefa trivial | — | **Haiku** | — |
| Bug fix simples | Sonnet | Sonnet | Sonnet |
| Feature pequena | Sonnet | Sonnet | Sonnet |
| Refactor | — | Sonnet | — |
| CRUD simples | — | **Haiku** | — |
| UI / cosmética | — | **Haiku** | — |
| Design ambíguo | **Opus** | Sonnet | Sonnet |
| Bug sensível | **Opus** | Sonnet | **Opus** |

## Perfil dos Modelos

| | Haiku | Sonnet | Opus |
|--|:-----:|:------:|:----:|
| **Custo** | 1× | ~3× | ~5× |
| **Latência** | ⚡ Baixa | ⏱ Média | 🐢 Alta |
| **Força** | Mecânico | Equilíbrio | Raciocínio |
| **Usar para** | Boilerplate, renames, CSS | Default do dia a dia | Arquitetura, segurança |
| **Não usar para** | Decisões de design | — | Tarefas triviais |
