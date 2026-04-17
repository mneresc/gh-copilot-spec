# TypeScript Service Implementation

## Purpose
Instrução estrita baseada nas boas formas de escrita em TypeScript orientando na execução prática pelo engenheiro (IA). Combina o uso da base universal com a instrução generalista.

## When to use
- Quando operando em repórterio TypeScript sob o slice em andamento ditado pelo `PLAN.md`.

## When not to use
- Em repos Python.

## Expected inputs
- O Milestone do `PLAN.md`.
- Leitura do `.github/copilot-instructions.md` com a verdade local para mapear bibliotecas aprovadas localmente (Zod, TypeBox, Nest, Vanilla Node, etc).

## Operating steps
1. Verifique `tsconfig.json` e obedeça ao `strict: true`.
2. Produza e injete as interfaces nos domínios antes de ir pros modulos de adaptação (Data, Controller).
3. Use injecão de dependências em constructors e mantenha DTOs explícitos na saída das funções em vez de devolver arrays não tipados ou Records genéricos.

## Quality bar
Sem arquivos sujos importando ORM dentro de lógicas matemáticas da aplicação. Desacoplamento através de Types limpos.

## Expected outputs
Geração de código de software (Arquivos .ts) para atingir os testes do slice exigido.

## Common failure modes
- A IA acidentalmente ignorar o padrão do repositório porque a feature pede "um redis rápido", sujando um serviço TypeScript limpo conectando bibliotecas de DB no Controller.

## Minimal checklist
- [ ] Validou o boundary explicitado em instruções de TS/Microservices?
- [ ] Está tipado rigidamente sem "any"?

## Stack-specific notes
Respeitar `typescript.instructions.md`.
