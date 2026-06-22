# Guard Rails (Content Filters)

System for detecting and removing sensitive information in AI provider request and response traffic, preventing data leakage.

## Detectors

### PII (Personal Identifiable Information)

Protects end-user personal data:

| Pattern | Category | Example |
|---|---|---|
| CPF (11 digits) | `CPF` | `123.456.789-00` |
| CNPJ (14 digits) | `CNPJ` | `12.345.678/0001-90` |
| Email | `EMAIL` | `user@example.com` |
| Phone | `PHONE` | `+55 (11) 99999-8888` |
| ZIP Code | `ZIPCODE` | `01234-567` |

### Credentials

Prevents sending passwords and tokens to external providers:

| Pattern | Category | Example |
|---|---|---|
| Declared passwords | `PASSWORD` | `password=my-pass` |
| API Keys | `API_KEY` | `apikey=sk-...` |
| Tokens | `TOKEN` | `Bearer ghp_...` |
| Secrets | `SECRET` | `client_secret=...` |

### Financial

Protects financial and banking data:

| Pattern | Category | Example |
|---|---|---|
| Credit card | `CREDIT_CARD` | `4111 1111 1111 1111` |
| PIX key | `PIX_KEY` | `pix key=...` |
| Bank account | `BANK_ACCOUNT` | `branch=1234 account=56789` |

### Health

Detects protected health information:

| Pattern | Category | Example |
|---|---|---|
| Medical record | `MEDICAL_RECORD` | `record=12345` |
| Diagnosis | `DIAGNOSIS` | `diagnosis of diabetes` |
| ICD code | `ICD` | `ICD-10 E11` |

## Operation Modes

Each detector can be configured per provider or MCP server:

| Mode | Description |
|---|---|
| `Request` | Filters content sent from the app to the provider only |
| `Response` | Filters content returned by the provider only |
| `Both` | Filters in both directions |

## Filter Pipeline

```
Request Body (before proxy)
    │
    ├── PII Detection → [CPF-REDACTED]
    ├── Credentials Detection → [PASSWORD-REDACTED]
    ├── Financial Detection → [CREDIT_CARD-REDACTED]
    └── Health Detection → [MEDICAL_RECORD-REDACTED]
    │
    ▼
Sanitized Request → AI Provider
    │
    ▼
Response Body (before client)
    │
    └── Same process in reverse
```

## Per-Provider Configuration

Guard rails are enabled when creating/editing each provider or MCP server through the `GuardRailSelector` UI component.

## Related Screens

- [Providers (Admin)](../screens.md#providers-adminproviders) — Guard rail config per provider
- [MCP Servers (Admin)](../screens.md#mcp-servers-admin-adminmcp) — Guard rail config per MCP server
