# Smart Routing

Smart Router is a virtual provider that analyzes the user's message intent and routes the request to the most suitable provider/model, optimizing cost, speed, and quality.

## How It Works

```
Request with model = "smart-routing"
         │
         ├── Extracts last user message
         ├── Classifies intent by keywords
         ├── Evaluates configured rules (by priority)
         │    ├─ keyword → does any keyword match?
         │    ├─ semantic → does exact text match?
         │    ├─ model → does requested model match?
         │    └─ always → catch-all
         │
         ├── Determines target provider + model
         └── Forwards to target
```

## Intent Classification

The system classifies messages into categories using keyword matching:

| Intent | Keywords | Typical Use |
|---|---|---|
| `code` | function, class, async, sql, docker, debug, test | Code generation, SQL, debugging |
| `simple_qa` | what is, who is, define, explain, how many | Simple factual questions |
| `complex_reasoning` | analyze, evaluate, hypothesis, paradigm | Deep analysis, hypotheses |
| `creative_writing` | write a story, compose, imagine, narrative | Stories, narratives, metaphors |
| `analysis` | breakdown, review, investigate, statistics | Data review, statistics |
| `translation` | translate, english, portuguese, spanish | Language translation |

## Routing Rules

Each rule has:

| Property | Description |
|---|---|
| **Name** | Rule identifier |
| **Priority** | Lower number = higher priority |
| **Condition Type** | `keyword`, `semantic`, `model`, or `always` |
| **Condition Value** | Text/keywords for matching |
| **Target Provider** | Which provider to forward to |
| **Target Model** | Which model to use (optional) |

### Fallback

If no rule matches, the system uses:
1. Configured fallback provider
2. Configured fallback model
3. Active provider with lowest priority

## Testing Rules

Use `POST /api/providers/{id}/smart-routing/test` to test routing rules without making the actual call.

## Related Screens

- [Providers (Admin)](../screens.md#providers-adminproviders) — Smart Routing config in provider wizard
