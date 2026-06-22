# Auditoria

Registro completo e imutável de todas as operações de criação, atualização e exclusão realizadas no sistema, com detalhes de antes e depois.

![Audit Logs](../assets/audit-logs.png)

## Como Funciona

Toda operação de escrita no banco de dados é interceptada automaticamente por um **AuditInterceptor** (EF Core `SaveChanges` interceptor) que:

1. Captura a entidade alterada
2. Identifica o tipo de ação (Create, Update, Delete)
3. Registra os valores antigos e novos dos campos modificados
4. Associa a operação ao usuário autenticado
5. Salva o registro na tabela `AuditLogs`

## Visualização

![Audit Logs](../assets/audit-logs.png)

A interface de auditoria oferece:

- **Filtros:**
  - Entidade (combobox com nomes das entidades auditadas)
  - Ação (Create, Update, Delete)
  - Usuário (ID do usuário que realizou a ação)
  - Período (seleção por calendário)

- **Tabela paginada** com:
  - Entidade afetada
  - Tipo de ação
  - Timestamp
  - Usuário responsável
  - Endereço IP

- **Detalhes em sheet lateral:**
  - Valores-chave do registro
  - Campos alterados com valores antigos e novos

## Endpoints

| Método | Rota | Permissão | Descrição |
|---|---|---|---|
| `GET` | `/api/admin/audit` | `audit:view` | Listar logs de auditoria |
| `GET` | `/api/admin/audit/{id}` | `audit:view` | Detalhe do log |
| `GET` | `/api/admin/audit/entities` | `audit:view` | Nomes das entidades auditadas |

## Telas Relacionadas

- [Auditoria](../screens.md#auditoria-adminaudit)
