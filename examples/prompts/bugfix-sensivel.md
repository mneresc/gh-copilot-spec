# Prompt — Bug em Código Sensível

**Quando usar:** Bugs que afetam autenticação, pagamentos, ou integridade de dados.
**Modelo:** Opus → Sonnet → Opus
**Skills relacionadas:** `incident-hotfix-review`, `security-review`
**Agent relacionado:** `@security-auditor`

```markdown
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
