# API Endpoints Reference

## Authentication

| Method | Route | Auth | Description |
|---|---|---|---|
| POST | `/api/auth/login` | Anonymous (+ Rate Limit) | Local login with email and password |
| POST | `/api/auth/refresh` | JWT | Refresh JWT token |
| GET | `/api/auth/me` | JWT | Get authenticated user info |
| GET | `/api/auth/microsoft` | Anonymous | Redirect to Microsoft OAuth |
| GET | `/api/auth/microsoft/callback` | Anonymous | Microsoft OAuth callback |
| GET | `/api/auth/oauth-token` | Anonymous | Read OAuth token from cookie (one-time) |

## AI Proxy

| Method | Route | Auth | Description |
|---|---|---|---|
| POST | `/v1/chat/completions` | API Key | Chat completion proxy (OpenAI-compatible) |
| GET | `/v1/models` | API Key | List accessible models |
| GET | `/v1/providers/health` | API Key | Provider health status |

## Projects

| Method | Route | Permission | Description |
|---|---|---|---|
| GET | `/api/projects` | `projects:view` | List user projects |
| GET | `/api/projects/{id}` | `projects:view` | Get project by ID |
| POST | `/api/projects` | `projects:create` | Create project |
| PUT | `/api/projects/{id}` | `projects:edit` | Update project |
| DELETE | `/api/projects/{id}` | `projects:delete` | Delete project |
| GET | `/api/projects/{projectId}/groups` | `projects:view` | List group bindings |
| POST | `/api/projects/{projectId}/groups` | `projects:edit` | Add group binding |
| DELETE | `/api/projects/{projectId}/groups/{groupId}` | `projects:edit` | Remove group binding |
| GET | `/api/projects/{projectId}/keys` | `apikeys:view` | List project keys |
| POST | `/api/projects/{projectId}/keys` | `apikeys:create` | Create project key |
| POST | `/api/projects/{projectId}/keys/{keyId}/revoke` | `apikeys:revoke` | Revoke key |
| DELETE | `/api/projects/{projectId}/keys/{keyId}` | `apikeys:delete` | Delete key |
| GET | `/api/projects/{projectId}/members` | `projects:view` | List project members |
| POST | `/api/projects/{projectId}/members` | `projects:edit` | Add member |
| DELETE | `/api/projects/{projectId}/members/{memberId}` | `projects:edit` | Remove member |
| GET | `/api/projects/{projectId}/usage` | `consumption:view:own` | Project usage |

## API Keys

| Method | Route | Permission | Description |
|---|---|---|---|
| GET | `/api/api-keys` | `apikeys:view` | List user keys |
| GET | `/api/api-keys/{id}` | `apikeys:view` | Get key by ID |
| POST | `/api/api-keys` | `apikeys:create` | Create key |
| DELETE | `/api/api-keys/{id}` | `apikeys:delete` | Delete key |
| POST | `/api/api-keys/{id}/revoke` | `apikeys:revoke` | Revoke key |

## Providers

| Method | Route | Permission | Description |
|---|---|---|---|
| GET | `/api/providers` | `providers:view` | List providers |
| POST | `/api/providers` | `providers:create` | Create provider |
| PUT | `/api/providers/{id}` | `providers:edit` | Update provider |
| PATCH | `/api/providers/{id}/status` | `providers:edit` | Toggle provider active/inactive |
| DELETE | `/api/providers/{id}` | `providers:delete` | Delete provider |
| POST | `/api/providers/fetch-models` | `providers:edit` | Fetch models from provider |
| POST | `/api/providers/test-connection` | `providers:edit` | Test provider connection |
| GET | `/api/providers/{id}/visibility` | `providers:view` | Get provider visibility |
| PUT | `/api/providers/{id}/visibility` | `providers:edit` | Set provider visibility |
| POST | `/api/providers/{id}/smart-routing/test` | `providers:view` | Test smart routing rules |

## Models

| Method | Route | Permission | Description |
|---|---|---|---|
| GET | `/api/models` | `models:view` | List visible models |

## Admin

| Method | Route | Permission | Description |
|---|---|---|---|
| GET | `/api/admin/providers` | `admin:access` | List providers (admin) |
| POST | `/api/admin/providers` | `admin:access` | Create provider (admin) |
| PUT | `/api/admin/providers/{id}` | `admin:access` | Update provider (admin) |
| DELETE | `/api/admin/providers/{id}` | `admin:access` | Delete provider (admin) |
| GET | `/api/admin/rate-policies` | `admin:access` | List rate limit policies |
| PUT | `/api/admin/rate-policies` | `admin:access` | Upsert rate policy |
| DELETE | `/api/admin/rate-policies/{id}` | `admin:access` | Delete rate policy |
| GET | `/api/admin/usage` | `admin:access` | Global usage stats |
| GET | `/api/admin/settings` | `admin:access` | App settings |
| PUT | `/api/admin/settings` | `admin:access` | Update settings |
| POST | `/api/admin/providers/test` | `admin:access` | Test provider connection |
| POST | `/api/admin/providers/fetch-models` | `admin:access` | Fetch provider models |
| POST | `/api/admin/providers/test-inference` | `admin:access` | Test inference |

## Users (Admin)

| Method | Route | Permission | Description |
|---|---|---|---|
| GET | `/api/admin/users` | `users:manage` | List users |
| GET | `/api/admin/users/{id}` | `users:manage` | Get user |
| POST | `/api/admin/users` | `users:create` | Create user |
| PUT | `/api/admin/users/{id}` | `users:edit` | Update user |
| DELETE | `/api/admin/users/{id}` | `users:delete` | Delete user |

## Groups

| Method | Route | Permission | Description |
|---|---|---|---|
| GET | `/api/groups` | `groups:view` | List groups |
| GET | `/api/groups/{id}` | `groups:view` | Get group |
| POST | `/api/groups` | `groups:create` | Create group |
| PUT | `/api/groups/{id}` | `groups:edit` | Update group |
| DELETE | `/api/groups/{id}` | `groups:delete` | Delete group |
| POST | `/api/groups/{groupId}/members` | `groups:manage-members` | Add member |
| DELETE | `/api/groups/{groupId}/members/{userId}` | `groups:manage-members` | Remove member |
| GET | `/api/groups/{groupId}/members` | `groups:view` | List members |

## Role Profiles

| Method | Route | Permission | Description |
|---|---|---|---|
| GET | `/api/role-profiles` | `roles:manage` | List roles |
| GET | `/api/role-profiles/{id}` | `roles:manage` | Get role |
| POST | `/api/role-profiles` | `roles:manage` | Create role |
| PUT | `/api/role-profiles/{id}` | `roles:manage` | Update role |
| DELETE | `/api/role-profiles/{id}` | `roles:manage` | Delete role |
| POST | `/api/role-profiles/assign` | `users:manage-roles` | Assign role to user |
| DELETE | `/api/role-profiles/assign/{assignmentId}` | `users:manage-roles` | Remove assignment |
| GET | `/api/role-profiles/user/{userId}/assignments` | `users:view` | User assignments |
| GET | `/api/role-profiles/my-permissions` | `permissions:view` | My permissions |
| GET | `/api/role-profiles/my-check/{permissionKey}` | `permissions:view` | Check permission |

## Permissions

| Method | Route | Permission | Description |
|---|---|---|---|
| GET | `/api/permissions/metadata` | `permissions:view` | List available permissions |
| GET | `/api/permissions/my-providers` | `providers:view` | My visible providers |
| GET | `/api/permissions/my-mcp-servers` | `mcp:access` | My visible MCP servers |
| POST | `/api/permissions/grant-user` | `permissions:grant` | Grant permission to user |
| POST | `/api/permissions/grant-group` | `permissions:grant` | Grant permission to group |
| DELETE | `/api/permissions/user/{userId}/{providerId}` | `permissions:revoke` | Revoke user permission |
| DELETE | `/api/permissions/group/{groupId}/{providerId}` | `permissions:revoke` | Revoke group permission |
| GET | `/api/permissions/user/{userId}` | `permissions:view` | User permissions |
| GET | `/api/permissions/group/{groupId}` | `permissions:view` | Group permissions |

## Visibility

| Method | Route | Permission | Description |
|---|---|---|---|
| GET | `/api/visibility/group/{groupId}/providers` | `permissions:view` | Provider visibility by group |
| PUT | `/api/visibility/providers/{providerId}/groups` | `permissions:grant` | Toggle provider visibility |
| GET | `/api/visibility/group/{groupId}/mcp-servers` | `mcp:servers:view` | MCP visibility by group |
| PUT | `/api/visibility/mcp-servers/{serverId}/groups` | `mcp:servers:manage-visibility` | Toggle MCP visibility |
| GET | `/api/visibility/group/{groupId}/projects` | `projects:view` | Project visibility by group |
| PUT | `/api/visibility/projects/{projectId}/groups` | `projects:edit` | Toggle project-group binding |

## MCP Gateway

| Method | Route | Auth | Description |
|---|---|---|---|
| POST | `/v1/mcp` | API Key | JSON-RPC MCP (streamable HTTP) |
| POST | `/mcp` | API Key | JSON-RPC MCP (alias) |
| POST | `/v1/mcp/{slug}` | API Key | JSON-RPC for specific server |
| POST | `/mcp/{slug}` | API Key | JSON-RPC for specific server (alias) |
| GET | `/v1/mcp/tools/list` | API Key | List tools (REST) |
| GET | `/api/mcp/tools/visible` | API Key | Visible tools |
| POST | `/v1/mcp/tools/call` | API Key | Call tool (API Key) |
| POST | `/api/mcp/tools/call` | JWT | Call tool (JWT) |
| GET | `/v1/mcp/servers` | API Key | List servers |
| GET | `/api/mcp/tools` | Anonymous | Public tools |
| GET | `/api/mcp/visible-servers` | JWT | Visible servers |
| GET | `/api/mcp/discovery` | JWT | Full tool discovery |
| GET | `/v1/mcp/{slug}/tools/list` | API Key | Server tools |
| POST | `/v1/mcp/{slug}/tools/call` | API Key | Call server tool |
| GET | `/v1/mcp/{slug}/servers` | API Key | Server info by slug |

## MCP Admin

| Method | Route | Permission | Description |
|---|---|---|---|
| GET | `/api/mcp/servers` | `mcp:servers:view` | List MCP servers |
| POST | `/api/mcp/servers` | `mcp:servers:create` | Create MCP server |
| PUT | `/api/mcp/servers/{id}` | `mcp:servers:edit` | Update MCP server |
| DELETE | `/api/mcp/servers/{id}` | `mcp:servers:delete` | Delete MCP server |
| GET | `/api/mcp/servers/{id}/visibility` | `mcp:servers:manage-visibility` | Get visibility |
| PUT | `/api/mcp/servers/{id}/visibility` | `mcp:servers:manage-visibility` | Set visibility |
| POST | `/api/mcp/servers/sync` | `mcp:servers:create` | Sync plugins |
| POST | `/api/mcp/servers/{id}/discover` | `mcp:servers:edit` | Discover tools |
| GET | `/api/mcp/servers/{id}/tools` | `mcp:servers:view` | List server tools |
| POST | `/api/mcp/servers/{id}/import-openapi` | `mcp:servers:edit` | Import OpenAPI |

## MCP Builder

| Method | Route | Permission | Description |
|---|---|---|---|
| POST | `/api/mcp/builder/parse` | `mcp:servers:create` | Parse definitions into tools |
| POST | `/api/mcp/builder/preview` | `mcp:servers:create` | Preview MCP artifacts |
| POST | `/api/mcp/builder/deploy` | `mcp:servers:create` | Deploy MCP server |
| POST | `/api/mcp/builder/import-config` | `mcp:servers:create` | Import JSON config |

## OpenAPI Converter

| Method | Route | Permission | Description |
|---|---|---|---|
| POST | `/api/mcp/converter/discover` | `mcp:servers:create` | Discover OpenAPI spec from URL |
| POST | `/api/mcp/converter/parse` | `mcp:servers:create` | Parse OpenAPI content |
| POST | `/api/mcp/converter/preview` | `mcp:servers:create` | Preview conversion |
| POST | `/api/mcp/converter/deploy` | `mcp:servers:create` | Deploy as MCP server |
| GET | `/api/mcp/converter/projects` | `mcp:servers:view` | List converter projects |
| GET | `/api/mcp/converter/projects/{id}` | `mcp:servers:view` | Get converter project |
| PUT | `/api/mcp/converter/projects/{id}` | `mcp:servers:create` | Update project |
| DELETE | `/api/mcp/converter/projects/{id}` | `mcp:servers:create` | Delete project |
| POST | `/api/mcp/converter/projects/export` | `mcp:servers:create` | Export as JSON |
| POST | `/api/mcp/converter/projects/import` | `mcp:servers:create` | Import JSON project |
| GET | `/api/mcp/converter/projects/by-server/{serverId}` | `mcp:servers:view` | Project by server ID |

## Analytics

| Method | Route | Permission | Description |
|---|---|---|---|
| GET | `/api/analytics/overview` | `insights:view` | Aggregate stats |
| GET | `/api/analytics/daily` | `insights:view` | Daily usage |
| GET | `/api/analytics/providers` | `insights:view` | Usage by provider |
| GET | `/api/analytics/models` | `insights:view` | Usage by model |
| GET | `/api/analytics/matrix` | `insights:view` | Provider-model matrix |
| GET | `/api/analytics/group/{groupId}` | `consumption:view:group` | Analytics by group |
| GET | `/api/analytics/own` | `consumption:view:own` | Own consumption |

## Team Consumption

| Method | Route | Permission | Description |
|---|---|---|---|
| GET | `/api/team-consumption` | `consumption:view:own` | Consumption (own/team/all) |
| GET | `/api/team-consumption/my-team` | `consumption:view:own` | Team consumption |
| GET | `/api/team-consumption/my-groups` | `consumption:view:group` | Group consumption |
| GET | `/api/team-consumption/by-project/{projectId}` | `consumption:view:own` | By project |
| GET | `/api/team-consumption/by-provider/{providerName}` | `consumption:view:own` | By provider |
| GET | `/api/team-consumption/groups-by-project/{projectId}` | `consumption:view:group` | Group by project |
| GET | `/api/team-consumption/groups-by-provider/{providerName}` | `consumption:view:group` | Group by provider |
| GET | `/api/team-consumption/by-projects` | `consumption:view:own` | Grouped by project |
| GET | `/api/team-consumption/user/{targetUserId}` | `consumption:view:own` | Specific user |

## Security

| Method | Route | Permission | Description |
|---|---|---|---|
| GET | `/api/security/ip` | `security:view` | IP rules |
| PUT | `/api/security/ip` | `security:edit` | Upsert IP rules |
| DELETE | `/api/security/ip/{id}` | `security:edit` | Delete IP rule |
| GET | `/api/security/geo` | `security:view` | Geo rules |
| PUT | `/api/security/geo` | `security:edit` | Upsert geo rules |
| DELETE | `/api/security/geo/{id}` | `security:edit` | Delete geo rule |
| GET | `/api/security/status` | `security:view` | Security status |

## Secret Vault

| Method | Route | Permission | Description |
|---|---|---|---|
| GET | `/api/secrets` | `secrets:view` | List secrets |
| GET | `/api/secrets/{id}` | `secrets:view` | Get secret |
| POST | `/api/secrets` | `secrets:create` | Create secret |
| PUT | `/api/secrets/{id}` | `secrets:edit` | Update secret |
| GET | `/api/secrets/env-health` | `secrets:view` | Env var health check |
| DELETE | `/api/secrets/{id}` | `secrets:delete` | Delete secret |

## Settings

| Method | Route | Permission | Description |
|---|---|---|---|
| GET | `/api/settings` | `settings:view` | Get settings |
| PUT | `/api/settings` | `settings:edit` | Update settings |

## Audit

| Method | Route | Permission | Description |
|---|---|---|---|
| GET | `/api/admin/audit` | `audit:view` | Audit logs |
| GET | `/api/admin/audit/{id}` | `audit:view` | Audit detail |
| GET | `/api/admin/audit/entities` | `audit:view` | Audited entity names |

## Global Usage

| Method | Route | Permission | Description |
|---|---|---|---|
| GET | `/api/usage` | `admin:access` | Global usage stats |
| GET | `/api/logs` | `consumption:view:all` | Request logs |

## Health Check

| Method | Route | Auth | Description |
|---|---|---|---|
| GET | `/health` | None | Provider health check |
