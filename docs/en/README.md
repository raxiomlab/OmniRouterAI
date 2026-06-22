# OmniRouterAI

🌐 **English** · [Português](../../README.pt-BR.md)

> 🇺🇸 This is the English documentation. For Portuguese, go to [README.pt-BR.md](../../README.pt-BR.md).

Intelligent gateway for Large Language Model (LLM) APIs with smart routing, multi-layer security, MCP (Model Context Protocol) support, and role-based access control (RBAC).

![Dashboard](../../assets/dashboard.png)

## About OmniRouterAI

A unified platform that acts as a proxy and gateway for multiple AI providers. It centralizes API key management, cost control, content security, and exposes a single OpenAI-compatible interface, while also implementing the Model Context Protocol (MCP).

## Features

| Feature | Description |
|---|---|
| **Multi-Provider Proxy** | Routes requests to 11 AI providers |
| **Smart Routing** | Rule-based intelligent routing with intent classification |
| **API Key Management** | Key generation, revocation, and granular scoping |
| **Guard Rails** | Content filters for PII, credentials, financial, and health data |
| **RBAC** | 5 system roles + custom roles with 59 permissions |
| **MCP Gateway** | Tool discovery and execution via Model Context Protocol |
| **OpenAPI Converter** | Import OpenAPI/Swagger specs as MCP servers |
| **Geo/IP Security** | Country-based and CIDR-based access control |
| **Rate Limiting** | Per-key, per-project, and global rate limits |
| **Analytics** | Consumption dashboards by provider, model, project, group, user |
| **Secret Vault** | Encrypted secrets storage with env var resolution |
| **Audit** | Complete tracking of all system changes |
| **Dual Authentication** | Local login (JWT) + Microsoft OAuth |

## Getting Started

1. **Access** `https://localhost:3000` — the login screen will appear
2. **Authenticate** with email/password or Microsoft Account
3. **Configure providers** at `Admin > Providers`
4. **Create an API Key** in `API Keys` or within a project
5. **Make requests** to `POST /v1/chat/completions` using your key

## Documentation

| Document | Description |
|---|---|
| [Screens](screens.md) | All system pages with screenshots |
| [API Endpoints](api-endpoints.md) | Complete 150+ endpoint reference |
| [Permissions and Roles](permissions-and-roles.md) | Full RBAC system reference |
| [Flows](flows.md) | Functional flows with diagrams |

### Features (detailed)

| Document | Description |
|---|---|
| [Multi-Provider Proxy](features/proxy-multi-provedor.md) | Routing for 11 AI providers |
| [Smart Routing](features/smart-routing.md) | Intent-based intelligent routing |
| [API Keys](features/api-keys.md) | API key lifecycle management |
| [Guard Rails](features/guard-rails.md) | Sensitive content filters |
| [Access Control](features/controle-acesso.md) | RBAC with 59 permissions |
| [MCP Gateway](features/mcp-gateway.md) | Model Context Protocol |
| [OpenAPI Converter](features/openapi-converter.md) | OpenAPI to MCP conversion |
| [MCP Builder](features/mcp-builder.md) | Custom MCP server builder |
| [Security](features/seguranca.md) | IP, Geo, Rate Limit, Secret Vault |
| [Analytics](features/analytics.md) | Dashboards and monitoring |
| [Secret Vault](features/secret-vault.md) | Encrypted secrets storage |
| [Audit](features/auditoria.md) | Audit logging |
| [Authentication](features/autenticacao.md) | Local login and OAuth |

## Supported Providers

OpenAI · Anthropic · Google Gemini · Azure OpenAI · Groq · Mistral AI · Fireworks AI · Together AI · DeepSeek · Perplexity AI · Ollama · Smart Router

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | ASP.NET Core 10 (MVC Controllers) |
| Frontend | React 19 + Vite 8 + shadcn/ui + TanStack Query |
| Database | PostgreSQL / SQLite (dev) |
| Cache | In-Memory (swappable for Redis) |
| Auth | JWT + Microsoft Account (OAuth 2.0) |
| MCP | Model Context Protocol SDK |
