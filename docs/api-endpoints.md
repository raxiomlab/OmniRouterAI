# Referência de Endpoints da API

## Autenticação

| Método | Rota | Autenticação | Descrição |
|---|---|---|---|
| POST | `/api/auth/login` | Anônimo (+ Rate Limit) | Login local com e-mail e senha |
| POST | `/api/auth/refresh` | JWT | Renovar token JWT |
| GET | `/api/auth/me` | JWT | Dados do usuário autenticado |
| GET | `/api/auth/microsoft` | Anônimo | Redirecionar para OAuth Microsoft |
| GET | `/api/auth/microsoft/callback` | Anônimo | Callback do OAuth Microsoft |
| GET | `/api/auth/oauth-token` | Anônimo | Ler token OAuth do cookie (uso único) |

## Proxy de IA

| Método | Rota | Autenticação | Descrição |
|---|---|---|---|
| POST | `/v1/chat/completions` | API Key | Proxy de chat completion (compatível OpenAI) |
| GET | `/v1/models` | API Key | Listar modelos acessíveis pela chave |
| GET | `/v1/providers/health` | API Key | Status de saúde dos provedores |

## Projetos

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| GET | `/api/projects` | `projects:view` | Listar projetos do usuário |
| GET | `/api/projects/{id}` | `projects:view` | Obter projeto por ID |
| POST | `/api/projects` | `projects:create` | Criar projeto |
| PUT | `/api/projects/{id}` | `projects:edit` | Atualizar projeto |
| DELETE | `/api/projects/{id}` | `projects:delete` | Excluir projeto |
| GET | `/api/projects/{projectId}/groups` | `projects:view` | Vincular grupos ao projeto |
| POST | `/api/projects/{projectId}/groups` | `projects:edit` | Adicionar vínculo de grupo |
| DELETE | `/api/projects/{projectId}/groups/{groupId}` | `projects:edit` | Remover vínculo de grupo |
| GET | `/api/projects/{projectId}/keys` | `apikeys:view` | Listar chaves do projeto |
| POST | `/api/projects/{projectId}/keys` | `apikeys:create` | Criar chave para o projeto |
| POST | `/api/projects/{projectId}/keys/{keyId}/revoke` | `apikeys:revoke` | Revogar chave |
| DELETE | `/api/projects/{projectId}/keys/{keyId}` | `apikeys:delete` | Excluir chave |
| GET | `/api/projects/{projectId}/members` | `projects:view` | Listar membros do projeto |
| POST | `/api/projects/{projectId}/members` | `projects:edit` | Adicionar membro |
| DELETE | `/api/projects/{projectId}/members/{memberId}` | `projects:edit` | Remover membro |
| GET | `/api/projects/{projectId}/usage` | `consumption:view:own` | Consumo do projeto |

## API Keys

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| GET | `/api/api-keys` | `apikeys:view` | Listar chaves do usuário |
| GET | `/api/api-keys/{id}` | `apikeys:view` | Obter chave por ID |
| POST | `/api/api-keys` | `apikeys:create` | Criar chave |
| DELETE | `/api/api-keys/{id}` | `apikeys:delete` | Excluir chave |
| POST | `/api/api-keys/{id}/revoke` | `apikeys:revoke` | Revogar chave |

## Provedores

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| GET | `/api/providers` | `providers:view` | Listar provedores |
| POST | `/api/providers` | `providers:create` | Criar provedor |
| PUT | `/api/providers/{id}` | `providers:edit` | Atualizar provedor |
| PATCH | `/api/providers/{id}/status` | `providers:edit` | Ativar/desativar provedor |
| DELETE | `/api/providers/{id}` | `providers:delete` | Excluir provedor |
| POST | `/api/providers/fetch-models` | `providers:edit` | Buscar modelos do provedor |
| POST | `/api/providers/test-connection` | `providers:edit` | Testar conexão |
| GET | `/api/providers/{id}/visibility` | `providers:view` | Visibilidade do provedor |
| PUT | `/api/providers/{id}/visibility` | `providers:edit` | Alterar visibilidade |
| POST | `/api/providers/{id}/smart-routing/test` | `providers:view` | Testar regras de smart routing |

## Modelos

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| GET | `/api/models` | `models:view` | Listar modelos visíveis ao usuário |

## Administração Geral

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| GET | `/api/admin/providers` | `admin:access` | Listar provedores (admin) |
| POST | `/api/admin/providers` | `admin:access` | Criar provedor (admin) |
| PUT | `/api/admin/providers/{id}` | `admin:access` | Atualizar provedor (admin) |
| DELETE | `/api/admin/providers/{id}` | `admin:access` | Excluir provedor (admin) |
| GET | `/api/admin/rate-policies` | `admin:access` | Listar políticas de rate limit |
| PUT | `/api/admin/rate-policies` | `admin:access` | Criar/atualizar política |
| DELETE | `/api/admin/rate-policies/{id}` | `admin:access` | Excluir política |
| GET | `/api/admin/usage` | `admin:access` | Estatísticas globais de uso |
| GET | `/api/admin/settings` | `admin:access` | Configurações da aplicação |
| PUT | `/api/admin/settings` | `admin:access` | Atualizar configurações |
| POST | `/api/admin/providers/test` | `admin:access` | Testar conexão de provedor |
| POST | `/api/admin/providers/fetch-models` | `admin:access` | Buscar modelos do provedor |
| POST | `/api/admin/providers/test-inference` | `admin:access` | Testar inferência |

## Usuários (Admin)

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| GET | `/api/admin/users` | `users:manage` | Listar usuários |
| GET | `/api/admin/users/{id}` | `users:manage` | Obter usuário |
| POST | `/api/admin/users` | `users:create` | Criar usuário |
| PUT | `/api/admin/users/{id}` | `users:edit` | Atualizar usuário |
| DELETE | `/api/admin/users/{id}` | `users:delete` | Excluir usuário |

## Grupos

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| GET | `/api/groups` | `groups:view` | Listar grupos |
| GET | `/api/groups/{id}` | `groups:view` | Obter grupo |
| POST | `/api/groups` | `groups:create` | Criar grupo |
| PUT | `/api/groups/{id}` | `groups:edit` | Atualizar grupo |
| DELETE | `/api/groups/{id}` | `groups:delete` | Excluir grupo |
| POST | `/api/groups/{groupId}/members` | `groups:manage-members` | Adicionar membro |
| DELETE | `/api/groups/{groupId}/members/{userId}` | `groups:manage-members` | Remover membro |
| GET | `/api/groups/{groupId}/members` | `groups:view` | Listar membros |

## Papéis (Role Profiles)

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| GET | `/api/role-profiles` | `roles:manage` | Listar papéis |
| GET | `/api/role-profiles/{id}` | `roles:manage` | Obter papel |
| POST | `/api/role-profiles` | `roles:manage` | Criar papel |
| PUT | `/api/role-profiles/{id}` | `roles:manage` | Atualizar papel |
| DELETE | `/api/role-profiles/{id}` | `roles:manage` | Excluir papel |
| POST | `/api/role-profiles/assign` | `users:manage-roles` | Atribuir papel a usuário |
| DELETE | `/api/role-profiles/assign/{assignmentId}` | `users:manage-roles` | Remover atribuição |
| GET | `/api/role-profiles/user/{userId}/assignments` | `users:view` | Atribuições do usuário |
| GET | `/api/role-profiles/my-permissions` | `permissions:view` | Permissões do usuário atual |
| GET | `/api/role-profiles/my-check/{permissionKey}` | `permissions:view` | Verificar permissão específica |

## Permissões (Visibilidade)

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| GET | `/api/permissions/metadata` | `permissions:view` | Listar permissões disponíveis |
| GET | `/api/permissions/my-providers` | `providers:view` | Provedores visíveis ao usuário |
| GET | `/api/permissions/my-mcp-servers` | `mcp:access` | Servidores MCP visíveis |
| POST | `/api/permissions/grant-user` | `permissions:grant` | Conceder permissão a usuário |
| POST | `/api/permissions/grant-group` | `permissions:grant` | Conceder permissão a grupo |
| DELETE | `/api/permissions/user/{userId}/{providerId}` | `permissions:revoke` | Revogar permissão de usuário |
| DELETE | `/api/permissions/group/{groupId}/{providerId}` | `permissions:revoke` | Revogar permissão de grupo |
| GET | `/api/permissions/user/{userId}` | `permissions:view` | Permissões de um usuário |
| GET | `/api/permissions/group/{groupId}` | `permissions:view` | Permissões de um grupo |

## Visibilidade

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| GET | `/api/visibility/group/{groupId}/providers` | `permissions:view` | Visibilidade de provedores por grupo |
| PUT | `/api/visibility/providers/{providerId}/groups` | `permissions:grant` | Alternar visibilidade de provedor |
| GET | `/api/visibility/group/{groupId}/mcp-servers` | `mcp:servers:view` | Visibilidade de MCPs por grupo |
| PUT | `/api/visibility/mcp-servers/{serverId}/groups` | `mcp:servers:manage-visibility` | Alternar visibilidade de MCP |
| GET | `/api/visibility/group/{groupId}/projects` | `projects:view` | Visibilidade de projetos por grupo |
| PUT | `/api/visibility/projects/{projectId}/groups` | `projects:edit` | Alternar vínculo projeto-grupo |

## MCP Gateway

| Método | Rota | Autenticação | Descrição |
|---|---|---|---|
| POST | `/v1/mcp` | API Key | JSON-RPC MCP (streamable HTTP) |
| POST | `/mcp` | API Key | JSON-RPC MCP (atalho) |
| POST | `/v1/mcp/{slug}` | API Key | JSON-RPC para servidor específico |
| POST | `/mcp/{slug}` | API Key | JSON-RPC para servidor específico (atalho) |
| GET | `/v1/mcp/tools/list` | API Key | Listar ferramentas (REST) |
| GET | `/api/mcp/tools/visible` | API Key | Ferramentas visíveis |
| POST | `/v1/mcp/tools/call` | API Key | Executar ferramenta (API Key) |
| POST | `/api/mcp/tools/call` | JWT | Executar ferramenta (JWT) |
| GET | `/v1/mcp/servers` | API Key | Listar servidores |
| GET | `/api/mcp/tools` | Anônimo | Ferramentas públicas |
| GET | `/api/mcp/visible-servers` | JWT | Servidores visíveis |
| GET | `/api/mcp/discovery` | JWT | Descoberta completa de ferramentas |
| GET | `/v1/mcp/{slug}/tools/list` | API Key | Ferramentas de um servidor |
| POST | `/v1/mcp/{slug}/tools/call` | API Key | Executar ferramenta em servidor |
| GET | `/v1/mcp/{slug}/servers` | API Key | Info de servidor por slug |

## MCP Admin

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| GET | `/api/mcp/servers` | `mcp:servers:view` | Listar servidores MCP |
| POST | `/api/mcp/servers` | `mcp:servers:create` | Criar servidor MCP |
| PUT | `/api/mcp/servers/{id}` | `mcp:servers:edit` | Atualizar servidor MCP |
| DELETE | `/api/mcp/servers/{id}` | `mcp:servers:delete` | Excluir servidor MCP |
| GET | `/api/mcp/servers/{id}/visibility` | `mcp:servers:manage-visibility` | Visibilidade do servidor |
| PUT | `/api/mcp/servers/{id}/visibility` | `mcp:servers:manage-visibility` | Alterar visibilidade |
| POST | `/api/mcp/servers/sync` | `mcp:servers:create` | Sincronizar plugins |
| POST | `/api/mcp/servers/{id}/discover` | `mcp:servers:edit` | Descobrir ferramentas |
| GET | `/api/mcp/servers/{id}/tools` | `mcp:servers:view` | Listar ferramentas do servidor |
| POST | `/api/mcp/servers/{id}/import-openapi` | `mcp:servers:edit` | Importar OpenAPI |

## MCP Builder

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| POST | `/api/mcp/builder/parse` | `mcp:servers:create` | Parsear definições em ferramentas |
| POST | `/api/mcp/builder/preview` | `mcp:servers:create` | Preview do artefato MCP |
| POST | `/api/mcp/builder/deploy` | `mcp:servers:create` | Deploy do servidor MCP |
| POST | `/api/mcp/builder/import-config` | `mcp:servers:create` | Importar configuração JSON |

## OpenAPI Converter

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| POST | `/api/mcp/converter/discover` | `mcp:servers:create` | Descobrir spec OpenAPI de URL |
| POST | `/api/mcp/converter/parse` | `mcp:servers:create` | Parsear conteúdo OpenAPI |
| POST | `/api/mcp/converter/preview` | `mcp:servers:create` | Preview da conversão |
| POST | `/api/mcp/converter/deploy` | `mcp:servers:create` | Deploy como servidor MCP |
| GET | `/api/mcp/converter/projects` | `mcp:servers:view` | Listar projetos do conversor |
| GET | `/api/mcp/converter/projects/{id}` | `mcp:servers:view` | Obter projeto do conversor |
| PUT | `/api/mcp/converter/projects/{id}` | `mcp:servers:create` | Atualizar projeto |
| DELETE | `/api/mcp/converter/projects/{id}` | `mcp:servers:create` | Excluir projeto |
| POST | `/api/mcp/converter/projects/export` | `mcp:servers:create` | Exportar projeto como JSON |
| POST | `/api/mcp/converter/projects/import` | `mcp:servers:create` | Importar projeto JSON |
| GET | `/api/mcp/converter/projects/by-server/{serverId}` | `mcp:servers:view` | Projeto por server ID |

## Analytics

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| GET | `/api/analytics/overview` | `insights:view` | Estatísticas agregadas |
| GET | `/api/analytics/daily` | `insights:view` | Uso diário |
| GET | `/api/analytics/providers` | `insights:view` | Uso por provedor |
| GET | `/api/analytics/models` | `insights:view` | Uso por modelo |
| GET | `/api/analytics/matrix` | `insights:view` | Matriz provedor-modelo |
| GET | `/api/analytics/group/{groupId}` | `consumption:view:group` | Analytics por grupo |
| GET | `/api/analytics/own` | `consumption:view:own` | Próprio consumo |

## Consumo do Time

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| GET | `/api/team-consumption` | `consumption:view:own` | Consumo (próprio/time/todos) |
| GET | `/api/team-consumption/my-team` | `consumption:view:own` | Consumo do time |
| GET | `/api/team-consumption/my-groups` | `consumption:view:group` | Consumo dos grupos |
| GET | `/api/team-consumption/by-project/{projectId}` | `consumption:view:own` | Consumo por projeto |
| GET | `/api/team-consumption/by-provider/{providerName}` | `consumption:view:own` | Consumo por provedor |
| GET | `/api/team-consumption/groups-by-project/{projectId}` | `consumption:view:group` | Consumo de grupos por projeto |
| GET | `/api/team-consumption/groups-by-provider/{providerName}` | `consumption:view:group` | Consumo de grupos por provedor |
| GET | `/api/team-consumption/by-projects` | `consumption:view:own` | Consumo agrupado por projeto |
| GET | `/api/team-consumption/user/{targetUserId}` | `consumption:view:own` | Consumo de usuário específico |

## Segurança

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| GET | `/api/security/ip` | `security:view` | Regras de IP |
| PUT | `/api/security/ip` | `security:edit` | Criar/atualizar regras de IP |
| DELETE | `/api/security/ip/{id}` | `security:edit` | Excluir regra de IP |
| GET | `/api/security/geo` | `security:view` | Regras geográficas |
| PUT | `/api/security/geo` | `security:edit` | Criar/atualizar regras geográficas |
| DELETE | `/api/security/geo/{id}` | `security:edit` | Excluir regra geográfica |
| GET | `/api/security/status` | `security:view` | Status geral de segurança |

## Secret Vault

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| GET | `/api/secrets` | `secrets:view` | Listar segredos |
| GET | `/api/secrets/{id}` | `secrets:view` | Obter segredo |
| POST | `/api/secrets` | `secrets:create` | Criar segredo |
| PUT | `/api/secrets/{id}` | `secrets:edit` | Atualizar segredo |
| GET | `/api/secrets/env-health` | `secrets:view` | Health check de env vars |
| DELETE | `/api/secrets/{id}` | `secrets:delete` | Excluir segredo |

## Configurações

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| GET | `/api/settings` | `settings:view` | Obter configurações |
| PUT | `/api/settings` | `settings:edit` | Atualizar configurações |

## Auditoria

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| GET | `/api/admin/audit` | `audit:view` | Logs de auditoria |
| GET | `/api/admin/audit/{id}` | `audit:view` | Detalhe do log |
| GET | `/api/admin/audit/entities` | `audit:view` | Nomes de entidades auditadas |

## Uso Global

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| GET | `/api/usage` | `admin:access` | Estatísticas globais de uso |
| GET | `/api/logs` | `consumption:view:all` | Logs de requisição |

## Health Check

| Método | Rota | Autenticação | Descrição |
|---|---|---|---|
| GET | `/health` | Nenhuma | Health check dos provedores |
