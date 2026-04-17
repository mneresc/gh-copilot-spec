# Workflows e Ciclos

Este documento unifica e define quando utilizar os fluxos fornecidos em `workflows/`.

## Ciclo Completo (Feature Nova)
Quando a modificação necessitar criar arquivos ou possuir múltiplos impactos indiretos.

**Workflows Relacionados:** `feature-spec-driven.md`
1. **História e Escopo**: Uma User Story com os critérios.
2. **BDD**: Regras de negócio viram testes (ex: Locadora: *Given: locação atrasada 2 dias; When: finalizado, Then: multa = R$ 100*).
3. **Test Plan**: Plano do que deve ser garantido por teste e do que pode ser ignorado.
4. **Implementação**: Código iterativo usando artefatos auxiliares.
5. **Auditoria**: A IA cruza código gerado x Spec para verificar desvios de escopo.

## Ciclo Mínimo (Corrigir Falha Simples)
Um ajuste rápido de design ou pequena exception não documentada.

- Não engatilhar um ciclo pesado de Spec.
- Use a Skill solta `pair-implement` para focar em difusões e pequenas manutenções.

## Quando Entra BDD / Test Plan / Auditorias?
- **O BDD entra** para congelar expectativas de negócio sobre o comportamento antes da engenharia supor regras.
- **O Test Plan entra** logo após o BDD para validar a arquitetura dos testes sem a IA escrever as respostas das classes que ainda não existem.
- **As Auditorias entram** logo após a codificação em `pair-implement` falhar ou alertar suspeição de mudança colateral. Serve para validar "Least Privilege" ou que nenhum desvio de feature ou regressão foi feito.
