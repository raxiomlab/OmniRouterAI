# MCP Gateway

Implementation of the **Model Context Protocol (MCP)** for dynamic tool discovery and execution through a standardized JSON-RPC over HTTP gateway.

![MCP Admin](../../assets/mcp-admin.png)

## What is MCP?

Model Context Protocol is an open protocol that enables AI applications to discover and execute external tools in a standardized way. OmniRouterAI acts as an MCP gateway, routing requests to internal and external MCP servers.

## Architecture

```
Client
    │
    POST /v1/mcp (JSON-RPC)
    │
    ▼
MCP Gateway
    │
    ├── Validates API Key
    ├── Checks tool permission
    ├── Applies Guard Rails (request)
    ├── Forwards to target MCP server
    ├── Receives result
    ├── Applies Guard Rails (response)
    └── Returns JSON-RPC response
```

## MCP Server Types

| Type | Description |
|---|---|
| **HTTP** | MCP server via JSON-RPC over HTTP |
| **SSE** | MCP server via Server-Sent Events |
| **OpenAPI** | Auto-generated from OpenAPI/Swagger spec |
| **Custom** | Manually defined via MCP Builder |
| **Plugin** | Loaded via plugin system |

## Gateway Endpoints

### JSON-RPC (Streamable HTTP)

| Method | Route | Auth |
|---|---|---|
| `POST` | `/v1/mcp` | API Key |
| `POST` | `/v1/mcp/{slug}` | API Key (specific server) |
| `POST` | `/api/mcp/tools/call` | JWT |

### REST

| Method | Route | Description |
|---|---|---|
| `GET` | `/v1/mcp/tools/list` | List available tools |
| `GET` | `/api/mcp/tools` | Public tools (no auth) |
| `GET` | `/api/mcp/discovery` | Full tool discovery |
| `GET` | `/api/mcp/visible-servers` | User-visible servers |

## Execution Flow

1. Client sends `POST /v1/mcp` with JSON-RPC body
2. Gateway validates the API Key
3. Checks if the key has permission for the requested tool
4. Applies Guard Rails filters on arguments (request)
5. Routes to the responsible MCP server
6. Receives the execution result
7. Applies Guard Rails filters on response
8. Returns to the client in JSON-RPC format

## Slug Routing

Each MCP server has a unique **slug** for direct routing:

```
POST /v1/mcp/my-server
```

This directs the JSON-RPC request specifically to the server with slug "my-server".

## Administration

![MCP Admin](../../assets/mcp-admin.png)

The admin module enables:
- Full MCP server CRUD
- Tool discovery via server connection
- OpenAPI spec import
- Group-based visibility control
- Guard Rail configuration per server

## Related Screens

- [MCP Servers (Admin)](../screens.md#mcp-servers-admin-adminmcp)
- [MCP Tools](../screens.md#mcp-tools-adminmcp-tools)
- [MCP Servers (User)](../screens.md#mcp-servers-user-mcp)
