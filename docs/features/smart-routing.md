# Smart Routing

O Smart Router é um provedor virtual que analisa a intenção da mensagem do usuário e roteia a requisição para o provedor/modelo mais adequado, otimizando custo, velocidade e qualidade.

## Como Funciona

```
Requisição com model = "smart-routing"
         │
         ├── Extrai última mensagem do usuário
         ├── Classifica intenção por palavras-chave
         ├── Avalia regras configuradas (por prioridade)
         │    ├── keyword → alguma palavra-chave corresponde?
         │    ├── semantic → texto exato corresponde?
         │    ├── model → modelo solicitado corresponde?
         │    └── always → catch-all
         │
         ├── Determina provedor + modelo destino
         └── Encaminha ao destino
```

## Classificação de Intenção

O sistema classifica a mensagem em categorias usando correspondência de palavras-chave:

| Intenção | Palavras-chave | Uso Típico |
|---|---|---|
| `code` | function, class, async, sql, docker, debug, test | Geração de código, SQL, depuração |
| `simple_qa` | what is, who is, define, explain, how many | Perguntas factuais simples |
| `complex_reasoning` | analyze, evaluate, hypothesis, paradigm | Análises profundas, hipóteses |
| `creative_writing` | write a story, compose, imagine, narrative | Histórias, narrativas, metáforas |
| `analysis` | breakdown, review, investigate, statistics | Revisão de dados, estatísticas |
| `translation` | translate, english, portuguese, spanish | Tradução entre idiomas |

## Regras de Roteamento

Cada regra possui:

| Propriedade | Descrição |
|---|---|
| **Nome** | Identificador da regra |
| **Prioridade** | Número menor = maior prioridade |
| **Tipo de Condição** | `keyword`, `semantic`, `model` ou `always` |
| **Valor da Condição** | Texto/palavras-chave para correspondência |
| **Provedor Destino** | Para qual provedor encaminhar |
| **Modelo Destino** | Qual modelo usar (opcional) |

### Fallback

Se nenhuma regra corresponder, o sistema usa:
1. Provedor de fallback configurado
2. Modelo de fallback configurado
3. Provedor ativo com menor prioridade

## Teste de Regras

![Providers](../assets/providers.png)

O endpoint `POST /api/providers/{id}/smart-routing/test` permite testar regras de roteamento sem executar a chamada real, informando qual regra seria acionada para um determinado texto.

## Telas Relacionadas

- [Provedores (Admin)](../screens.md#provedores-adminproviders) — Configuração de Smart Routing no wizard de provedores
