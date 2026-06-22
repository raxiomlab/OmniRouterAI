# MCP Gateway

Implementação do **Model Context Protocol (MCP)** para descoberta e execução dinâmica de ferramentas através de um gateway padronizado usando JSON-RPC sobre HTTP.

![MCP Admin](../assets/mcp-admin.png)

## O que é MCP?

O Model Context Protocol é um protocolo aberto que permite que aplicações de IA descubram e executem ferramentas externas de forma padronizada. O OmniRouterAI atua como um gateway MCP, roteando requisições para servidores MCP internos e externos.

## Arquitetura

```
Cliente
    │
    POST /v1/mcp (JSON-RPC)
    │
    ▼
MCP Gateway
    │
    ├── Valida API Key
    ├── Verifica permissão da ferramenta
    ├── Aplica Guard Rails (request)
    ├── Encaminha ao servidor MCP destino
    ├── Recebe resultado
    ├── Aplica Guard Rails (response)
    └── Retorna JSON-RPC response
```

## Tipos de Servidores MCP

| Tipo | Descrição |
|---|---|
| **HTTP** | Servidor MCP via JSON-RPC sobre HTTP |
| **SSE** | Servidor MCP via Server-Sent Events |
| **OpenAPI** | Gerado automaticamente a partir de uma especificação OpenAPI/Swagger |
| **Custom** | Definido manualmente via MCP Builder |
| **Plugin** | Carregado via sistema de plugins |

## Endpoints do Gateway

### JSON-RPC (Streamable HTTP)

| Método | Rota | Autenticação |
|---|---|---|
| `POST` | `/v1/mcp` | API Key |
| `POST` | `/v1/mcp/{slug}` | API Key (servidor específico) |
| `POST` | `/api/mcp/tools/call` | JWT |

### REST

| Método | Rota | Descrição |
|---|---|---|
| `GET` | `/v1/mcp/tools/list` | Listar ferramentas disponíveis |
| `GET` | `/api/mcp/tools` | Ferramentas públicas (sem autenticação) |
| `GET` | `/api/mcp/discovery` | Descoberta completa de ferramentas |
| `GET` | `/api/mcp/visible-servers` | Servidores visíveis ao usuário |

## Fluxo de Execução

1. Cliente envia `POST /v1/mcp` com corpo JSON-RPC
2. Gateway valida a API Key
3. Verifica se a chave tem permissão para a ferramenta solicitada
4. Aplica filtros de Guard Rails nos argumentos (request)
5. Encaminha ao servidor MCP responsável
6. Recebe o resultado da execução
7. Aplica filtros de Guard Rails na resposta (response)
8. Retorna ao cliente no formato JSON-RPC

## Slug Routing

Cada servidor MCP possui um **slug** único que permite roteamento direto:

```
POST /v1/mcp/meu-servidor
```

Isso direciona a requisição JSON-RPC especificamente para o servidor com slug "meu-servidor".

## Administração

![MCP Admin](../assets/mcp-admin.png)

O módulo de administração permite:
- CRUD completo de servidores MCP
- Descoberta de ferramentas via conexão ao servidor
- Importação de specs OpenAPI
- Controle de visibilidade por grupo
- Configuração de Guard Rails por servidor

## Telas Relacionadas

- [MCP Servers (Admin)](../screens.md#servidores-mcp-admin-adminmcp)
- [MCP Tools](../screens.md#mcp-tools-adminmcp-tools)
- [MCP Servers (Usuário)](../screens.md#mcp-servers-usuário-mcp)
