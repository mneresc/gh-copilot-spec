# Checklist de Adoção

Siga este checklist para estruturar a adoção da engenharia assistida por IA num novo projeto de sua organização.

- [ ] **Configuração Geral**
  - Copiar pasta `general/copilot/` para a raiz oculta do seu usuário local `C:\Users\<USER>\.copilot\`.
  - Revisar a estrutura com os arquivos dentro das pastas `agents`, `skills` e `instructions`.
- [ ] **Configuração do Repositório (Produto/Serviço)**
  - Fazer o clone e abrir a pasta base do seu repositório de produto.
  - Copiar a pasta `repo-specific/templates/` para dentro.
  - Preencher manualmente o arquivo `copilot-instructions.md` com os atalhos vitais que definem as operações ali (scripts, arquitetura).
- [ ] **Treinamento e Hábito com Agents e Skills**
  - Fazer a equipe invocar `@pm-bdd-manager` no seu dia a dia ao debater as user stories.
  - Testar a Skill `unit-test-ts` (ou a da sua linguagem local) e auditar os testes de um modelo preexistente.
  - Invocar e testar a Skill `security-review`.
- [ ] **Fluxos Canônicos (Workflows)**
  - Implementar o primeiro `workflow/feature-spec-driven.md` usando a branch de testes ou durante um pequeno card sem grandes restrições burocráticas para treinar artefatos e a visão do STATUS.md.
