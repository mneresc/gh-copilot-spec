# Workflow Enxuto VSCode + Copilot + Claude

## 1. Setup inicial no VSCode + Copilot

**Configurações recomendadas do `settings.json`:**
- `github.copilot.chat.localeOverride`: "pt-BR"
- `github.copilot.chat.agent.enabled`: true (ativa agent mode)
- `github.copilot.chat.codeGeneration.useInstructionFiles`: true

**Atalhos essenciais:**

| Atalho | Ação | Descrição |
|--------|------|-----------|
| `Ctrl+I` | Inline Chat | Edição rápida no arquivo aberto |
| `Ctrl+Shift+I` | Copilot Chat Panel | Conversas longas, planejamento |
| `Ctrl+Shift+Alt+I`| Agent Mode | Execução autônoma com ferramentas |
| `Ctrl+/` | Toggle Copilot | Ativar/desativar inline suggestions |

**Como trocar de modelo:** 
No Chat Panel, clique no nome do modelo no topo para selecionar Haiku/Sonnet/Opus. Indique a troca ANTES de enviar o prompt.

**`copilot-instructions.md`:** 
Utilize o [template base](../repo-specific/templates/.github/copilot-instructions.md). Ele representa a "verdade local" do repositório.

## 2. Matriz de modelo por fase e tipo de tarefa

| Tipo de tarefa | Plan | Implement | Review |
|----------------|------|-----------|--------|
| Tarefa trivial | — | **Haiku** | — |
| Bug fix simples | Sonnet | Sonnet | self (Sonnet) |
| Feature pequena | Sonnet | Sonnet | self (Sonnet) |
| Refactor | — | Sonnet | — |
| CRUD simples | — | **Haiku** | — |
| UI/cosmética | — | **Haiku**/Sonnet | — |
| Design ambíguo | **Opus** | Sonnet | Sonnet |
| Bug crítico/sensível | **Opus** | Sonnet | **Opus** |

**Justificativas:**
- **Trivial/CRUD com Haiku**: Tarefa mecânica, bem definida, sem ambiguidade. Haiku é 5x mais barato e 3x mais rápido.
- **Bug fix com Sonnet full-cycle**: Precisa raciocinar sobre causa-raiz e efeitos colaterais. Haiku erra diagnósticos.
- **Feature pequena com Sonnet**: Equilíbrio — precisa entender contexto de negócio mas não exige raciocínio arquitetural profundo.
- **Refactor sem Plan/Review formal**: Se os testes já passam, o refactor é mecânico. Sonnet aplica e roda testes.
- **Design ambíguo com Opus no Plan**: Decisões de arquitetura erradas custam caro. Opus pensa melhor sobre tradeoffs.
- **Bug sensível com Opus nos extremos**: Plan (entender superfície de ataque) e Review (validar que não abriu brechas) exigem raciocínio profundo. Implement fica em Sonnet porque é execução.

## 3. Templates de prompt

Veja os exemplos na pasta `prompts/`.
- [Tarefa trivial](prompts/trivial.md)
- [Bug fix](prompts/bugfix.md)
- [Feature pequena](prompts/feature-pequena.md)
- [Refactor](prompts/refactor.md)
- [Design exploration](prompts/design-exploration.md)
- [Bug em código sensível](prompts/bugfix-sensivel.md)

## 4. Heurística de decisão

**Qual modelo para começar?**
| Pergunta | Se sim → | Se não → |
|----------|---------|---------|
| Sei exatamente o que fazer e é <20 linhas? | Haiku | ↓ |
| É implementação padrão sem decisão de design? | Sonnet | ↓ |
| Envolve tradeoffs, arquitetura ou segurança? | Opus | Sonnet (default) |

**Quando trocar de modelo no meio?**
- Começou em Haiku e a tarefa revelou ambiguidade → Sonnet
- Começou em Sonnet e o Claude está dando respostas rasas/erradas sobre arquitetura → Opus
- Começou em Opus e percebeu que é implementação mecânica → Sonnet

**Fluxo enxuto vs estruturado?**
| Pergunta | Se sim → Estruturado |
|----------|---------------------|
| Toca mais de 3 serviços/módulos? | Sim |
| Precisa de alinhamento com PM/stakeholder? | Sim |
| Mudança irreversível (migração, schema)? | Sim |
| Estimativa >4h de trabalho? | Sim |
| Tudo "não"? | Fique no enxuto |

**Quando escrever artefatos em disco?**
| Artefato | Quando vale |
|----------|-----------|
| `PLAN.md` | >2 milestones ou >1 dia de trabalho |
| `BDD.md` / `.feature` | Comportamento novo que precisa de aceite |
| `ADR.md` | Decisão arquitetural que outros devs precisam entender |
| Nenhum | Tarefas triviais, bug fixes, refactors simples |

## 5. Sinais para trocar de modelo

**Subir: Sonnet → Opus**
- Claude está gerando código que não compila ou tem bugs lógicos em algo não-trivial
- Pediu para analisar tradeoffs e recebeu resposta superficial ("depende do caso")
- Bug que envolve concorrência, race conditions, ou interações entre serviços
- Precisa entender implicações de segurança de uma mudança

**Descer: Opus → Sonnet**
- O plan já está definido e agora é só implementar
- Está esperando muito pela resposta e a tarefa não justifica
- O output é código mecânico (CRUD, boilerplate, testes padrão)

**Descer: Sonnet → Haiku**
- Renomear variáveis, mover arquivos, ajustar imports
- Gerar boilerplate repetitivo (DTOs, mappers)
- Corrigir typos, ajustar CSS, trocar constantes
- Tarefa que você conseguiria descrever em 1 frase sem ambiguidade

## 6. Anti-padrões

1. **"Opus para tudo"** — Usar Opus em toda tarefa por "segurança". Desperdiça dinheiro e tempo. Correção: comece no Sonnet, suba se necessário.
2. **"Haiku arquiteto"** — Pedir decisões de design para Haiku. Ele não tem raciocínio profundo suficiente. Correção: Haiku só para tarefas mecânicas sem ambiguidade.
3. **"Contexto infinito"** — Jogar o repo inteiro no chat e esperar mágica. O modelo se perde. Correção: abra só os arquivos relevantes, descreva o escopo em 3 linhas.
4. **"Sem self-review"** — Aceitar o primeiro output sem pedir revisão. Correção: inclua "Self-review: [critérios]" no fim do prompt.
5. **"Prompt vago"** — "Arruma esse bug" sem contexto. Correção: sempre inclua: sintoma, onde ocorre, comportamento esperado.
6. **"Pular testes"** — Implementar sem pedir testes. Correção: inclua "Escreva um teste que reproduza" no prompt. Use skills `unit-test-ts`/`unit-test-python`.
7. **"Refactor oportunista"** — Pedir fix e aceitar refactor não solicitado junto. Correção: exija "fix mínimo" no prompt. Referência: skill `incident-hotfix-review` (princípio do menor fix).

## 7. Rotina de manutenção

**Quando atualizar `copilot-instructions.md`:**
- A cada novo serviço/módulo adicionado ao repo
- Quando trocar de framework de teste ou ORM
- Quando um padrão novo se estabelecer (ex: "sempre use Zod v4")
- Revisão trimestral mínima

**Como capturar regras recorrentes:**
- Se você corrigiu o Claude 3+ vezes sobre a mesma coisa → vira regra no `copilot-instructions.md`
- Exemplos: "Nunca use console.log, use o logger injetado", "DTOs sempre com Zod schema"

**Sinais de que o fluxo enxuto não basta:**
- Mais de 3 pessoas precisam revisar/aprovar a mudança
- Você está no 3º "ah, mas também precisa mudar X" na mesma sessão
- A sessão do chat passou de 20+ mensagens sem convergir
- Escalação: usar `feature-spec-driven.md` workflow do repositório
