# Decisões Arquiteturais

Aqui listamos os ADRs (Architecture Decision Records) fundamentais de design **deste próprio repositório**.

## 1. Por que separar Camada Geral e Específica?
Arquivos no user profile contêm os padrões da organização que governam todos os repositórios (postura de qualidade, limites rígidos para infraestrutura). Colocar a camada específica no repositório faz com que regras locais sobre bibliotecas, scripts ou variáveis de ambiente persistam organicamente onde pertencem. IA precisa da verdade local isolada para evitar usar Next.js num repositório Vue.js só porque os prompts gerais listam tudo.

## 2. Por que especificação orientada (Spec-Driven)?
Bate-papo prolongado com IA dilui contexto, ocasiona código alucinado e foca excessivamente no código perdendo requisitos em loopings infinitos. Os artefatos criam ancoragens fixas para "o que foi acertado", obrigando verificações documentais constantes antes do engenheiro aprovar.

## 3. Por que manter assets genéricos no User Profile?
Se as `instructions`, `skills` e `agents` fossem importadas e copiadas repo-by-repo, as atualizações nelas iriam requerer Pull Requests em 400 repositórios diferentes de microsserviços. Mantê-las via injeção global ou no user profile garante alinhamento unificado da equipe na hora da promptagem, diminuindo custos de manutenção.
