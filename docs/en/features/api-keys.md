# API Key Management

The API key system authenticates requests to the AI proxy and MCP gateway, with granular scopes per provider, model, and access type.

![API Keys](../../assets/api-keys.png)

## Lifecycle

```
Generation → One-Time Display → Usage → Revocation/Expiration
```

### 1. Generation

Each key generates a **prefix + secret** pair:

- **Prefix** (e.g., `gbk-a1b2c3`) — stored in plain text for fast lookup
- **Secret** (e.g., `x9y8z7w6...`) — HMAC-SHA256 hashed before storage

### 2. One-Time Display

The secret is displayed **only once** with the warning:

> "COPY NOW. You will never see this secret again."

### 3. Usage

The key is used in the `Authorization: Bearer <prefix>.<secret>` header for:
- `POST /v1/chat/completions`
- `POST /v1/mcp`
- Any `/v1/*` endpoint

### 4. Revocation

Revocation is immediate. A revoked key can no longer be used.

## Scopes

Each key can have granular scopes:

| Scope | Description |
|---|---|
| **Access Type** | Models, MCPs, or Both |
| **Providers** | Which providers the key can access |
| **Models** | Which specific models are allowed |
| **Project** | Which project the key belongs to |

## Expiration

Configurable at creation time:

- 7 days
- 30 days
- 90 days
- 1 year
- No expiration

## Security

- The secret is never stored in plain text — only the HMAC-SHA256 hash
- The prefix enables fast lookup without exposing the hash
- Rate limiting per API key
- Validation on every request via `ApiKeyAuthMiddleware`

## Endpoints

| Method | Route | Description |
|---|---|---|
| `GET` | `/api/api-keys` | List user keys |
| `POST` | `/api/api-keys` | Create new key |
| `DELETE` | `/api/api-keys/{id}` | Delete key |
| `POST` | `/api/api-keys/{id}/revoke` | Revoke key |

## Related Screens

- [API Keys](../screens.md#api-keys-apikeys)
- [Projects](../screens.md#projects-projects) — Keys linked to projects
