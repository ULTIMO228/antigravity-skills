---
name: densecode
description: >
  Compression standard for .ai/ files. Apply before writing any file
  to .ai/ directory. Converts verbose text to token-efficient format.
  Trigger: any .ai/ write operation.
---

# densecode — Token-Efficient Compression Protocol

## When to apply
Before writing ANY file to `.ai/` directory. Other skills reference this:
`→ Before .ai/ write: apply densecode`

## Rules (in priority order)

1. **English only** — no Russian in .ai/ files
2. **Imperative verbs** — "Check → Run → Write → Return" not "You should check"
3. **No fluff** — remove: please, make sure, could you, it is important to
4. **Symbols over words**:

| Symbol | Meaning | Example |
|--------|---------|---------|
| `→` | then/leads to | `scan → detect → write` |
| `=>` | results in | `ruff check => 0 errors` |
| `+` | and | `pytest + coverage` |
| `\|` | or/alternative | `pyproject.toml \| package.json` |
| `>` | prefer | `asyncio > threading` |
| `?` | unknown/TBD | `deploy strategy?` |
| `!` | important/priority | `!1: fix auth` |
| `@` | context/scope | `@src/agents/` |
| `-` | without/exclude | `all - tests` |

5. **Abbreviations** (stable vocabulary):

| Short | Full | Short | Full |
|-------|------|-------|------|
| svc | service | fn | function |
| cfg | config | impl | implementation |
| req | request | res | response |
| db | database | usr | user |
| err | error | auth | authentication |
| dep | dependency | env | environment |
| msg | message | val | validation |
| gen | generate | init | initialize |
| mgr | manager | util | utility |

6. **Emoji flags** (semantic markers):

| Flag | Meaning |
|------|---------|
| ✅ | done/passing |
| ❌ | failed/blocked |
| ⚠️ | warning/caution |
| 🧠 | key insight/logic |
| 🔄 | iteration/retry |

7. **Structure**: use `##` headers, `key:value` pairs, pipe-separated fields
8. **Tables > paragraphs** for structured data
9. **One line = one fact** — no multi-sentence paragraphs

## Format templates

### Project state
```
## Meta
Proj:cortex | Stack:Python3.11+LangGraph | Linter:ruff | Tests:pytest
Branch:feat/risk-mgr | Mode:hard | Status:active

## Tree
/src → main code
  /agents → LLM agents (Watcher, Analyst, Debater)
  /market → MOEX ISS client + indicators
  /trading → Risk Manager + strategies
```

### Session entry
```
## 2026-05-12T18:30 | @vsevolod | branch:feat/knowledge-graph
Done: impl Knowledge Graph + NetworkX integration ✅
Decisions: NetworkX > Neo4j (lightweight, no infra) 🧠
Next: !1 backtest framework | !2 paper trading
Caution: ⚠️ no .env configured → skip API tests
```

### Flow notation
```
Flow:trade → signal:detect → risk:check → order:place => executed ✅
Flow:auth → usr:login → svc:verify → db:select => token | err:401
```

## Anti-patterns
- ❌ Russian text in .ai/ files
- ❌ Long paragraphs (> 2 lines)
- ❌ Redundant explanations of obvious things
- ❌ Full words when abbreviation exists in vocabulary
- ❌ Prose when table works better

## .foryou/ files (NOT compressed)
.foryou/ = for humans. Use:
- Russian language
- Full sentences
- Mermaid diagrams
- Beautiful markdown formatting
- Tables with headers
