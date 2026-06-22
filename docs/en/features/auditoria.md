# Audit

Complete and immutable record of all create, update, and delete operations in the system, with before/after details.

![Audit Logs](../../assets/audit-logs.png)

## How It Works

Every database write operation is automatically intercepted by an **AuditInterceptor** (EF Core `SaveChanges` interceptor) that:

1. Captures the changed entity
2. Identifies the action type (Create, Update, Delete)
3. Records old and new values of modified fields
4. Associates the operation with the authenticated user
5. Saves the record to the `AuditLogs` table

## Viewing

![Audit Logs](../../assets/audit-logs.png)

The audit interface offers:

- **Filters:**
  - Entity (combobox with audited entity names)
  - Action (Create, Update, Delete)
  - User (ID of the user who performed the action)
  - Period (calendar picker)

- **Paginated table** with:
  - Affected entity
  - Action type
  - Timestamp
  - Responsible user
  - IP address

- **Detail sheet:**
  - Record key values
  - Changed fields with old and new values

## Endpoints

| Method | Route | Permission | Description |
|---|---|---|---|
| `GET` | `/api/admin/audit` | `audit:view` | List audit logs |
| `GET` | `/api/admin/audit/{id}` | `audit:view` | Audit detail |
| `GET` | `/api/admin/audit/entities` | `audit:view` | Audited entity names |

## Related Screens

- [Audit Logs](../screens.md#audit-logs-adminaudit)
