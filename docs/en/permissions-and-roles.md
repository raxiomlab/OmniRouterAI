# Permissions and Roles (RBAC)

## Overview

OmniRouterAI uses a role-based access control (RBAC) system with granular permissions. Permissions are constant strings defined in code, and roles are automatically synchronized with the database on startup.

## Permissions

### Providers

| Constant | Value | Description |
|---|---|---|
| `ProvidersView` | `providers:view` | View AI providers |
| `ProvidersCreate` | `providers:create` | Create new providers |
| `ProvidersEdit` | `providers:edit` | Edit existing providers |
| `ProvidersDelete` | `providers:delete` | Delete providers |

### Models

| Constant | Value | Description |
|---|---|---|
| `ModelsView` | `models:view` | View AI models |
| `ModelsCreate` | `models:create` | Create new models |
| `ModelsEdit` | `models:edit` | Edit models |
| `ModelsDelete` | `models:delete` | Delete models |

### Projects

| Constant | Value | Description |
|---|---|---|
| `ProjectsView` | `projects:view` | View projects |
| `ProjectsCreate` | `projects:create` | Create projects |
| `ProjectsEdit` | `projects:edit` | Edit projects |
| `ProjectsDelete` | `projects:delete` | Delete projects |

### API Keys

| Constant | Value | Description |
|---|---|---|
| `ApiKeysView` | `apikeys:view` | View API keys |
| `ApiKeysCreate` | `apikeys:create` | Create API keys |
| `ApiKeysEdit` | `apikeys:edit` | Edit keys |
| `ApiKeysRevoke` | `apikeys:revoke` | Revoke keys |
| `ApiKeysDelete` | `apikeys:delete` | Delete keys |

### Users

| Constant | Value | Description |
|---|---|---|
| `UsersView` | `users:view` | View users |
| `UsersCreate` | `users:create` | Create users |
| `UsersEdit` | `users:edit` | Edit user details |
| `UsersDelete` | `users:delete` | Delete users |
| `UsersManage` | `users:manage` | Manage users (general) |
| `UsersManageRoles` | `users:manage-roles` | Manage role assignments |

### Groups

| Constant | Value | Description |
|---|---|---|
| `GroupsView` | `groups:view` | View groups |
| `GroupsCreate` | `groups:create` | Create groups |
| `GroupsEdit` | `groups:edit` | Edit groups |
| `GroupsDelete` | `groups:delete` | Delete groups |
| `GroupsManageMembers` | `groups:manage-members` | Manage group members |

### Permissions (system)

| Constant | Value | Description |
|---|---|---|
| `PermissionsView` | `permissions:view` | View permissions |
| `PermissionsGrant` | `permissions:grant` | Grant permissions |
| `PermissionsRevoke` | `permissions:revoke` | Revoke permissions |

### Security

| Constant | Value | Description |
|---|---|---|
| `SecurityView` | `security:view` | View security config |
| `SecurityEdit` | `security:edit` | Edit security config |

### Consumption

| Constant | Value | Description |
|---|---|---|
| `ConsumptionViewOwn` | `consumption:view:own` | View own consumption |
| `ConsumptionViewTeam` | `consumption:view:team` | View team consumption |
| `ConsumptionViewGroup` | `consumption:view:group` | View group consumption |
| `ConsumptionViewAll` | `consumption:view:all` | View all consumption |

### Rate Limits

| Constant | Value | Description |
|---|---|---|
| `RateLimitsView` | `ratelimits:view` | View rate limits |
| `RateLimitsEdit` | `ratelimits:edit` | Edit rate limits |

### Settings

| Constant | Value | Description |
|---|---|---|
| `SettingsView` | `settings:view` | View settings |
| `SettingsEdit` | `settings:edit` | Edit settings |

### Audit

| Constant | Value | Description |
|---|---|---|
| `AuditView` | `audit:view` | View audit logs |
| `AuditExport` | `audit:export` | Export audit logs |

### Administration

| Constant | Value | Description |
|---|---|---|
| `AdminAccess` | `admin:access` | Administrative access |
| `RoleProfilesManage` | `roles:manage` | Manage role profiles |
| `InsightsView` | `insights:view` | View insights |

### MCP (Model Context Protocol)

| Constant | Value | Description |
|---|---|---|
| `McpAccess` | `mcp:access` | Access MCP |
| `McpServersView` | `mcp:servers:view` | View MCP servers |
| `McpServersCreate` | `mcp:servers:create` | Create MCP servers |
| `McpServersEdit` | `mcp:servers:edit` | Edit MCP servers |
| `McpServersDelete` | `mcp:servers:delete` | Delete MCP servers |
| `McpServersManageVisibility` | `mcp:servers:manage-visibility` | Manage MCP visibility |
| `McpToolsCall` | `mcp:tools:call` | Call MCP tools |

### Secret Vault

| Constant | Value | Description |
|---|---|---|
| `SecretsView` | `secrets:view` | View secrets |
| `SecretsCreate` | `secrets:create` | Create secrets |
| `SecretsEdit` | `secrets:edit` | Edit secrets |
| `SecretsDelete` | `secrets:delete` | Delete secrets |

### Retry

| Constant | Value | Description |
|---|---|---|
| `RetryView` | `retry:view` | View retry configs |
| `RetryEdit` | `retry:edit` | Edit retry configs |

## System Roles

### Administrator

- **ID:** `b1eebc99-9c0b-4ef8-bb6d-6bb9bd380a22`
- **Description:** Full system access — cannot be deleted
- **Permissions:** All 59 permissions

### User

- **ID:** `b2eebc99-9c0b-4ef8-bb6d-6bb9bd380a23`
- **Description:** Standard platform access
- **Permissions (15):**
  - `providers:view`, `models:view`
  - `projects:view`, `projects:create`, `projects:edit`, `projects:delete`
  - `apikeys:view`, `apikeys:create`, `apikeys:edit`, `apikeys:revoke`, `apikeys:delete`
  - `consumption:view:own`
  - `security:view`
  - `permissions:view`
  - `mcp:servers:view`

### Manager

- **ID:** `b3eebc99-9c0b-4ef8-bb6d-6bb9bd380a24`
- **Description:** Can view team consumption and manage group members
- **Permissions (21):**
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
- **Description:** Read-only access across all modules
- **Permissions (9):**
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
- **Description:** Can access MCP servers and call tools
- **Permissions (3):**
  - `mcp:access`
  - `mcp:servers:view`
  - `mcp:tools:call`

## Custom Roles

Custom roles can be created via API:

```http
POST /api/role-profiles
{
  "name": "DevOps Lead",
  "description": "Can manage providers and view all consumption",
  "permissions": ["providers:view", "providers:create", "consumption:view:all"]
}
```

### System vs Custom Roles

| Feature | System Role | Custom Role |
|---|---|---|
| ID Origin | Fixed in code | Generated on creation |
| `IsSystem` | `true` | `false` |
| Deletable via API? | No | Yes |
| Versioned in Git? | Yes | No |
| Auto-seeded? | Yes (startup) | No (API only) |
| Editable? | Yes (except deletion) | Yes |

## Role Assignment

### Simple Assignment (Global)

```http
POST /api/role-profiles/assign
{
  "userId": "user-uuid",
  "roleProfileId": "b1eebc99-9c0b-4ef8-bb6d-6bb9bd380a22"
}
```

### Scoped Assignment

```http
POST /api/role-profiles/assign
{
  "userId": "user-uuid",
  "roleProfileId": "b2eebc99-9c0b-4ef8-bb6d-6bb9bd380a23",
  "projectId": "project-uuid",
  "expiresAt": "2026-12-31T23:59:59Z"
}
```

### Scope Dimensions

| Field | Required | Description |
|---|---|---|
| `userId` | Yes | User receiving the role |
| `roleProfileId` | Yes | Role to assign |
| `projectId` | No | Project scope |
| `groupId` | No | Group scope |
| `expiresAt` | No | Time-bound assignment |

## Permission Verification

### Layer 1: Declarative Attribute (Controller)

```csharp
[RequirePermission(SystemPermissions.ProvidersView)]
public async Task<IActionResult> GetProviders() { ... }
```

Administrators have automatic access to all protected endpoints.

### Layer 2: Programmatic Check (Service)

```csharp
var allowed = await _roleService.HasPermissionAsync(userId, "providers:create");
var allowedScoped = await _roleService.HasPermissionAsync(userId, "consumption:view:own", projectId: projectId);
```

## Auto-Synchronization

On startup, `RoleSeedService` synchronizes system roles with the database:

- New role in code but not in DB → created with permissions
- Changed permissions → synced (removed/added)
- Unchanged role → no action (idempotent)

**No migration required** for system role changes.
