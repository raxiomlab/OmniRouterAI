# OpenAPI Converter

Converts OpenAPI/Swagger specifications into ready-to-use MCP servers, enabling any REST API to be exposed as MCP tools.

![MCP Builder](../../assets/mcp-builder.png)

## Conversion Flow

```
OpenAPI Spec (URL / Upload / JSON)
    │
    ├── Discovery → GET /api/mcp/converter/discover
    ├── Parse → GET /api/mcp/converter/parse
    ├── Preview → GET /api/mcp/converter/preview
    │
    ▼
Configuration:
    ├── Global authentication (API Key, Bearer, etc.)
    ├── Custom headers
    └── Endpoint → tool mapping
    │
    ▼
Deploy → POST /api/mcp/converter/deploy
    │
    ▼
MCP server ready for use
```

## Endpoints

| Method | Route | Description |
|---|---|---|
| `POST` | `/api/mcp/converter/discover` | Discover OpenAPI spec from URL |
| `POST` | `/api/mcp/converter/parse` | Parse OpenAPI content |
| `POST` | `/api/mcp/converter/preview` | Preview MCP artifacts |
| `POST` | `/api/mcp/converter/deploy` | Deploy as MCP server |
| `GET` | `/api/mcp/converter/projects` | List converter projects |
| `PUT` | `/api/mcp/converter/projects/{id}` | Update project |
| `DELETE` | `/api/mcp/converter/projects/{id}` | Delete project |
| `POST` | `/api/mcp/converter/projects/export` | Export project as JSON |
| `POST` | `/api/mcp/converter/projects/import` | Import JSON project |

## Related Screens

- [MCP Builder / OpenAPI Converter](../screens.md#mcp-builder--openapi-converter-adminconverter)
