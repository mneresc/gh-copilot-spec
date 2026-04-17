# Spec Authoring

## Purpose
Materializar a demanda cortada logicamente e torná-la oficial via documento tangível (`FEATURE_SPEC.md`). É a conversão explícita para "specification-driven".

## When to use
- No momento onde a solução conceitual precisa se estabilizar e "congelar" como norte do que os robôs e humanos lerão daqui pra frente como Single Source of Truth para novos cenários operacionais.

## When not to use
- Updates muito triviais onde o tempo de ler uma spec excede uma explicação de um parágrafo via hotfix na rotina interna da equipe.

## Expected inputs
- As notas de Slices já formatadas.
- O Intake inicial validado e com permissões.

## Operating steps
1. Estruture um esqueleto do tipo "Feature Spec".
2. Preencha as seções vitais: Objetivos, Impactos, Suposições do Negócio, e a seção crucial "Fora de Escopo".
3. Descreva a arquitetura de módulos modificados sem ditar a lógica if/else. Apenas defina fluxos.
4. Apresente o texto.

## Quality bar
Sem vocabulário ambíguo (evite "talvez faremos", use "será feito X, não será feito Y").

## Expected outputs
O documento puro de specification para armazenamento persistente e leitura obrigatória posterior.

## Common failure modes
- Repetir a História em vez de escrever um Spec Técnico com limites de software explícitos.
- Ignorar de qual banco vai tirar os dados ou onde os conectores são necessários na escrita.

## Minimal checklist
- [ ] O spec tem exclusões (fora de escopo) firmes explicitadas de maneira fácil de encontrar?
- [ ] Define dependências diretas inter-serviços ou falhas sistêmicas (se o viável A cair)?
