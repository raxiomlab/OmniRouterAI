# Multi-Provider Proxy

The core of OmniRouterAI is an OpenAI-compatible API proxy that routes requests to 11 different AI providers, centralizing authentication, costs, and monitoring in a single endpoint.

![Providers](../../assets/providers.png)

## Supported Providers

| Provider | Auth Type | Protocol |
|---|---|---|
| **OpenAI** | API Key | HTTP |
| **Anthropic** (Claude) | API Key / Bearer Token | HTTP |
| **Google Gemini** | Google Service Account / ADC | HTTP |
| **Azure OpenAI** | API Key + Endpoint | HTTP |
| **Groq** | API Key | HTTP |
| **Mistral AI** | API Key | HTTP |
| **Fireworks AI** | API Key | HTTP |
| **Together AI** | API Key | HTTP |
| **DeepSeek** | API Key | HTTP |
| **Perplexity AI** | API Key | HTTP |
| **Ollama** | None (local) | HTTP |
| **Smart Router** | — | Virtual (intelligent router) |

## How It Works

1. Client sends a `POST /v1/chat/completions` request in OpenAI format, authenticated via API Key
2. Middleware validates the key and applies security policies (IP, Geo, Rate Limit)
3. Guard Rails filter sensitive content from the request
4. The proxy forwards the request to the configured provider, preserving relevant headers
5. The provider processes and returns the response (streaming or not)
6. Guard Rails filter the response (if configured)
7. Tokens are counted and the request log is recorded

## Configuration

Each provider has the following settings:

- **Name and description**
- **Base URL** — Provider API endpoint
- **Authentication** — API Key, Bearer Token, Google Service Account (JSON) or Google ADC
- **Custom headers** — Additional headers sent in requests
- **Priority** — Selection order for load balancing
- **Cost per 1k tokens** — Input/output cost for spending calculation
- **Groups** — Visibility assignment by user group
- **Timeout and Retry** — Fault tolerance settings (via Polly)
- **Models** — List of available models
- **Smart Routing** — Intelligent routing rules (Smart Router only)
- **Guard Rails** — Active content filters per provider

## Endpoints

| Method | Route | Description |
|---|---|---|
| `POST` | `/v1/chat/completions` | Chat completion proxy |
| `GET` | `/v1/models` | List accessible models |
| `GET` | `/v1/providers/health` | Provider health status |

## Related Screens

- [Providers (Admin)](../screens.md#providers-adminproviders)
- [New Provider Wizard](../screens.md#new-provider-wizard-adminprovidersnew)
- [My Providers](../screens.md#my-providers-my-providers)
