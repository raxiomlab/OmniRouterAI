# MCP Builder

Ferramenta para criar servidores MCP customizados a partir de definições textuais ou arquivos de configuração JSON, sem necessidade de escrever código.

![MCP Builder](../assets/mcp-builder.png)

## Como Funciona

O MCP Builder permite que usuários criem servidores MCP completos através de:

1. **Definição textual** — Descreva as ferramentas em linguagem natural e o builder faz o parse
2. **Configuração JSON** — Importe um arquivo JSON completo com a definição do servidor
3. **Tool Editor** — Editor visual para definir cada ferramenta, seus parâmetros e schemas

## Endpoints

| Método | Rota | Descrição |
|---|---|---|
| `POST` | `/api/mcp/builder/parse` | Parsear definições brutas em ferramentas estruturadas |
| `POST` | `/api/mcp/builder/preview` | Visualizar artefatos antes do deploy |
| `POST` | `/api/mcp/builder/deploy` | Criar o servidor MCP com as ferramentas definidas |
| `POST` | `/api/mcp/builder/import-config` | Importar configuração JSON completa |

## Componentes do Builder

### Tool Editor

Editor visual para configurar cada ferramenta:

- Nome e descrição
- Parâmetros de entrada (path, query, body)
- Schema de entrada/saída (JSON Schema)
- Headers customizados
- Payload template

### Configuração Global

Configurações que se aplicam a todas as ferramentas do servidor:

- URL base
- Autenticação
- Headers padrão

## Telas Relacionadas

- [MCP Builder / OpenAPI Converter](../screens.md#mcp-builder--openapi-converter-adminconverter)
