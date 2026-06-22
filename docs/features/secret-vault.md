# Secret Vault

Sistema de armazenamento criptografado para segredos utilizados pelos provedores de IA e servidores MCP, com suporte a resolução de variáveis de ambiente.

![Secret Vault](../assets/secret-vault.png)

## Tipos de Segredo

| Tipo | Descrição |
|---|---|
| **Token** | Tokens de autenticação genéricos |
| **API Key** | Chaves de API para provedores |
| **Password** | Senhas para autenticação básica |
| **Header** | Headers HTTP customizados |
| **Google Service Account** | JSON de conta de serviço Google |
| **Google ADC** | Application Default Credentials |

## Resolução de Variáveis de Ambiente

Os valores dos segredos podem referenciar variáveis de ambiente:

```
${MINHA_VAR}           → Substituído pela variável de ambiente
${MINHA_VAR:-default}  → Usa "default" se a variável não existir
```

Isso permite que valores sensíveis sejam definidos fora do banco de dados, no ambiente de execução.

## Health Check

![Secret Vault](../assets/secret-vault.png)

O endpoint `GET /api/secrets/env-health` verifica automaticamente se todas as variáveis de ambiente referenciadas nos segredos estão resolvidas, facilitando a identificação de configurações incompletas.

## Segurança

- Valores armazenados de forma criptografada no banco de dados
- Interface mascarada (nunca exibe o valor completo)
- Acesso controlado por permissão (`secrets:view`, `secrets:create`, `secrets:edit`, `secrets:delete`)

## Endpoints

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| `GET` | `/api/secrets` | `secrets:view` | Listar segredos |
| `POST` | `/api/secrets` | `secrets:create` | Criar segredo |
| `PUT` | `/api/secrets/{id}` | `secrets:edit` | Atualizar segredo |
| `DELETE` | `/api/secrets/{id}` | `secrets:delete` | Excluir segredo |
| `GET` | `/api/secrets/env-health` | `secrets:view` | Health check de env vars |

## Telas Relacionadas

- [Secret Vault](../screens.md#secret-vault-adminsecrets)
