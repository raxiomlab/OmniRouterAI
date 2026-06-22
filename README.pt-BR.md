# OmniRouterAI

🌐 [English](README.md) · **Português**

> 🇧🇷 Esta é a documentação em português. Para a versão em inglês, acesse [docs/en/](docs/en/README.md).

Gateway inteligente para APIs de Large Language Models (LLMs) com roteamento inteligente, segurança multicamadas, suporte a MCP (Model Context Protocol) e controle de acesso baseado em papéis (RBAC).

![Dashboard](assets/dashboard.png)

## Sobre o OmniRouterAI

Plataforma unificada que atua como proxy e gateway para múltiplos provedores de IA. Centraliza o gerenciamento de chaves de API, controle de custos, segurança de conteúdo, e expõe uma interface única compatível com a API OpenAI, além de implementar o Model Context Protocol (MCP).

## Funcionalidades

| Funcionalidade | Descrição |
|---|---|
| **Proxy Multi-Provedor** | Roteia requisições para 11 provedores de IA |
| **Smart Routing** | Roteamento inteligente baseado em regras e classificação de intenção |
| **API Key Management** | Geração, revogação e escopagem de chaves de API |
| **Guard Rails** | Filtros de conteúdo para PII, credenciais, dados financeiros e de saúde |
| **Controle de Acesso (RBAC)** | 5 papéis de sistema + papéis customizados com 59 permissões |
| **MCP Gateway** | Descoberta e execução de ferramentas via Model Context Protocol |
| **OpenAPI Converter** | Importa specs OpenAPI/Swagger como servidores MCP |
| **Geolocalização e IP Rules** | Controle de acesso por país e lista de permissão/negação por CIDR |
| **Rate Limiting** | Limitação de taxa por chave de API, projeto ou global |
| **Analytics e Insights** | Dashboards de consumo por provedor, modelo, projeto, grupo e usuário |
| **Secret Vault** | Armazenamento criptografado de segredos com resolução de variáveis de ambiente |
| **Auditoria** | Rastreamento completo de todas as alterações no sistema |
| **Autenticação Dual** | Login local (JWT) + OAuth Microsoft |

## Documentação em Português

### Funcionalidades (detalhadas)

| Documento | Descrição |
|---|---|
| [Proxy Multi-Provedor](docs/features/proxy-multi-provedor.md) | Roteamento para 11 provedores de IA |
| [Smart Routing](docs/features/smart-routing.md) | Roteamento inteligente por intenção |
| [API Keys](docs/features/api-keys.md) | Gerenciamento de chaves de API |
| [Guard Rails](docs/features/guard-rails.md) | Filtros de conteúdo sensível |
| [Controle de Acesso](docs/features/controle-acesso.md) | RBAC com 59 permissões |
| [MCP Gateway](docs/features/mcp-gateway.md) | Model Context Protocol |
| [OpenAPI Converter](docs/features/openapi-converter.md) | OpenAPI → MCP |
| [MCP Builder](docs/features/mcp-builder.md) | Builder de servidores MCP |
| [Segurança](docs/features/seguranca.md) | IP, Geo, Rate Limit, Secret Vault |
| [Analytics](docs/features/analytics.md) | Dashboards e monitoramento |
| [Secret Vault](docs/features/secret-vault.md) | Armazenamento de segredos |
| [Auditoria](docs/features/auditoria.md) | Logs de auditoria |
| [Autenticação](docs/features/autenticacao.md) | Login local e OAuth Microsoft |

### Referências

| Documento | Conteúdo |
|---|---|
| [Telas do Sistema](docs/screens.md) | Todas as páginas com capturas de tela |
| [API Endpoints](docs/api-endpoints.md) | Referência completa de 150+ endpoints |
| [Permissões e Papéis](docs/permissions-and-roles.md) | Sistema completo de RBAC |
| [Fluxos](docs/flows.md) | Fluxos funcionais com diagramas |

## Provedores Suportados

OpenAI · Anthropic · Google Gemini · Azure OpenAI · Groq · Mistral AI · Fireworks AI · Together AI · DeepSeek · Perplexity AI · Ollama · Smart Router

## Stack

| Camada | Tecnologia |
|---|---|
| Backend | ASP.NET Core 10 (Controllers MVC) |
| Frontend | React 19 + Vite 8 + shadcn/ui + TanStack Query |
| Banco | PostgreSQL / SQLite (dev) |
| Cache | In-Memory (substituível por Redis) |
| Auth | JWT + Microsoft Account (OAuth 2.0) |
| MCP | Model Context Protocol SDK |
