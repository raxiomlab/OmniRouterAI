# Fluxos Funcionais

## 1. Proxy de Chat Completion

```
Cliente (curl/app)
    │
    ▼
[1] ApiKeyAuthMiddleware
    │   Extrai Bearer <prefixo.segredo>
    │   Busca por prefixo → compara HMAC → popula HttpContext
    │
    ▼
[2] SecurityPipelineMiddleware
    │   ├─ Denylist? IP bate? → 403 Forbidden
    │   ├─ GeoRules? País bloqueado? → 403 Forbidden
    │   ├─ Allowlist? IP não permitido? → 403 Forbidden
    │   └─ RateLimit? Janela excedida? → 429 Too Many Requests
    │
    ▼
[3] GuardRailsMiddleware (Request)
    │   Filtra conteúdo sensível (PII, credentials, etc.)
    │
    ▼
[4] ProxyService → Provedor destino
    │   Encaminha requisição via HttpClient
    │   Stream SSE ou response completa
    │
    ▼
[5] GuardRailsMiddleware (Response)
    │   Filtra resposta (se configurado)
    │   Conta tokens
    │   Grava RequestLog
    │
    ▼
Cliente ← 200 OK ou stream SSE
```

### Fluxo com Smart Routing

```
Proxy recebe requisição com model = "smart-routing"
    │
    ├─ Extrai última mensagem do usuário
    ├─ Classifica intenção (code, simple_qa, creative_writing, etc.)
    ├─ Avalia regras configuradas (por prioridade)
    │   ├─ keyword: alguma palavra-chave corresponde?
    │   ├─ semantic: texto exato corresponde?
    │   ├─ model: modelo solicitado corresponde?
    │   └─ always: catch-all (sempre corresponde)
    │
    ├─ Determina provedor + modelo destino
    │   ├─ Regra específica → target provider + model
    │   └─ Nenhuma regra → fallback provider + model
    │
    └─ Encaminha requisição ao destino
```

## 2. Ciclo de Vida da API Key

```
Usuário → "Nova API Key"
    │
    ▼
Seleciona:
    ├─ Projeto
    ├─ Nome da chave
    ├─ Prazo de expiração (7d/30d/90d/1yr/never)
    ├─ Tipo de acesso (Models / MCPs / Ambos)
    └─ Escopos granulares (por provedor/modelo)
    │
    ▼
Backend gera: gbk-a1b2c3.x9y8z7w6...
    │
    ├─ Prefixo: gbk-a1b2c3 → salvo em claro (busca rápida)
    ├─ Segredo: x9y8z7w6... → HMAC-SHA256(segredo, appSecret) = KeyHash
    └─ Salva: ApiKey { ProjectId, KeyHash, Prefix, Name, ExpiresAt, Scopes }
    │
    ▼
Frontend exibe segredo UMA ÚNICA VEZ:
    "COPIE AGORA. Você nunca mais verá este segredo."
    │
    ▼
Listagem (GET /api/api-keys):
    Apenas prefixo + nome + criado em + expira em + revogado em
    (NUNCA exibe o segredo)
```

### Validação da Chave no Middleware

```
Request recebido com "Authorization: Bearer gbk-a1b2c3.x9y8z7w6..."
    │
    ├─ Divide em prefixo (gbk-a1b2c3) e segredo (x9y8z7w6...)
    ├─ Busca no banco pelo prefixo
    ├─ Verifica se não está revogada nem expirada
    ├─ HMAC-SHA256(segredo, appSecret) → compara com KeyHash
    └─ Popula HttpContext.Items com ApiKeyId, ProjectId, UserId
```

## 3. Fluxo de Autenticação

### Login Local

```
Usuário → Preenche e-mail + senha
    │
    ▼
POST /api/auth/login
    │
    ├─ [Rate Limiter] Proteção contra brute force (429 após excesso)
    ├─ Valida credenciais
    ├─ Gera JWT próprio do gateway
    │   Claims: userId, email, role, permissions
    └─ Retorna { token, user } para o frontend
```

### Login Microsoft (OAuth)

```
Usuário → "Entrar com Microsoft"
    │
    ▼
GET /api/auth/microsoft → Redirect para login.microsoft.com
    │
    ▼
Usuário autentica na Microsoft
    │
    ▼
Microsoft redireciona para /api/auth/microsoft/callback
    │
    ├─ Recebe claims do OAuth (name, email, oid)
    ├─ Cria/atualiza User local se necessário
    ├─ Emite JWT próprio do gateway (mesmo formato do login local)
    ├─ Salva token em cookie oauth_token
    └─ Redireciona para /dashboard
```

### Uso do JWT

```
Frontend armazena JWT em memória (Zustand store)
    │
    ├─ Todas as requisições /api/* incluem: Authorization: Bearer <jwt>
    ├─ Refresh automático via POST /api/auth/refresh
    └─ Logout → limpa store + redireciona para /auth/login
```

## 4. Gerenciamento de Provedores

### Criação de Provedor

```
Admin → "Adicionar Provedor"
    │
    ▼
Wizard de criação:
    ├─ Tipo: OpenAI / Anthropic / Ollama / Azure OpenAI / Groq / Mistral /
    │        Fireworks / TogetherAI / DeepSeek / Perplexity / Smart Router
    ├─ Nome e descrição
    ├─ URL base da API
    ├─ Autenticação:
    │   ├─ API Key
    │   ├─ Bearer Token
    │   ├─ Google Service Account (JSON)
    │   └─ Google ADC (Application Default Credentials)
    ├─ Headers customizados (com suporte a Secret Vault)
    ├─ Prioridade (para balanceamento)
    ├─ Custo por 1k tokens (input/output)
    ├─ Atribuição a grupos (visibilidade)
    │
    ├─ [Opcional] Smart Routing:
    │   ├─ Provedor/modelo de fallback
    │   └─ Regras: prioridade, condição (keyword/semantic/model/always),
    │      target provider/model
    │
    ├─ [Opcional] Guard Rails:
    │   ├─ PII Detection (Request / Response / Both)
    │   ├─ Credentials Detection
    │   ├─ Financial Detection
    │   └─ Health Detection
    │
    └─ Teste de conexão → Fetch Models → Salvar
```

### Teste de Inferência

```
Admin → "Test Inference" no provedor
    │
    ▼
Diálogo com:
    ├─ Modelo: (dropdown com modelos disponíveis)
    ├─ Mensagem: (textarea)
    └─ Botão "Testar"
    │
    ▼
Requisição é enviada ao provedor real
Resposta é exibida no diálogo
```

## 5. Fluxo MCP (Model Context Protocol)

### Registro de Servidor MCP

```
Admin → "Novo Servidor MCP"
    │
    ▼
Configura:
    ├─ Tipo: HTTP / SSE / OpenAPI / Custom
    ├─ Nome, slug (identificador URL), descrição
    ├─ URL do servidor
    ├─ Headers customizados
    ├─ Visibilidade: público ou por grupo
    └─ Guard Rails ativos
    │
    ▼
Descobre ferramentas:
    ├─ Conecta ao servidor e lista ferramentas disponíveis
    ├─ Ou importa de spec OpenAPI
    └─ Salva ferramentas no banco
```

### Execução de Ferramenta MCP via Gateway

```
Cliente (API Key)
    │
    ▼
POST /v1/mcp  (JSON-RPC)
    │
    Corpo: { "jsonrpc": "2.0", "method": "tools/call",
             "params": { "name": "server-tool", "arguments": {...} } }
    │
    ▼
Gateway:
    ├─ Valida API Key
    ├─ Verifica permissão da ferramenta para a chave
    ├─ [Guard Rails] Filtra argumentos (request)
    ├─ Encaminha ao servidor MCP destino
    ├─ Recebe resultado
    ├─ [Guard Rails] Filtra resposta (response)
    └─ Retorna JSON-RPC response
```

### Rota para Servidor Específico

```
POST /v1/mcp/meu-servidor
    │
    ▼
Gateway roteia diretamente para o servidor com slug "meu-servidor"
    ├─ Verifica acesso da chave ao servidor
    └─ Forward JSON-RPC
```

## 6. Gerenciamento de Grupos e Visibilidade

### Controle de Visibilidade por Grupo

```
Admin → "Permissões" → Seleciona grupo
    │
    ▼
Para cada recurso (provedor / MCP server / projeto):
    ├─ Switch ON: recurso visível para membros do grupo
    └─ Switch OFF: recurso não visível
    │
    ▼
Usuários do grupo veem apenas os recursos ativados:
    ├─ GET /api/models → apenas modelos dos provedores visíveis
    ├─ GET /api/mcp → apenas servidores MCP visíveis
    └─ GET /api/projects → apenas projetos vinculados
```

## 7. Guard Rails (Pipeline de Filtragem)

```
Request body (antes do proxy)
    │
    ▼
GuardRailsMiddleware
    │
    ├─ Verifica se provedor/servidor tem guard rails configurados
    ├─ Para cada detector ativo (PII, Credentials, Financial, Health):
    │   ├─ Escaneia mensagens do usuário
    │   ├─ Escaneia tool calls
    │   └─ Substitui matches por [CATEGORIA-REDACTED]
    │
    ▼
Request sanitizado → enviado ao provedor
    │
    ▼
Response body (antes do cliente)
    │
    ├─ Mesmo processo no sentido inverso
    └─ Conteúdo sensível removido da resposta
```

## 8. Auditoria

```
Toda operação de escrita (Create/Update/Delete)
    │
    ▼
AuditInterceptor (EF Core SaveChanges interceptor)
    │
    ├─ Captura entidade alterada, ação, timestamp
    ├─ Registra valores antigos e novos
    ├─ Associa ao usuário autenticado (via ICurrentUserService)
    └─ Salva na tabela AuditLogs
    │
    ▼
Admin visualiza em /admin/audit
    ├─ Filtros por entidade, ação, usuário, período
    └─ Detalhes: campos alterados, valores anteriores/novos
```

## 9. Secret Vault

### Criação de Segredo

```
Admin → Secret Vault → Novo Segredo
    │
    ▼
Tipo: Token / API Key / Password / Header / Google SA / Google ADC
    │
    ▼
Nome + Valor (mascarado na UI)
    │
    ▼
Valor pode conter variáveis de ambiente:
    ├─ ${MINHA_VAR} → substituído pela env var no runtime
    └─ ${MINHA_VAR:-default} → usa "default" se env var não existir
    │
    ▼
Armazenado criptografado no banco
```

### Health Check

```
GET /api/secrets/env-health
    │
    ▼
Para cada segredo com variáveis de ambiente:
    ├─ Verifica se cada ${VAR} está definida no ambiente
    └─ Retorna lista de variáveis resolvidas e não resolvidas
```

## 10. Dashboard (Visão Geral)

```
Usuário logado → "/" ou "/dashboard"
    │
    ▼
Carrega em paralelo:
    ├─ StatsGrid: total de requisições, tokens, custo, provedores ativos
    ├─ UsageCharts: gráficos de consumo ao longo do tempo
    ├─ RecentActivity: últimas requisições em timeline
    ├─ ProviderHealth: status de cada provedor (online/offline)
    └─ Projetos recentes do usuário
    │
    ▼
Ações rápidas:
    ├─ Gerar API Key → /apikeys
    ├─ Convidar Usuário → /admin/users
    ├─ Adicionar MCP Server → /admin/mcp
    └─ Regras de Segurança → /admin/security
```
