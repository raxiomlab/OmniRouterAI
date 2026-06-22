# Segurança

O OmniRouterAI implementa múltiplas camadas de segurança para proteger o acesso aos provedores de IA, incluindo controle por IP, geolocalização e limitação de taxa.

## Regras de IP

![Security Rules](../assets/security-rules.png)

Controle de acesso baseado em endereço IP com suporte a CIDR:

| Tipo | Descrição |
|---|---|
| **Allowlist** | Apenas IPs listados têm permissão de acesso |
| **Denylist** | IPs listados são bloqueados |

### Ordem de Avaliação

1. Denylist é verificada primeiro — se o IP está na lista de negação, o acesso é bloqueado
2. GeoRules são verificadas — se o país está bloqueado, o acesso é negado
3. Allowlist é verificada — se não está na lista de permissão e a lista não é vazia, o acesso é bloqueado

## Regras Geográficas (GeoIP)

![Security Rules](../assets/security-rules.png)

Bloqueio ou permissão de acesso baseado no país de origem:

| Configuração | Descrição |
|---|---|
| **País Bloqueado** | Requisições deste país são rejeitadas |
| **País Permitido** | Apenas requisições deste país são aceitas |

O sistema utiliza a base de dados **MaxMind GeoLite2** para resolução geográfica, com atualização automática mensal via job em background.

## Rate Limiting

| Escopo | Descrição |
|---|---|
| **Global** | Limite aplicado a todas as requisições do sistema |
| **Por Projeto** | Limite específico para um projeto |
| **Por API Key** | Limite específico para uma chave de API |

### Métricas

| Métrica | Descrição |
|---|---|
| **RPS** | Requisições por segundo |
| **RPM** | Requisições por minuto |
| **TPM** | Tokens por minuto |
| **Burst** | Rajada máxima permitida |

### Login Rate Limiting

Proteção específica contra força bruta no endpoint de login:

- Limite de tentativas por minuto por IP
- Resposta `429 Too Many Requests` quando excedido
- Desabilitado em ambientes de desenvolvimento e teste

## Secret Vault

![Secret Vault](../assets/secret-vault.png)

Armazenamento criptografado de segredos utilizados pelos provedores:

- Tipos suportados: token, API key, password, header, Google Service Account JSON, Google ADC
- Valores mascarados na interface
- Resolução de variáveis de ambiente (`${VAR_NAME}` ou `${VAR_NAME:-default}`)
- Health check de variáveis não resolvidas

## Pipeline de Segurança

```
Request
    │
    ├── [1] ApiKeyAuthMiddleware → Valida chave de API
    ├── [2] SecurityPipeline → IP → Geo → Rate Limit
    ├── [3] GuardRailsMiddleware → Filtro de conteúdo
    │
    ▼
Provedor de IA
```

## Telas Relacionadas

- [Regras de Segurança](../screens.md#regras-de-segurança-adminsecurity)
- [Secret Vault](../screens.md#secret-vault-adminsecrets)
