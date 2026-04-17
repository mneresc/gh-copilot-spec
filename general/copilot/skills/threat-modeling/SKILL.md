# Threat Modeling

## Purpose
Estudar o mapeamento de fluxo da aplicacão gerado durante o *Spec Authoring* ou no Intake e elencar de forma sistemática potenciais atores de dolo. Avaliar a "superfície de contato".

## When to use
- Imediatamente após uma nova Feature Spec gigante que cruza de ponta a ponta a empresa e o ambiente for validada, antes ou junto ao desenvolvimento do BDD.

## When not to use
- Em pipelines já consolidados onde o componente em si obedece as mesmas regras mapeadas no passado e os vetores de entrada não alteram a topologia sensível da malha do sistema.

## Expected inputs
- `FEATURE_SPEC.md` contendo fluxos do sistema e módulos envolvidos.
- Topologia resumida do microserviço.

## Operating steps
1. Enumere os limitadores e portas de entrada (APIs abertas, Cron Jobs sem trava, tópicos de Fila assinados).
2. Pense como os Agentes Maliciosos comuns àquele vetor (Robôs varredores, atacantes injetando payloads na fila, usuários logados manipulando ID interno pra apagar conta alheia).
3. Entregue ações MITIGADORAS explícitas para o Test Plan cobrar a arquitetura gerada a fazer frente.

## Quality bar
Sem ser terrorista digital. Não exija soluções inoperáveis, a mitigação precisa apontar pragmatismo de escopo viável atrelado à natureza da aplicação corporativa, baseada no conceito do *Least Privilege*.

## Expected outputs
Adição de cenários mitigadores para os planos de Arquitetura e os inputs que o time utilizará em `security-review`.

## Common failure modes
- Levantar ameaças de infra global (Acesso na console AWS pelo admin) se o sistema avalia uma classe em código TypeScript pontual de uma rota interna puramente. Escopo deve ser restrito ao Spec provindo.

## Minimal checklist
- [ ] Foram considerados vetores Inbound e Outbound?
