# Execution Planning

## Purpose
Transformar especificações e planos de testes difíceis e complexos em marcos seqüenciais e lógicos limitadores ("Milestones") para a injeção do ciclo de escrita/codificação do parIA/Engenheiro. 

## When to use
- Nas véspéras da implementação de uma funcionalidade grande descrita em FEATURES_SPEC.

## When not to use
- Se a tarefa demanda edição de apenas um script na raiz.

## Expected inputs
- `FEATURE_SPEC.md`
- `TEST_PLAN.md` 
- Acesso restrito a estrutura de pastas originais do sistema (arvore de arquivos via bash command).

## Operating steps
1. Avalie as dependências: se criar o serviço A depende da interface B, o Módulo de Interfaces precisa vir primeiro na ordem cronológica de ações.
2. Fatie a demanda em Milestones rastreáveis. (Milestone 1, Milestone 2, etc.)
3. Estabeleça um plano de Stop-and-Fix (ex. "se as queries de TestIntegrationM1 não rodarem perfeitamente no Milestone 1, interrompa a esteira e não faça o Milestone 2".

## Quality bar
Planos que a cada sub-item dão a chance de se compilar a aplicação sem que ela quebre porque o meio foi ignorado.

## Expected outputs
Geração do arquivo `PLAN.md` definindo os marcos, os arquivos a serem afetados em cada um e rollback de estragos pontuais.

## Common failure modes
- Supor que dezenas de arquivos podem ser alterados em um fluxo síncrono da LLM de uma vez, quebrando a contagem de tokens e gerando códigos pela metade. A Skill corrige isso quebrando em etapas duras.

## Minimal checklist
- [ ] Todos os módulos alterados estão explícitos em seu milestone devidamente?
- [ ] A ordem chronologica respeita o pilar (Inversão de Dep.) do repositório?
