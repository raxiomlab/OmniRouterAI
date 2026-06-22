# Security

OmniRouterAI implements multiple security layers to protect access to AI providers, including IP control, geolocation, and rate limiting.

## IP Rules

![Security Rules](../../assets/security-rules.png)

IP-based access control with CIDR support:

| Type | Description |
|---|---|
| **Allowlist** | Only listed IPs have access permission |
| **Denylist** | Listed IPs are blocked |

### Evaluation Order

1. Denylist is checked first — if the IP is denied, access is blocked
2. GeoRules are checked — if the country is blocked, access is denied
3. Allowlist is checked — if not allowed and list is non-empty, access is blocked

## Geo Rules (GeoIP)

![Security Rules](../../assets/security-rules.png)

Country-based access control:

| Setting | Description |
|---|---|
| **Blocked Country** | Requests from this country are rejected |
| **Allowed Country** | Only requests from this country are accepted |

Uses **MaxMind GeoLite2** database with monthly automatic updates via background job.

## Rate Limiting

| Scope | Description |
|---|---|
| **Global** | Applied to all system requests |
| **Per Project** | Specific limit for a project |
| **Per API Key** | Specific limit for an API key |

### Metrics

| Metric | Description |
|---|---|
| **RPS** | Requests per second |
| **RPM** | Requests per minute |
| **TPM** | Tokens per minute |
| **Burst** | Maximum allowed burst |

### Login Rate Limiting

Brute force protection on the login endpoint:

- Attempt limit per minute per IP
- `429 Too Many Requests` response when exceeded
- Disabled in development and test environments

## Secret Vault

![Secret Vault](../../assets/secret-vault.png)

Encrypted secrets storage for provider credentials:

- Supported types: token, API key, password, header, Google Service Account JSON, Google ADC
- Masked values in the UI
- Environment variable resolution (`${VAR_NAME}` or `${VAR_NAME:-default}`)
- Unresolved variable health check

## Security Pipeline

```
Request
    │
    ├── [1] ApiKeyAuthMiddleware → Validates API key
    ├── [2] SecurityPipeline → IP → Geo → Rate Limit
    ├── [3] GuardRailsMiddleware → Content filter
    │
    ▼
AI Provider
```

## Related Screens

- [Security Rules](../screens.md#security-rules-adminsecurity)
- [Secret Vault](../screens.md#secret-vault-adminsecrets)
