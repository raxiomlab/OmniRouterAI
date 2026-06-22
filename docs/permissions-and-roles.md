# Permissões e Papéis (RBAC)

## Visão Geral

O OmniRouterAI utiliza um sistema de controle de acesso baseado em papéis (RBAC) com permissões granulares. As permissões são strings constantes definidas em código, e os papéis são sincronizados automaticamente com o banco de dados na inicialização.

## Permissões

### Provedores

| Constante | Valor | Descrição |
|---|---|---|
| `ProvidersView` | `providers:view` | Visualizar provedores de IA |
| `ProvidersCreate` | `providers:create` | Criar novos provedores |
| `ProvidersEdit` | `providers:edit` | Editar provedores existentes |
| `ProvidersDelete` | `providers:delete` | Excluir provedores |

### Modelos

| Constante | Valor | Descrição |
|---|---|---|
| `ModelsView` | `models:view` | Visualizar modelos de IA |
| `ModelsCreate` | `models:create` | Criar novos modelos |
| `ModelsEdit` | `models:edit` | Editar modelos |
| `ModelsDelete` | `models:delete` | Excluir modelos |

### Projetos

| Constante | Valor | Descrição |
|---|---|---|
| `ProjectsView` | `projects:view` | Visualizar projetos |
| `ProjectsCreate` | `projects:create` | Criar projetos |
| `ProjectsEdit` | `projects:edit` | Editar projetos |
| `ProjectsDelete` | `projects:delete` | Excluir projetos |

### API Keys

| Constante | Valor | Descrição |
|---|---|---|
| `ApiKeysView` | `apikeys:view` | Visualizar chaves de API |
| `ApiKeysCreate` | `apikeys:create` | Criar chaves de API |
| `ApiKeysEdit` | `apikeys:edit` | Editar chaves |
| `ApiKeysRevoke` | `apikeys:revoke` | Revogar chaves |
| `ApiKeysDelete` | `apikeys:delete` | Excluir chaves |

### Usuários

| Constante | Valor | Descrição |
|---|---|---|
| `UsersView` | `users:view` | Visualizar usuários |
| `UsersCreate` | `users:create` | Criar usuários |
| `UsersEdit` | `users:edit` | Editar dados do usuário |
| `UsersDelete` | `users:delete` | Excluir usuários |
| `UsersManage` | `users:manage` | Gerenciar usuários (geral) |
| `UsersManageRoles` | `users:manage-roles` | Gerenciar atribuição de papéis |

### Grupos

| Constante | Valor | Descrição |
|---|---|---|
| `GroupsView` | `groups:view` | Visualizar grupos |
| `GroupsCreate` | `groups:create` | Criar grupos |
| `GroupsEdit` | `groups:edit` | Editar grupos |
| `GroupsDelete` | `groups:delete` | Excluir grupos |
| `GroupsManageMembers` | `groups:manage-members` | Gerenciar membros do grupo |

### Permissões (do sistema)

| Constante | Valor | Descrição |
|---|---|---|
| `PermissionsView` | `permissions:view` | Visualizar permissões |
| `PermissionsGrant` | `permissions:grant` | Conceder permissões |
| `PermissionsRevoke` | `permissions:revoke` | Revogar permissões |

### Segurança

| Constante | Valor | Descrição |
|---|---|---|
| `SecurityView` | `security:view` | Visualizar configuração de segurança |
| `SecurityEdit` | `security:edit` | Editar configuração de segurança |

### Consumo

| Constante | Valor | Descrição |
|---|---|---|
| `ConsumptionViewOwn` | `consumption:view:own` | Visualizar próprio consumo |
| `ConsumptionViewTeam` | `consumption:view:team` | Visualizar consumo do time |
| `ConsumptionViewGroup` | `consumption:view:group` | Visualizar consumo do grupo |
| `ConsumptionViewAll` | `consumption:view:all` | Visualizar todo o consumo |

### Rate Limits

| Constante | Valor | Descrição |
|---|---|---|
| `RateLimitsView` | `ratelimits:view` | Visualizar limites de taxa |
| `RateLimitsEdit` | `ratelimits:edit` | Editar limites de taxa |

### Configurações

| Constante | Valor | Descrição |
|---|---|---|
| `SettingsView` | `settings:view` | Visualizar configurações |
| `SettingsEdit` | `settings:edit` | Editar configurações |

### Auditoria

| Constante | Valor | Descrição |
|---|---|---|
| `AuditView` | `audit:view` | Visualizar logs de auditoria |
| `AuditExport` | `audit:export` | Exportar logs de auditoria |

### Administração

| Constante | Valor | Descrição |
|---|---|---|
| `AdminAccess` | `admin:access` | Acesso administrativo |
| `RoleProfilesManage` | `roles:manage` | Gerenciar papéis |
| `InsightsView` | `insights:view` | Visualizar insights |

### MCP (Model Context Protocol)

| Constante | Valor | Descrição |
|---|---|---|
| `McpAccess` | `mcp:access` | Acessar MCP |
| `McpServersView` | `mcp:servers:view` | Visualizar servidores MCP |
| `McpServersCreate` | `mcp:servers:create` | Criar servidores MCP |
| `McpServersEdit` | `mcp:servers:edit` | Editar servidores MCP |
| `McpServersDelete` | `mcp:servers:delete` | Excluir servidores MCP |
| `McpServersManageVisibility` | `mcp:servers:manage-visibility` | Gerenciar visibilidade de MCP |
| `McpToolsCall` | `mcp:tools:call` | Executar ferramentas MCP |

### Secret Vault

| Constante | Valor | Descrição |
|---|---|---|
| `SecretsView` | `secrets:view` | Visualizar segredos |
| `SecretsCreate` | `secrets:create` | Criar segredos |
| `SecretsEdit` | `secrets:edit` | Editar segredos |
| `SecretsDelete` | `secrets:delete` | Excluir segredos |

### Retry

| Constante | Valor | Descrição |
|---|---|---|
| `RetryView` | `retry:view` | Visualizar configurações de retry |
| `RetryEdit` | `retry:edit` | Editar configurações de retry |

## Papéis de Sistema

### Administrador

- **ID:** `b1eebc99-9c0b-4ef8-bb6d-6bb9bd380a22`
- **Descrição:** Acesso total ao sistema — não pode ser excluído
- **Permissões:** Todas as 59 permissões

### User

- **ID:** `b2eebc99-9c0b-4ef8-bb6d-6bb9bd380a23`
- **Descrição:** Acesso padrão à plataforma
- **Permissões:**
  - `providers:view`, `models:view`
  - `projects:view`, `projects:create`, `projects:edit`, `projects:delete`
  - `apikeys:view`, `apikeys:create`, `apikeys:edit`, `apikeys:revoke`, `apikeys:delete`
  - `consumption:view:own`
  - `security:view`
  - `permissions:view`
  - `mcp:servers:view`

### Manager

- **ID:** `b3eebc99-9c0b-4ef8-bb6d-6bb9bd380a24`
- **Descrição:** Pode visualizar consumo do time e gerenciar membros de grupos
- **Permissões:**
  - `providers:view`, `models:view`
  - `projects:view`, `projects:create`, `projects:edit`, `projects:delete`
  - `apikeys:view`, `apikeys:create`, `apikeys:edit`, `apikeys:revoke`, `apikeys:delete`
  - `groups:view`, `groups:manage-members`
  - `consumption:view:own`, `consumption:view:team`, `consumption:view:group`
  - `security:view`
  - `ratelimits:view`
  - `insights:view`
  - `permissions:view`
  - `audit:view`

### Viewer

- **ID:** `b4eebc99-9c0b-4ef8-bb6d-6bb9bd380a25`
- **Descrição:** Acesso somente leitura a todos os módulos
- **Permissões:**
  - `providers:view`, `models:view`
  - `projects:view`
  - `apikeys:view`
  - `consumption:view:own`
  - `security:view`
  - `permissions:view`
  - `audit:view`
  - `mcp:servers:view`

### MCP User

- **ID:** `b5eebc99-9c0b-4ef8-bb6d-6bb9bd380a26`
- **Descrição:** Pode acessar servidores MCP e executar ferramentas
- **Permissões:**
  - `mcp:access`
  - `mcp:servers:view`
  - `mcp:tools:call`

## Papéis Customizados

Além dos papéis de sistema, é possível criar papéis customizados via API:

```http
POST /api/role-profiles
{
  "name": "DevOps Lead",
  "description": "Can manage providers and view all consumption",
  "permissions": ["providers:view", "providers:create", "consumption:view:all"]
}
```

### Diferenças entre Papéis de Sistema e Customizados

| Característica | Papel de Sistema | Papel Customizado |
|---|---|---|
| Origem do ID | Fixo em código | Gerado na criação |
| `IsSystem` | `true` | `false` |
| Pode ser excluído via API? | Não | Sim |
| Versionado no Git? | Sim | Não |
| Seed automático? | Sim (startup) | Não (apenas via API) |
| Editável? | Sim (menos exclusão) | Sim |

## Atribuição de Papéis

### Atribuição Simples (Global)

```http
POST /api/role-profiles/assign
{
  "userId": "uuid-do-usuario",
  "roleProfileId": "b1eebc99-9c0b-4ef8-bb6d-6bb9bd380a22"
}
```

### Atribuição com Escopo

```http
POST /api/role-profiles/assign
{
  "userId": "uuid-do-usuario",
  "roleProfileId": "b2eebc99-9c0b-4ef8-bb6d-6bb9bd380a23",
  "projectId": "uuid-do-projeto",
  "expiresAt": "2026-12-31T23:59:59Z"
}
```

### Dimensões de Escopo

| Campo | Obrigatório | Descrição |
|---|---|---|
| `userId` | Sim | Usuário que recebe o papel |
| `roleProfileId` | Sim | Papel a ser atribuído |
| `projectId` | Não | Escopo por projeto |
| `groupId` | Não | Escopo por grupo |
| `expiresAt` | Não | Atribuição temporária |

## Verificação de Permissão

### Camada 1: Atributo Declarativo (Controller)

```csharp
[RequirePermission(SystemPermissions.ProvidersView)]
public async Task<IActionResult> GetProviders() { ... }
```

Administradores têm acesso automático a todos os endpoints protegidos.

### Camada 2: Verificação Programática (Service)

```csharp
var allowed = await _roleService.HasPermissionAsync(userId, "providers:create");
var allowedScoped = await _roleService.HasPermissionAsync(userId, "consumption:view:own", projectId: projectId);
```

## Sincronização Automática

Na inicialização, o `RoleSeedService` sincroniza os papéis de sistema com o banco de dados:

- Papel novo no código mas não no banco → criado com permissões
- Permissões alteradas → sincronizadas (removidas/adiocionadas)
- Papel inalterado → nenhuma ação (idempotente)

**Nenhuma migration é necessária** para alterações em papéis de sistema.
