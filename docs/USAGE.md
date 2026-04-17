# Guia de Uso

Este documento detalha como inicializar, adotar e operar usando as diretrizes e ferramentas baseadas em IA deste repositório.

## Como Instalar (Windows)

A separação é vital. Nunca coloque a "camada geral" acoplada ou misturada com a "camada específica" dentro de um repositório git.

### Camada Geral
Acesse o seu diretório de perfil (home) e crie uma pasta oculta `.copilot` (o nome pode variar mediante sua IA preferencial, como `.cursorrules` ou similar).

```powershell
# Abra o PowerShell
mkdir C:\Users\<USER>\.copilot\
```
Copie a pasta `general/copilot/` inteira diretamente para o diretório recém-criado, de forma que o caminho fique da seguinte maneira:
- `C:\Users\<USER>\.copilot\instructions\`
- `C:\Users\<USER>\.copilot\skills\`
- `C:\Users\<USER>\.copilot\agents\`

### Camada Específica (Assets e Templates)
Nos repositórios de produto, copie o diretório `repo-specific/templates/` para dentro da raiz da sua infraestrutura. 

## Como Adotar (Fluxo Diário)

- Sempre que abrir um repositório, assegure que o agente tem lido o arquivo de "verdade local" `copilot-instructions.md`.
- No dia a dia, invoque explicitamente as roles para limitar alucinações.
- Se atuar no design, chame o `@solution-architect` e passe um template da `templates/` a ser preenchido, como um `ADR.md`.

## Como Usar Agents, Skills, Workflows e Templates

- **Agents**: Atribuição de persona (ex: `security-auditor`). Limita o modelo apenas a preocupações de segurança.
- **Skills**: São chamadas para a ação curtas (ex: execute `story-intake`). O IA lerá `SKILL.md` para entender as entradas e o formato exato de saída.
- **Workflows**: Use para demandas maiores em múltiplos passos pontuais para não esgotar a janela de contexto.
- **Templates**: O esqueleto canônico que obriga a IA a escrever o que foi acertado e não apenas tagarelar no chat.
