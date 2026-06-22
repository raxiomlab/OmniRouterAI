# Guard Rails (Filtros de Conteúdo)

Sistema de detecção e remoção de informações sensíveis no tráfego de requisições e respostas dos provedores de IA, evitando vazamento de dados.

## Detectores Implementados

### PII (Dados Pessoais)

Protege informações pessoais de usuários finais:

| Padrão | Categoria | Exemplo |
|---|---|---|
| CPF (11 dígitos) | `CPF` | `123.456.789-00` |
| CNPJ (14 dígitos) | `CNPJ` | `12.345.678/0001-90` |
| E-mail | `EMAIL` | `usuario@exemplo.com` |
| Telefone | `TELEFONE` | `+55 (11) 99999-8888` |
| CEP | `CEP` | `01234-567` |

### Credenciais

Impede o envio de senhas e tokens para provedores externos:

| Padrão | Categoria | Exemplo |
|---|---|---|
| Senhas declaradas | `PASSWORD` | `password=minha-senha` |
| API Keys | `API_KEY` | `apikey=sk-...` |
| Tokens | `TOKEN` | `Bearer ghp_...` |
| Secrets | `SECRET` | `client_secret=...` |

### Financeiro

Protege dados financeiros e bancários:

| Padrão | Categoria | Exemplo |
|---|---|---|
| Cartão de crédito | `CARTAO_CREDITO` | `4111 1111 1111 1111` |
| Chave PIX | `PIX_KEY` | `chave pix=...` |
| Conta bancária | `CONTA_BANCARIA` | `agência=1234 conta=56789` |

### Saúde

Detecta informações médicas protegidas:

| Padrão | Categoria | Exemplo |
|---|---|---|
| Prontuário médico | `PRONTUARIO` | `prontuário=12345` |
| Diagnóstico | `DIAGNOSTICO` | `diagnóstico de diabetes` |
| Código CID | `CID` | `CID-10 E11` |

## Modos de Operação

Cada detector pode ser configurado por provedor ou servidor MCP em um dos modos:

| Modo | Descrição |
|---|---|
| `Request` | Filtra apenas o conteúdo enviado pela aplicação ao provedor |
| `Response` | Filtra apenas o conteúdo retornado pelo provedor |
| `Both` | Filtra em ambos os sentidos |

## Pipeline de Filtragem

```
Request Body (antes do proxy)
    │
    ├── PII Detection → [CPF-REDACTED]
    ├── Credentials Detection → [PASSWORD-REDACTED]
    ├── Financial Detection → [CARTAO_CREDITO-REDACTED]
    └── Health Detection → [PRONTUARIO-REDACTED]
    │
    ▼
Request Sanitizado → Provedor de IA
    │
    ▼
Response Body (antes do cliente)
    │
    └── Mesmo processo no sentido inverso
```

## Configuração por Provedor

A ativação dos guard rails é feita no cadastro de cada provedor ou servidor MCP, através do componente `GuardRailSelector` na interface.

## Telas Relacionadas

- [Provedores (Admin)](../screens.md#provedores-adminproviders) — Configuração de guard rails por provedor
- [Servidores MCP (Admin)](../screens.md#servidores-mcp-admin-adminmcp) — Configuração de guard rails por servidor MCP
