# Autenticação

O OmniRouterAI oferece autenticação dual: login local com JWT próprio e login via Microsoft Account (OAuth 2.0 / OpenID Connect).

![Login](../assets/login.png)

## Login Local

O usuário se autentica com e-mail e senha:

```
POST /api/auth/login
    │
    ├── Rate Limiter → proteção contra brute force
    ├── Valida credenciais no banco
    ├── Gera JWT próprio do gateway
    │   Claims: userId, email, role, permissions
    └── Retorna { token, user }
```

### Rate Limiting

O endpoint de login possui proteção contra ataques de força bruta:
- Limite de tentativas por minuto por IP
- Resposta `429 Too Many Requests` quando excedido
- Desabilitado em ambientes de desenvolvimento e teste

## Login Microsoft

```
Usuário → "Entrar com Microsoft"
    │
    ▼
GET /api/auth/microsoft → Redirect para login.microsoft.com
    │
    ▼
Usuário autentica na Microsoft
    │
    ▼
Callback → /api/auth/microsoft/callback
    │
    ├── Recebe claims do OAuth
    ├── Cria/atualiza User local
    ├── Emite JWT próprio do gateway
    └── Redireciona para /dashboard
```

O sistema nunca depende do token da Microsoft internamente — após o callback, um JWT próprio do gateway é emitido.

## Autenticação via API Key

Para requisições de proxy de IA e MCP:

```
Authorization: Bearer <prefixo.segredo>
    │
    ├── ApiKeyAuthMiddleware extrai o token
    ├── Divide em prefixo + segredo
    ├── Busca pelo prefixo no banco
    ├── Verifica HMAC-SHA256 do segredo
    ├── Verifica se não está revogada/expirada
    └── Popula HttpContext com ApiKeyId, ProjectId, UserId
```

## JWT

- **Algoritmo:** HMAC-SHA256
- **Claims:** userId, email, role, permissions
- **Refresh:** Via `POST /api/auth/refresh`
- **Armazenamento:** Em memória (Zustand store) no frontend

## Endpoints

| Método | Rota | Autenticação | Descrição |
|---|---|---|---|
| `POST` | `/api/auth/login` | Anônimo | Login local |
| `POST` | `/api/auth/refresh` | JWT | Renovar token |
| `GET` | `/api/auth/me` | JWT | Dados do usuário |
| `GET` | `/api/auth/microsoft` | Anônimo | Redirect OAuth |
| `GET` | `/api/auth/microsoft/callback` | Anônimo | Callback OAuth |
| `GET` | `/api/auth/oauth-token` | Anônimo | Ler token OAuth do cookie |

## Telas Relacionadas

- [Login](../screens.md#login-authlogin)
