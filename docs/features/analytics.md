# Analytics e Monitoramento

Dashboards completos para monitorar o consumo de IA por provedor, modelo, projeto, grupo e usuário, com métricas de requisições, tokens e custos.

## Dashboard Principal

![Dashboard](../assets/dashboard.png)

A página inicial exibe um resumo executivo com:

- **Ações Rápidas:** Gerar API Key, Convidar Usuário, Adicionar MCP Server, Regras de Segurança
- **Cards de Estatísticas:** Total de requisições, tokens (input/output), custo mensal, provedores ativos
- **Gráficos de Uso:** Requisições diárias, tokens input vs output, custo por provedor
- **Atividade Recente:** Timeline das últimas ações no sistema
- **Saúde dos Provedores:** Status de cada provedor (online/offline)
- **Projetos Recentes:** Últimos projetos acessados

## Insights Avançados

![Consumption](../assets/consumption.png)

Painel de analytics completo com filtros por período:

| Componente | Descrição |
|---|---|
| **PeriodFilter** | Seleção de período (7/30/90 dias ou customizado) |
| **StatsCards** | Métricas consolidadas do período |
| **DailyUsageChart** | Gráfico de requisições diárias |
| **ProviderPieChart** | Distribuição de custo por provedor |
| **ModelBarChart** | Top modelos por custo/requisições/tokens |
| **ProviderModelChart** | Matriz de uso provedor vs modelo |
| **UsageTable** | Tabela detalhada de uso |

## Consumo do Time

![Consumption](../assets/consumption.png)

Dashboard específico para gerentes visualizarem o consumo da equipe:

- **Filtros:** Por projeto, provedor e grupo
- **Cards:** Membros, requisições, tokens, custo
- **Gráficos:** Requisições diárias, input vs output, custo por provedor (donut), tokens (stack)
- **Rankings:** Top members, top cost, top projects/providers/groups

## Logs de Requisição

![Logs](../assets/logs.png)

Tabela detalhada de todas as requisições realizadas:

| Coluna | Descrição |
|---|---|
| Timestamp | Data e hora da requisição |
| Modelo | Modelo de IA utilizado |
| Provedor | Provedor que atendeu a requisição |
| Tokens Prompt | Tokens de entrada |
| Tokens Completion | Tokens de saída |
| Custo | Custo em USD |
| Status | Código HTTP de resposta |

## Endpoints

| Método | Rota | Descrição |
|---|---|---|
| `GET` | `/api/analytics/overview` | Estatísticas agregadas |
| `GET` | `/api/analytics/daily` | Uso diário |
| `GET` | `/api/analytics/providers` | Uso por provedor |
| `GET` | `/api/analytics/models` | Uso por modelo |
| `GET` | `/api/analytics/own` | Próprio consumo |
| `GET` | `/api/team-consumption` | Consumo do time |
| `GET` | `/api/logs` | Logs de requisição |

## Telas Relacionadas

- [Dashboard](../screens.md#dashboard-)
- [Consumo do Time](../screens.md#consumo-do-time-adminconsumption)
- [Logs de Requisição](../screens.md#logs-de-requisição-logs)
