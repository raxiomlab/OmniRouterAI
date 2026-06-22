# Access Control (RBAC)

Complete role-based access control system with granular permissions, system and custom roles, and scoped assignments.

## System Roles

![Roles](../../assets/roles.png)

Five fixed roles defined in code, automatically synchronized with the database on startup:

| Role | Access |
|---|---|
| **Administrator** | Full — all 59 permissions |
| **Manager** | Manages groups, views team consumption and insights |
| **User** | Standard — projects, API keys, own consumption |
| **Viewer** | Read-only across all modules |
| **MCP User** | Exclusive MCP server access and tool execution |

## Permissions

59 permissions organized by category:

| Category | Examples |
|---|---|
| **Providers** | `providers:view`, `providers:create`, `providers:edit`, `providers:delete` |
| **Models** | `models:view`, `models:create`, `models:edit`, `models:delete` |
| **Projects** | `projects:view`, `projects:create`, `projects:edit`, `projects:delete` |
| **API Keys** | `apikeys:view`, `apikeys:create`, `apikeys:revoke`, `apikeys:delete` |
| **Users** | `users:view`, `users:create`, `users:edit`, `users:delete`, `users:manage-roles` |
| **Groups** | `groups:view`, `groups:create`, `groups:manage-members` |
| **MCP** | `mcp:servers:view`, `mcp:servers:create`, `mcp:tools:call`, `mcp:access` |
| **Consumption** | `consumption:view:own`, `consumption:view:team`, `consumption:view:all` |
| **Security** | `security:view`, `security:edit` |
| **Settings** | `settings:view`, `settings:edit` |
| **Audit** | `audit:view`, `audit:export` |

## Custom Roles

![Permissions](../../assets/permissions.png)

Custom roles can be created with any permission combination:

```http
POST /api/role-profiles
{
  "name": "DevOps Lead",
  "permissions": ["providers:view", "providers:create", "consumption:view:all"]
}
```

### Differences

| Feature | System Role | Custom Role |
|---|---|---|
| ID Origin | Fixed in code | Generated on creation |
| Deletable via API | No | Yes |
| Versioned in Git | Yes | No |
| Auto-seeded | Yes | No |

## Assignment

Assignment can be global or scoped:

```http
POST /api/role-profiles/assign
{
  "userId": "user-uuid",
  "roleProfileId": "b1eebc99-...",
  "projectId": "project-uuid",
  "expiresAt": "2026-12-31T23:59:59Z"
}
```

### Scope Dimensions

- **Project** — Permission applies only within the project
- **Group** — Permission applies only to the group
- **Temporary** — With expiration date

## Verification

### Layer 1: Controller Attribute

```csharp
[RequirePermission(SystemPermissions.ProvidersView)]
public async Task<IActionResult> GetProviders() { ... }
```

### Layer 2: Programmatic Check

```csharp
var allowed = await _roleService.HasPermissionAsync(userId, "providers:create");
```

## Auto-Synchronization

On startup, `RoleSeedService` syncs system roles with the DB:
- New role → created with permissions
- Changed permissions → synced
- Unchanged → no action

**No migration required.**

## Related Screens

- [Roles (Role Profiles)](../screens.md#roles-adminroles)
- [Permissions](../screens.md#permissions-adminpermissions)
- [Users](../screens.md#users-adminusers)

## Related Docs

- [Permissions and Roles (full)](../permissions-and-roles.md)
