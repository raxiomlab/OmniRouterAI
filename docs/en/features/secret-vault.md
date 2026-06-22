# Secret Vault

Encrypted storage system for secrets used by AI providers and MCP servers, with environment variable resolution support.

![Secret Vault](../../assets/secret-vault.png)

## Secret Types

| Type | Description |
|---|---|
| **Token** | Generic authentication tokens |
| **API Key** | Provider API keys |
| **Password** | Basic auth passwords |
| **Header** | Custom HTTP headers |
| **Google Service Account** | Google service account JSON |
| **Google ADC** | Application Default Credentials |

## Environment Variable Resolution

Secret values can reference environment variables:

```
${MY_VAR}           → Replaced by environment variable
${MY_VAR:-default}  → Uses "default" if variable doesn't exist
```

This allows sensitive values to be defined outside the database, in the runtime environment.

## Health Check

![Secret Vault](../../assets/secret-vault.png)

`GET /api/secrets/env-health` automatically checks if all referenced environment variables are resolved, helping identify incomplete configurations.

## Security

- Values stored encrypted in the database
- Masked UI (never shows full value)
- Access controlled by permission (`secrets:view`, `secrets:create`, `secrets:edit`, `secrets:delete`)

## Endpoints

| Method | Route | Permission | Description |
|---|---|---|---|
| `GET` | `/api/secrets` | `secrets:view` | List secrets |
| `POST` | `/api/secrets` | `secrets:create` | Create secret |
| `PUT` | `/api/secrets/{id}` | `secrets:edit` | Update secret |
| `DELETE` | `/api/secrets/{id}` | `secrets:delete` | Delete secret |
| `GET` | `/api/secrets/env-health` | `secrets:view` | Env var health check |

## Related Screens

- [Secret Vault](../screens.md#secret-vault-adminsecrets)
