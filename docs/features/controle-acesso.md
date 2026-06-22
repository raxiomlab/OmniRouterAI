# Controle de Acesso (RBAC)

Sistema completo de controle de acesso baseado em papéis com permissões granulares, suporte a papéis de sistema e customizados, e atribuição com escopo.

## Papéis de Sistema

![Roles](../assets/roles.png)

Cinco papéis fixos definidos em código, sincronizados automaticamente com o banco na inicialização:

| Papel | Acesso |
|---|---|
| **Administrador** | Total — todas as 59 permissões |
| **Manager** | Gerencia grupos, visualiza consumo do time e insights |
| **User** | Padrão — projetos, API keys, consumo próprio |
| **Viewer** | Somente leitura em todos os módulos |
| **MCP User** | Acesso exclusivo a servidores MCP e execução de ferramentas |

## Permissões

59 permissões organizadas por categoria:

| Categoria | Exemplos |
|---|---|
| **Provedores** | `providers:view`, `providers:create`, `providers:edit`, `providers:delete` |
| **Modelos** | `models:view`, `models:create`, `models:edit`, `models:delete` |
| **Projetos** | `projects:view`, `projects:create`, `projects:edit`, `projects:delete` |
| **API Keys** | `apikeys:view`, `apikeys:create`, `apikeys:revoke`, `apikeys:delete` |
| **Usuários** | `users:view`, `users:create`, `users:edit`, `users:delete`, `users:manage-roles` |
| **Grupos** | `groups:view`, `groups:create`, `groups:manage-members` |
| **MCP** | `mcp:servers:view`, `mcp:servers:create`, `mcp:tools:call`, `mcp:access` |
| **Consumo** | `consumption:view:own`, `consumption:view:team`, `consumption:view:all` |
| **Segurança** | `security:view`, `security:edit` |
| **Configurações** | `settings:view`, `settings:edit` |
| **Auditoria** | `audit:view`, `audit:export` |

## Papéis Customizados

![Permissions](../assets/permissions.png)

Além dos papéis de sistema, é possível criar papéis customizados com qualquer combinação de permissões:

```http
POST /api/role-profiles
{
  "name": "DevOps Lead",
  "permissions": ["providers:view", "providers:create", "consumption:view:all"]
}
```

### Diferenças

| Característica | Papel de Sistema | Papel Customizado |
|---|---|---|
| Origem do ID | Fixo em código | Gerado na criação |
| Excluível via API | Não | Sim |
| Versionado no Git | Sim | Não |
| Seed automático | Sim | Não |

## Atribuição

A atribuição pode ser global ou com escopo:

```http
POST /api/role-profiles/assign
{
  "userId": "uuid-do-usuario",
  "roleProfileId": "b1eebc99-...",
  "projectId": "uuid-do-projeto",  // opcional
  "expiresAt": "2026-12-31T23:59:59Z"  // opcional
}
```

### Dimensões de Escopo

- **Projeto** — A permissão vale apenas dentro do projeto
- **Grupo** — A permissão vale apenas para o grupo
- **Temporária** — Com data de expiração

## Verificação

### Camada 1: Atributo no Controller

```csharp
[RequirePermission(SystemPermissions.ProvidersView)]
public async Task<IActionResult> GetProviders() { ... }
```

### Camada 2: Verificação Programática

```csharp
var allowed = await _roleService.HasPermissionAsync(userId, "providers:create");
```

## Sincronização Automática

Na inicialização, o `RoleSeedService` sincroniza os papéis de sistema com o banco:
- Papel novo → criado com permissões
- Permissões alteradas → sincronizadas
- Inalterado → nenhuma ação

**Nenhuma migration é necessária.**

## Telas Relacionadas

- [Papéis (Role Profiles)](../screens.md#papéis-role-profiles-adminroles)
- [Permissões](../screens.md#permissões-adminpermissions)
- [Usuários](../screens.md#usuários-adminusers)

## Documentos Relacionados

- [Permissões e Papéis (completo)](../permissions-and-roles.md)
