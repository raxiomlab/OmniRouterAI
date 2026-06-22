# Functional Flows

## 1. Chat Completion Proxy

```
Client (curl/app)
    │
    ▼
[1] ApiKeyAuthMiddleware
    │   Extracts Bearer <prefix.secret>
    │   Searches by prefix → validates HMAC → populates HttpContext
    │
    ▼
[2] SecurityPipelineMiddleware
    │   ├─ Denylist? IP matches? → 403 Forbidden
    │   ├─ GeoRules? Country blocked? → 403 Forbidden
    │   ├─ Allowlist? IP not allowed? → 403 Forbidden
    │   └─ RateLimit? Window exceeded? → 429 Too Many Requests
    │
    ▼
[3] GuardRailsMiddleware (Request)
    │   Filters sensitive content (PII, credentials, etc.)
    │
    ▼
[4] ProxyService → Target Provider
    │   Forwards request via HttpClient
    │   SSE stream or full response
    │
    ▼
[5] GuardRailsMiddleware (Response)
    │   Filters response (if configured)
    │   Counts tokens
    │   Writes RequestLog
    │
    ▼
Client ← 200 OK or SSE stream
```

### Smart Routing Flow

```
Proxy receives request with model = "smart-routing"
    │
    ├─ Extracts last user message
    ├─ Classifies intent (code, simple_qa, creative_writing, etc.)
    ├─ Evaluates configured rules (by priority)
    │   ├─ keyword: does any keyword match?
    │   ├─ semantic: does exact text match?
    │   ├─ model: does requested model match?
    │   └─ always: catch-all
    │
    ├─ Determines target provider + model
    │   ├─ Specific rule → target provider + model
    │   └─ No rule → fallback provider + model
    │
    └─ Forwards request to target
```

## 2. API Key Lifecycle

```
User → "New API Key"
    │
    ▼
Selects:
    ├─ Project
    ├─ Key name
    ├─ Expiration (7d/30d/90d/1yr/never)
    ├─ Access type (Models / MCPs / Both)
    └─ Granular scopes (per provider/model)
    │
    ▼
Backend generates: gbk-a1b2c3.x9y8z7w6...
    │
    ├─ Prefix: gbk-a1b2c3 → stored in plain text (fast lookup)
    ├─ Secret: x9y8z7w6... → HMAC-SHA256(secret, appSecret) = KeyHash
    └─ Saves: ApiKey { ProjectId, KeyHash, Prefix, Name, ExpiresAt, Scopes }
    │
    ▼
Frontend displays secret ONCE:
    "COPY NOW. You will never see this secret again."
    │
    ▼
Listing (GET /api/api-keys):
    Only prefix + name + created/expires/revoked dates
    (NEVER shows the secret)
```

### Key Validation in Middleware

```
Request received with "Authorization: Bearer gbk-a1b2c3.x9y8z7w6..."
    │
    ├─ Splits into prefix (gbk-a1b2c3) and secret (x9y8z7w6...)
    ├─ Searches DB by prefix
    ├─ Checks not revoked or expired
    ├─ HMAC-SHA256(secret, appSecret) → compares with KeyHash
    └─ Populates HttpContext.Items with ApiKeyId, ProjectId, UserId
```

## 3. Authentication Flow

### Local Login

```
User → Enters email + password
    │
    ▼
POST /api/auth/login
    │
    ├─ [Rate Limiter] Brute force protection (429 after excess)
    ├─ Validates credentials
    ├─ Generates JWT
    │   Claims: userId, email, role, permissions
    └─ Returns { token, user }
```

### Microsoft OAuth Login

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
Microsoft redirects to /api/auth/microsoft/callback
    │
    ├─ Receives OAuth claims (name, email, oid)
    ├─ Creates/updates local User if needed
    ├─ Issues gateway JWT (same format as local login)
    ├─ Saves token to oauth_token cookie
    └─ Redirects to /dashboard
```

## 4. Provider Management

### Provider Creation

```
Admin → "Add Provider"
    │
    ▼
Creation wizard:
    ├─ Type: OpenAI / Anthropic / Ollama / Azure OpenAI / Groq / Mistral /
    │        Fireworks / TogetherAI / DeepSeek / Perplexity / Smart Router
    ├─ Name and description
    ├─ API base URL
    ├─ Authentication:
    │   ├─ API Key
    │   ├─ Bearer Token
    │   ├─ Google Service Account (JSON)
    │   └─ Google ADC
    ├─ Custom headers (with Secret Vault support)
    ├─ Priority (for load balancing)
    ├─ Cost per 1k tokens (input/output)
    ├─ Group assignment (visibility)
    │
    ├─ [Optional] Smart Routing:
    │   ├─ Fallback provider/model
    │   └─ Rules: priority, condition (keyword/semantic/model/always),
    │      target provider/model
    │
    ├─ [Optional] Guard Rails:
    │   ├─ PII Detection (Request / Response / Both)
    │   ├─ Credentials Detection
    │   ├─ Financial Detection
    │   └─ Health Detection
    │
    └─ Test connection → Fetch Models → Save
```

### Inference Test

```
Admin → "Test Inference" on provider
    │
    ▼
Dialog with:
    ├─ Model: (dropdown with available models)
    ├─ Message: (textarea)
    └─ "Test" button
    │
    ▼
Request sent to real provider
Response displayed in dialog
```

## 5. MCP (Model Context Protocol) Flow

### MCP Server Registration

```
Admin → "New MCP Server"
    │
    ▼
Configure:
    ├─ Type: HTTP / SSE / OpenAPI / Custom
    ├─ Name, slug (URL identifier), description
    ├─ Server URL
    ├─ Custom headers
    ├─ Visibility: public or per-group
    └─ Active Guard Rails
    │
    ▼
Discover tools:
    ├─ Connects to server and lists available tools
    ├─ Or imports from OpenAPI spec
    └─ Saves tools in database
```

### MCP Tool Execution via Gateway

```
Client (API Key)
    │
    ▼
POST /v1/mcp  (JSON-RPC)
    │
    Body: { "jsonrpc": "2.0", "method": "tools/call",
             "params": { "name": "server-tool", "arguments": {...} } }
    │
    ▼
Gateway:
    ├─ Validates API Key
    ├─ Checks tool permission for the key
    ├─ [Guard Rails] Filters arguments (request)
    ├─ Forwards to target MCP server
    ├─ Receives result
    ├─ [Guard Rails] Filters response (response)
    └─ Returns JSON-RPC response
```

### Specific Server Routing

```
POST /v1/mcp/my-server
    │
    ▼
Gateway routes directly to server with slug "my-server"
    ├─ Checks key access to server
    └─ JSON-RPC forward
```

## 6. Group Visibility Management

### Visibility Control by Group

```
Admin → "Permissions" → Select group
    │
    ▼
For each resource (provider / MCP server / project):
    ├─ Switch ON: resource visible to group members
    └─ Switch OFF: resource not visible
    │
    ▼
Group users see only enabled resources:
    ├─ GET /api/models → only visible provider models
    ├─ GET /api/mcp → only visible MCP servers
    └─ GET /api/projects → only linked projects
```

## 7. Guard Rails (Content Filter Pipeline)

```
Request body (before proxy)
    │
    ▼
GuardRailsMiddleware
    │
    ├─ Checks if provider/server has guard rails configured
    ├─ For each active detector (PII, Credentials, Financial, Health):
    │   ├─ Scans user messages
    │   ├─ Scans tool calls
    │   └─ Replaces matches with [CATEGORY-REDACTED]
    │
    ▼
Sanitized request → sent to provider
    │
    ▼
Response body (before client)
    │
    ├─ Same process in reverse
    └─ Sensitive content removed from response
```

## 8. Audit

```
Every write operation (Create/Update/Delete)
    │
    ▼
AuditInterceptor (EF Core SaveChanges interceptor)
    │
    ├─ Captures changed entity, action, timestamp
    ├─ Records old and new values
    ├─ Associates with authenticated user
    └─ Saves to AuditLogs table
    │
    ▼
Admin views at /admin/audit
    ├─ Filters by entity, action, user, period
    └─ Details: changed fields, previous/new values
```

## 9. Secret Vault

### Creating a Secret

```
Admin → Secret Vault → New Secret
    │
    ▼
Type: Token / API Key / Password / Header / Google SA / Google ADC
    │
    ▼
Name + Value (masked in UI)
    │
    ▼
Value can contain environment variables:
    ├─ ${MY_VAR} → replaced by env var at runtime
    └─ ${MY_VAR:-default} → uses "default" if env var doesn't exist
    │
    ▼
Stored encrypted in database
```

### Health Check

```
GET /api/secrets/env-health
    │
    ▼
For each secret with environment variables:
    ├─ Checks if each ${VAR} is defined in environment
    └─ Returns list of resolved and unresolved variables
```

## 10. Dashboard Overview

```
Logged-in user → "/" or "/dashboard"
    │
    ▼
Loads in parallel:
    ├─ StatsGrid: total requests, tokens, cost, active providers
    ├─ UsageCharts: consumption over time
    ├─ RecentActivity: latest request timeline
    ├─ ProviderHealth: each provider's status (online/offline)
    └─ Recent user projects
    │
    ▼
Quick actions:
    ├─ Generate API Key → /apikeys
    ├─ Invite User → /admin/users
    ├─ Add MCP Server → /admin/mcp
    └─ Security Rules → /admin/security
```
