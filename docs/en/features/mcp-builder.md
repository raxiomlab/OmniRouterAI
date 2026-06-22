# MCP Builder

Tool for creating custom MCP servers from textual definitions or JSON configuration files, without writing code.

![MCP Builder](../../assets/mcp-builder.png)

## How It Works

The MCP Builder enables users to create complete MCP servers through:

1. **Textual definition** — Describe tools in natural language, the builder parses them
2. **JSON configuration** — Import a complete JSON file with the server definition
3. **Tool Editor** — Visual editor for defining each tool, its parameters, and schemas

## Endpoints

| Method | Route | Description |
|---|---|---|
| `POST` | `/api/mcp/builder/parse` | Parse raw definitions into structured tools |
| `POST` | `/api/mcp/builder/preview` | Preview artifacts before deployment |
| `POST` | `/api/mcp/builder/deploy` | Create the MCP server with defined tools |
| `POST` | `/api/mcp/builder/import-config` | Import complete JSON configuration |

## Builder Components

### Tool Editor

Visual editor for configuring each tool:

- Name and description
- Input parameters (path, query, body)
- Input/output schema (JSON Schema)
- Custom headers
- Payload template

### Global Configuration

Settings that apply to all server tools:

- Base URL
- Authentication
- Default headers

## Related Screens

- [MCP Builder / OpenAPI Converter](../screens.md#mcp-builder--openapi-converter-adminconverter)
