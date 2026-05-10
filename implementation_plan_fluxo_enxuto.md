# Plano de Execução — Workflow Enxuto VSCode + Copilot + Claude

> **INSTRUÇÃO**: Este documento é auto-contido. Uma IA com menor capacidade computacional deve conseguir criar todos os arquivos listados seguindo as especificações abaixo. NÃO improvise conteúdo — siga o plano.

---

## PARTE A — CONTEXTO DO USUÁRIO (preservar integralmente nos outputs)

### Filosofia
- **Plan, Implement, Review** num único contexto/sessão sempre que possível
- Velocidade sobre cerimônia para tarefas pequenas
- Humano é reviewer principal; Claude faz self-review embutido
- Cerimônia maior só para features médias ou código sensível
- **Modelo certo para a tarefa certa** — seleção de modelo é decisão de primeira classe

### Modelos disponíveis
| Modelo | Perfil | Custo | Latência |
|--------|--------|-------|----------|
| **Haiku 4.5** | Tarefas mecânicas, bem definidas | 1x | Baixa |
| **Sonnet 4.6** | Default, equilíbrio custo/qualidade | ~3x | Média |
| **Opus 4.7** | Raciocínio profundo, arquitetura, debug complexo | ~5x | Alta |

### Tipos de tarefa do dia a dia
1. Bug fixes (<100 linhas)
2. Features pequenas (1-2 cenários)
3. Refactors localizados
4. Ajustes de UI/cosmética
5. CRUD simples
6. Tarefas triviais (renomear, mover)
7. Bug em código sensível (auth, pagamentos, dados)

### Restrições
- Sem frameworks pesados (BMAD, GSD, Ralph Loop)
- Tudo dentro do Copilot Chat no VSCode (inline + panel + agent mode)
- Foco em prompting, configuração e seleção de modelo
- Português para textos; inglês para prompts técnicos quando natural
- Templates em blocos de código copiáveis
- Modelo indicado com formato `[Modelo: X]`

---

## PARTE B — INVENTÁRIO DO REPOSITÓRIO EXISTENTE

### Repositório base
`d:\marcelo\Documents\0_projetos\01_desenvolvimento\instructions\`

### Skills existentes (pasta `general/copilot/skills/`)
19 skills, cada uma com `SKILL.md`. Relevantes para o fluxo enxuto:

| Skill | Uso enxuto | Modelo sugerido |
|-------|-----------|-----------------|
| `story-intake` | Feature pequena (Plan) | Sonnet |
| `slice-scoping` | Feature pequena+ (Scoping) | Sonnet/Opus |
| `bdd-scenario-authoring` | Feature pequena (BDD) | Sonnet |
| `ts-service-implementation` | Implementação TS | Sonnet |
| `python-worker-implementation` | Implementação Python | Sonnet |
| `unit-test-ts` | Bug fix, feature, refactor | Sonnet/Haiku |
| `unit-test-python` | Bug fix, feature, refactor | Sonnet/Haiku |
| `incident-hotfix-review` | Bug crítico | Opus |
| `security-review` | Código sensível | Opus |

Skills do fluxo completo (referência de escalação): `spec-authoring`, `test-plan-authoring`, `execution-planning`, `spec-audit`.

### Agents existentes (pasta `general/copilot/agents/`)
| Agent | Modelo ideal | Uso enxuto |
|-------|-------------|-----------|
| `@pair-engineer-ts` / `@pair-engineer-python` | Sonnet | Implementação |
| `@test-auditor` | Sonnet | Self-review testes |
| `@pm-bdd-manager` | Sonnet/Opus | Feature pequena |
| `@security-auditor` | Opus | Código sensível |

### Workflows existentes (pasta `workflows/`)
| Workflow | Enxuto? |
|----------|---------|
| `pair-implement.md` | Sim |
| `unit-test-create.md` | Sim |
| `incident-hotfix-audit.md` | Sim |
| `story-to-bdd.md` | Parcial |
| `feature-spec-driven.md` | Não (escalação) |

### Template existente
`repo-specific/templates/.github/copilot-instructions.md` — template com placeholders para verdade local do repositório.

---

## PARTE C — ARQUIVOS A CRIAR

### Estrutura final
```text
examples/
├── README.md
├── claude-copilot-workflow.md
├── prompts/
│   ├── trivial.md
│   ├── bugfix.md
│   ├── feature-pequena.md
│   ├── refactor.md
│   ├── design-exploration.md
│   └── bugfix-sensivel.md
└── copilot-instructions-example.md
```

Todos os arquivos devem ser criados em:
`d:\marcelo\Documents\0_projetos\01_desenvolvimento\instructions\examples\`

---

## PARTE D — ESPECIFICAÇÃO DE CADA ARQUIVO

### Arquivo 1: `examples/claude-copilot-workflow.md`

Este é o **entregável principal**. Documento com 7 seções. Abaixo o conteúdo EXATO esperado em cada seção:

#### Seção 1: Setup inicial no VSCode + Copilot
Deve cobrir:
- **Configurações recomendadas** do `settings.json`:
  - `github.copilot.chat.localeOverride`: "pt-BR"
  - `github.copilot.chat.agent.enabled`: true (ativa agent mode)
  - `github.copilot.chat.codeGeneration.useInstructionFiles`: true
- **Atalhos essenciais** (tabela):
  - `Ctrl+I` → Inline Chat (edição rápida no arquivo aberto)
  - `Ctrl+Shift+I` → Copilot Chat Panel (conversas longas, planejamento)
  - `Ctrl+Shift+Alt+I` → Agent Mode (execução autônoma com ferramentas)
  - `Ctrl+/` → Toggle Copilot inline suggestions
- **Como trocar de modelo**: No Chat Panel, clicar no nome do modelo no topo para selecionar Haiku/Sonnet/Opus. Indicar que deve trocar ANTES de enviar o prompt.
- **`copilot-instructions.md`**: Linkar para `repo-specific/templates/.github/copilot-instructions.md` como template base. Explicar que é a "verdade local" do repositório.

#### Seção 2: Matriz de modelo por fase e tipo de tarefa
Tabela principal:

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

Justificativas (uma linha cada):
- **Trivial/CRUD com Haiku**: Tarefa mecânica, bem definida, sem ambiguidade. Haiku é 5x mais barato e 3x mais rápido.
- **Bug fix com Sonnet full-cycle**: Precisa raciocinar sobre causa-raiz e efeitos colaterais. Haiku erra diagnósticos.
- **Feature pequena com Sonnet**: Equilíbrio — precisa entender contexto de negócio mas não exige raciocínio arquitetural profundo.
- **Refactor sem Plan/Review formal**: Se os testes já passam, o refactor é mecânico. Sonnet aplica e roda testes.
- **Design ambíguo com Opus no Plan**: Decisões de arquitetura erradas custam caro. Opus pensa melhor sobre tradeoffs.
- **Bug sensível com Opus nos extremos**: Plan (entender superfície de ataque) e Review (validar que não abriu brechas) exigem raciocínio profundo. Implement fica em Sonnet porque é execução.

#### Seção 3: Templates de prompt (6 tipos)
Cada template deve:
- Ter indicação `[Modelo: X]` no topo
- Estar em bloco de código markdown (copiável)
- Referenciar skills existentes quando aplicável
- Incluir guards e self-review conforme o tipo

**Template 1 — Tarefa trivial**
```
[Modelo: Haiku]
Renomeie/mova/ajuste [descreva o que]. Mantenha as importações consistentes.
```
Sem plan, sem review. Haiku resolve.

**Template 2 — Bug fix**
```
[Modelo: Sonnet]
Bug: [descreva sintoma e onde ocorre]

1. Analise a causa raiz
2. Escreva um teste que reproduza o bug (red)
3. Implemente o fix mínimo para o teste passar (green)
4. Self-review: verifique se o fix não quebra comportamentos adjacentes
   e se o teste realmente falharia sem o fix
```
Referencia skills: `unit-test-ts` ou `unit-test-python`.

**Template 3 — Feature pequena**
```
[Modelo: Sonnet]
Feature: [descreva o comportamento desejado em 1-2 frases]

1. Mini-plan: liste os arquivos afetados e a abordagem em bullets
   (máx 5 bullets). Don't implement yet — espere meu OK.
2. [após OK] Implemente seguindo o plan. Inclua testes para os
   cenários: [cenário feliz] e [cenário de erro].
3. Self-review: revise se o código segue os padrões de
   copilot-instructions.md e se não há lógica fora do escopo pedido.
```
Se o mini-plan revelar complexidade → trocar para Opus no plan.
Referencia skills: `bdd-scenario-authoring` (se quiser formalizar cenários), `ts-service-implementation` ou `python-worker-implementation`.

**Template 4 — Refactor**
```
[Modelo: Sonnet]
Refactor: [descreva o que e por que]

1. Confirme que os testes existentes passam antes de começar
2. Aplique o refactor mantendo a interface pública inalterada
3. Rode os testes — se algum quebrar, corrija o refactor, não o teste
4. Self-review: o diff é menor que ~200 linhas? Se não, quebre em etapas.
```

**Template 5 — Design exploration (planejamento puro)**
```
[Modelo: Opus]
Preciso decidir como implementar: [descreva o problema/decisão]

Contexto: [stack, restrições, o que já existe]

**DON'T IMPLEMENT YET.** Apenas:
1. Liste 2-3 abordagens possíveis com prós/contras
2. Recomende uma com justificativa
3. Identifique riscos e o que ficaria fora de escopo
4. Indique quais arquivos seriam afetados

Formato: bullets curtos, sem código.
```
Guard explícito. Opus porque decisões de design erradas custam mais que o preço do modelo.

**Template 6 — Bug em código sensível**
```
[Modelo: Opus]
Bug em área sensível ([auth/pagamentos/dados]):
[descreva o bug e o impacto potencial]

1. Analise a superfície de risco: que dados/fluxos podem ser
   afetados por esse bug?
2. DON'T IMPLEMENT YET — espere meu OK no diagnóstico.

[após OK, trocar para Modelo: Sonnet]
3. Implemente o fix mínimo com teste
4. [trocar para Modelo: Opus]
5. Review de segurança: verifique se o fix não abre novas
   superfícies de ataque, se sanitiza inputs corretamente,
   e se os logs não vazam dados sensíveis.
```
Referencia skills: `incident-hotfix-review`, `security-review`.
Referencia agent: `@security-auditor` para review.

#### Seção 4: Heurística de decisão (1 página)
Tabela de decisão rápida (30 segundos):

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

#### Seção 5: Sinais para trocar de modelo
Três listas concretas:

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

#### Seção 6: Anti-padrões (7 itens)
Cada item: nome, descrição, correção.

1. **"Opus para tudo"** — Usar Opus em toda tarefa por "segurança". Desperdiça dinheiro e tempo. Correção: comece no Sonnet, suba se necessário.
2. **"Haiku arquiteto"** — Pedir decisões de design para Haiku. Ele não tem raciocínio profundo suficiente. Correção: Haiku só para tarefas mecânicas sem ambiguidade.
3. **"Contexto infinito"** — Jogar o repo inteiro no chat e esperar mágica. O modelo se perde. Correção: abra só os arquivos relevantes, descreva o escopo em 3 linhas.
4. **"Sem self-review"** — Aceitar o primeiro output sem pedir revisão. Correção: inclua "Self-review: [critérios]" no fim do prompt.
5. **"Prompt vago"** — "Arruma esse bug" sem contexto. Correção: sempre inclua: sintoma, onde ocorre, comportamento esperado.
6. **"Pular testes"** — Implementar sem pedir testes. Correção: inclua "Escreva um teste que reproduza" no prompt. Use skills `unit-test-ts`/`unit-test-python`.
7. **"Refactor oportunista"** — Pedir fix e aceitar refactor não solicitado junto. Correção: exija "fix mínimo" no prompt. Referência: skill `incident-hotfix-review` (princípio do menor fix).

#### Seção 7: Rotina de manutenção

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

### Arquivo 2: `examples/prompts/trivial.md`

Conteúdo: o Template 1 da Seção 3 acima com header explicativo:
- Título: "Prompt — Tarefa Trivial"
- Quando usar: renomear, mover, ajustar imports, trocar constantes, CSS pontual
- Modelo: Haiku 4.5
- Cerimônia: zero (sem plan, sem review)
- Skills relacionadas: nenhuma
- O prompt copiável
- Exemplo concreto de uso

### Arquivo 3: `examples/prompts/bugfix.md`
Template 2 com header. Modelo: Sonnet. Skills: `unit-test-ts`/`unit-test-python`. Workflow: `incident-hotfix-audit.md` se for P0/P1.

### Arquivo 4: `examples/prompts/feature-pequena.md`
Template 3 com header. Modelo: Sonnet (Opus se ambíguo no plan). Skills: `bdd-scenario-authoring`, `ts-service-implementation`/`python-worker-implementation`. Workflow: `pair-implement.md`.

### Arquivo 5: `examples/prompts/refactor.md`
Template 4 com header. Modelo: Sonnet. Skills: `unit-test-ts`/`unit-test-python`.

### Arquivo 6: `examples/prompts/design-exploration.md`
Template 5 com header. Modelo: Opus. Guard: DON'T IMPLEMENT YET. Skills: `slice-scoping`, `story-intake`.

### Arquivo 7: `examples/prompts/bugfix-sensivel.md`
Template 6 com header. Modelo: Opus→Sonnet→Opus. Skills: `incident-hotfix-review`, `security-review`. Agent: `@security-auditor`.

### Arquivo 8: `examples/copilot-instructions-example.md`
Versão preenchida do template `repo-specific/templates/.github/copilot-instructions.md` usando um projeto fictício "API de Gestão de Pedidos" com:
- Stack: Node.js + TypeScript + Express + Prisma + PostgreSQL
- Testes: Jest
- Logger: Pino
- Estrutura: `src/domain/`, `src/infrastructure/`, `src/api/`

### Arquivo 9: `examples/README.md`
Índice da pasta com:
- Propósito: "Exemplos práticos de uso das skills deste repositório num fluxo enxuto para VSCode + Copilot"
- Tabela com link para cada arquivo, descrição curta e modelo recomendado
- Seção "Quando usar o fluxo enxuto vs o fluxo completo" com referência ao fluxograma do README principal
- Link para o `claude-copilot-workflow.md` como ponto de entrada

---

## PARTE E — ALTERAÇÕES EM ARQUIVO EXISTENTE

### `README.md` principal (raiz do repo)

**Alteração 1**: Na seção 3 (árvore), adicionar:
```text
├── examples/               # Exemplos práticos e workflow enxuto para o dia a dia
```

**Alteração 2**: Na seção 8 (fluxograma), adicionar nó:
```mermaid
H -- Sim --> I[Não precisa de fluxo completo, use Pair-Implement Skill]
```
Substituir por:
```mermaid
H -- Sim --> I[Use o Fluxo Enxuto - veja examples/claude-copilot-workflow.md]
```

**Alteração 3**: Nova seção 11 após a seção 10:
```markdown
## 11. Fluxo Enxuto para o Dia a Dia

Para tarefas pequenas a médias (bug fixes, features de 1-2 cenários, refactors, CRUD), use o **fluxo enxuto** documentado em [`examples/claude-copilot-workflow.md`](examples/claude-copilot-workflow.md).

Ele cobre:
- Setup do VSCode + Copilot com Claude
- Matriz de modelo (Haiku/Sonnet/Opus) por tipo de tarefa
- Templates de prompt copiáveis para cada situação
- Heurística de decisão em 30 segundos
- Anti-padrões a evitar

Use o fluxo completo (seção 7) apenas quando a tarefa tocar múltiplos serviços, exigir alinhamento com stakeholders, ou envolver mudanças irreversíveis.
```

---

## PARTE F — ORDEM DE EXECUÇÃO

1. Criar `examples/claude-copilot-workflow.md` (arquivo principal, Seções 1-7)
2. Criar `examples/prompts/trivial.md`
3. Criar `examples/prompts/bugfix.md`
4. Criar `examples/prompts/feature-pequena.md`
5. Criar `examples/prompts/refactor.md`
6. Criar `examples/prompts/design-exploration.md`
7. Criar `examples/prompts/bugfix-sensivel.md`
8. Criar `examples/copilot-instructions-example.md`
9. Criar `examples/README.md`
10. Editar `README.md` principal (3 alterações da Parte E)

Cada passo é independente exceto o 9 (precisa dos paths dos arquivos 1-8) e o 10 (precisa do path do 1).
