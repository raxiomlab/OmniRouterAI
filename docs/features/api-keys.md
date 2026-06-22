# Gerenciamento de API Keys

O sistema de chaves de API permite autenticar requisições ao proxy de IA e ao gateway MCP, com escopos granulares por provedor, modelo e tipo de acesso.

![API Keys](../assets/api-keys.png)

## Ciclo de Vida

```
Geração → Exibição Única → Uso → Revogação/Expiração
```

### 1. Geração

Ao criar uma chave, o backend gera um par **prefixo + segredo**:

- **Prefixo** (ex: `gbk-a1b2c3`) — salvo em claro no banco para busca rápida
- **Segredo** (ex: `x9y8z7w6...`) — hasheado com HMAC-SHA256 antes de armazenar

### 2. Exibição Única

O segredo é exibido **uma única vez** na interface com o aviso:

> "COPIE AGORA. Você nunca mais verá este segredo."

### 3. Uso

A chave é usada no header `Authorization: Bearer <prefixo>.<segredo>` nas requisições para:
- `POST /v1/chat/completions`
- `POST /v1/mcp`
- Qualquer endpoint `/v1/*`

### 4. Revogação

A revogação é imediata. A chave revogada não pode mais ser usada.

## Escopos

Cada chave pode ter escopos granulares:

| Escopo | Descrição |
|---|---|
| **Tipo de Acesso** | Models, MCPs ou Ambos |
| **Provedores** | Quais provedores a chave pode acessar |
| **Modelos** | Quais modelos específicos estão liberados |
| **Projeto** | A qual projeto a chave pertence |

## Expiração

Prazos configuráveis no momento da criação:

- 7 dias
- 30 dias
- 90 dias
- 1 ano
- Sem expiração

## Segurança

- O segredo nunca é armazenado em texto plano — apenas o hash HMAC-SHA256
- O prefixo permite busca rápida sem expor o hash
- Rate limiting por chave de API
- Validação a cada requisição no middleware `ApiKeyAuthMiddleware`

## Endpoints

| Método | Rota | Descrição |
|---|---|---|
| `GET` | `/api/api-keys` | Listar chaves do usuário |
| `POST` | `/api/api-keys` | Criar nova chave |
| `DELETE` | `/api/api-keys/{id}` | Excluir chave |
| `POST` | `/api/api-keys/{id}/revoke` | Revogar chave |

## Telas Relacionadas

- [API Keys](../screens.md#api-keys-apikeys)
- [Projetos](../screens.md#projetos-projects) — Chaves vinculadas a projetos
