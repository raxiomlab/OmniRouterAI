# Proxy Multi-Provedor

O núcleo do OmniRouterAI é um proxy de API compatível com o formato OpenAI que roteia requisições para 11 provedores de IA diferentes, centralizando autenticação, custos e monitoramento em um único ponto.

![Providers](../assets/providers.png)

## Provedores Suportados

| Provedor | Tipo de Autenticação | Protocolo |
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
| **Ollama** | Nenhuma (local) | HTTP |
| **Smart Router** | — | Virtual (roteador inteligente) |

## Funcionamento

1. O cliente envia uma requisição `POST /v1/chat/completions` no formato OpenAI, autenticada via API Key
2. O middleware valida a chave e aplica as políticas de segurança (IP, Geo, Rate Limit)
3. O Guard Rails filtra o conteúdo sensível da requisição
4. O proxy encaminha a requisição ao provedor configurado, preservando headers relevantes
5. O provedor processa e retorna a resposta (streaming ou não)
6. O Guard Rails filtra a resposta (se configurado)
7. Os tokens são contabilizados e o log de requisição é registrado

## Configuração

Cada provedor possui as seguintes configurações:

- **Nome e descrição** — Identificação do provedor
- **URL base** — Endpoint da API do provedor
- **Autenticação** — API Key, Bearer Token, Google Service Account (JSON) ou Google ADC
- **Headers customizados** — Headers adicionais enviados nas requisições
- **Prioridade** — Ordem de seleção para balanceamento
- **Custo por 1k tokens** — Custo de input e output para cálculo de gastos
- **Grupos** — Atribuição de visibilidade por grupo de usuários
- **Timeout e Retry** — Configurações de tolerância a falhas (via Polly)
- **Modelos** — Lista de modelos disponíveis no provedor
- **Smart Routing** — Regras de roteamento inteligente (apenas para o provedor Smart Router)
- **Guard Rails** — Filtros de conteúdo ativos por provedor

## Endpoints

| Método | Rota | Descrição |
|---|---|---|
| `POST` | `/v1/chat/completions` | Proxy de chat completion |
| `GET` | `/v1/models` | Lista modelos acessíveis |
| `GET` | `/v1/providers/health` | Status de saúde dos provedores |

## Telas Relacionadas

- [Provedores (Admin)](../screens.md#provedores-adminproviders)
- [Novo Provedor (Wizard)](../screens.md#novo-provedor-wizard-adminprovidersnew)
- [Meus Provedores](../screens.md#meus-provedores-my-providers)
