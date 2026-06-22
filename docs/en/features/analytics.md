# Analytics and Monitoring

Complete dashboards for monitoring AI consumption by provider, model, project, group, and user, with request, token, and cost metrics.

## Dashboard

![Dashboard](../../assets/dashboard.png)

The main dashboard displays an executive summary with:

- **Quick Actions:** Generate API Key, Invite User, Add MCP Server, Security Rules
- **Stats Cards:** Total requests, tokens (input/output), monthly cost, active providers
- **Usage Charts:** Daily requests, input vs output tokens, cost by provider
- **Recent Activity:** Timeline of latest system actions
- **Provider Health:** Each provider's status (online/offline)
- **Recent Projects:** Last accessed projects

## Advanced Insights

![Consumption](../../assets/consumption.png)

Full analytics dashboard with period filters:

| Component | Description |
|---|---|
| **PeriodFilter** | Period selection (7/30/90 days or custom) |
| **StatsCards** | Consolidated period metrics |
| **DailyUsageChart** | Daily request chart |
| **ProviderPieChart** | Cost distribution by provider |
| **ModelBarChart** | Top models by cost/requests/tokens |
| **ProviderModelChart** | Provider vs model usage matrix |
| **UsageTable** | Detailed usage table |

## Team Consumption

![Consumption](../../assets/consumption.png)

Manager-specific dashboard for team usage:

- **Filters:** By project, provider, and group
- **Cards:** Members, requests, tokens, cost
- **Charts:** Daily requests, input vs output, cost by provider (donut), tokens (stack)
- **Rankings:** Top members, top cost, top projects/providers/groups

## Request Logs

![Logs](../../assets/logs.png)

Detailed table of all requests:

| Column | Description |
|---|---|
| Timestamp | Request date and time |
| Model | AI model used |
| Provider | Provider that served the request |
| Prompt Tokens | Input tokens |
| Completion Tokens | Output tokens |
| Cost | Cost in USD |
| Status | HTTP response code |

## Endpoints

| Method | Route | Description |
|---|---|---|
| `GET` | `/api/analytics/overview` | Aggregate stats |
| `GET` | `/api/analytics/daily` | Daily usage |
| `GET` | `/api/analytics/providers` | Usage by provider |
| `GET` | `/api/analytics/models` | Usage by model |
| `GET` | `/api/analytics/own` | Own consumption |
| `GET` | `/api/team-consumption` | Team consumption |
| `GET` | `/api/logs` | Request logs |

## Related Screens

- [Dashboard](../screens.md#dashboard-)
- [Team Consumption](../screens.md#team-consumption-adminconsumption)
- [Request Logs](../screens.md#request-logs-logs)
