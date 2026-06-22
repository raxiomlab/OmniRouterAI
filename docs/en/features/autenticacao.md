# Authentication

OmniRouterAI offers dual authentication: local login with JWT and Microsoft Account (OAuth 2.0 / OpenID Connect).

![Login](../../assets/login.png)

## Local Login

Users authenticate with email and password:

```
POST /api/auth/login
    │
    ├── Rate Limiter → brute force protection
    ├── Validates credentials in database
    ├── Generates gateway JWT
    │   Claims: userId, email, role, permissions
    └── Returns { token, user }
```

### Rate Limiting

The login endpoint has brute force protection:
- Attempt limit per minute per IP
- `429 Too Many Requests` response when exceeded
- Disabled in development and test environments

## Microsoft Login

```
User → "Sign in with Microsoft"
    │
    ▼
GET /api/auth/microsoft → Redirect to login.microsoft.com
    │
    ▼
User authenticates with Microsoft
    │
    ▼
Callback → /api/auth/microsoft/callback
    │
    ├── Receives OAuth claims
    ├── Creates/updates local User
    ├── Issues gateway JWT
    └── Redirects to /dashboard
```

The system never depends on the Microsoft token internally — after callback, a gateway-specific JWT is issued.

## API Key Authentication

For AI proxy and MCP requests:

```
Authorization: Bearer <prefix.secret>
    │
    ├── ApiKeyAuthMiddleware extracts the token
    ├── Splits into prefix + secret
    ├── Searches by prefix in database
    ├── Validates HMAC-SHA256 of the secret
    ├── Checks not revoked/expired
    └── Populates HttpContext with ApiKeyId, ProjectId, UserId
```

## JWT

- **Algorithm:** HMAC-SHA256
- **Claims:** userId, email, role, permissions
- **Refresh:** Via `POST /api/auth/refresh`
- **Storage:** In-memory (Zustand store) on the frontend

## Endpoints

| Method | Route | Auth | Description |
|---|---|---|---|
| `POST` | `/api/auth/login` | Anonymous | Local login |
| `POST` | `/api/auth/refresh` | JWT | Refresh token |
| `GET` | `/api/auth/me` | JWT | User info |
| `GET` | `/api/auth/microsoft` | Anonymous | OAuth redirect |
| `GET` | `/api/auth/microsoft/callback` | Anonymous | OAuth callback |
| `GET` | `/api/auth/oauth-token` | Anonymous | Read OAuth cookie |

## Related Screens

- [Login](../screens.md#login-authlogin)
