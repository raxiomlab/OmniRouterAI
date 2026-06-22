# OpenAPI Converter

Converte especificações OpenAPI/Swagger em servidores MCP prontos para uso, permitindo que qualquer API REST seja exposta como ferramentas MCP.

![MCP Builder](../assets/mcp-builder.png)

## Fluxo de Conversão

```
Spec OpenAPI (URL / Upload / JSON)
    │
    ├── Descoberta → GET /api/mcp/converter/discover
    ├── Parse → GET /api/mcp/converter/parse
    ├── Preview → GET /api/mcp/converter/preview
    │
    ▼
Configuração:
    ├── Autenticação global (API Key, Bearer, etc.)
    ├── Headers customizados
    └── Mapeamento de endpoints → ferramentas
    │
    ▼
Deploy → POST /api/mcp/converter/deploy
    │
    ▼
Servidor MCP pronto para uso
```

## Endpoints

| Método | Rota | Descrição |
|---|---|---|
| `POST` | `/api/mcp/converter/discover` | Descobrir spec OpenAPI de uma URL |
| `POST` | `/api/mcp/converter/parse` | Parsear conteúdo OpenAPI |
| `POST` | `/api/mcp/converter/preview` | Preview dos artefatos MCP |
| `POST` | `/api/mcp/converter/deploy` | Deploy como servidor MCP |
| `GET` | `/api/mcp/converter/projects` | Listar projetos do conversor |
| `PUT` | `/api/mcp/converter/projects/{id}` | Atualizar projeto |
| `DELETE` | `/api/mcp/converter/projects/{id}` | Excluir projeto |
| `POST` | `/api/mcp/converter/projects/export` | Exportar projeto como JSON |
| `POST` | `/api/mcp/converter/projects/import` | Importar projeto JSON |

## Telas Relacionadas

- [MCP Builder / OpenAPI Converter](../screens.md#mcp-builder--openapi-converter-adminconverter)
