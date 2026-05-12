---
name: gemini-dispatch
description: >
  Protocol for invoking Gemini CLI subagents from orchestrator (Antigravity).
  Use ONLY for junior/mid-level tasks. Complex architecture, algorithms,
  and multi-file refactoring — Antigravity does directly. Never delegate senior work.
---

# gemini-dispatch — Subagent Invocation Protocol

## ⚠️ CRITICAL: Task Complexity Gate

**Before dispatching ANY task — classify its complexity.**
Gemini subagents = pre-middle level max. They write decent code for well-defined tasks, but lose context, drop constraints, and produce shallow results on anything complex.

### Complexity classification

| Level | Delegate? | Examples |
|-------|-----------|---------|
| 🟢 **Intern** (Flash Lite) | ✅ YES | Write .dockerignore, update .gitignore, simple configs |
| 🟡 **Junior** (Flash) | ✅ YES | Documentation, simple data transforms, boilerplate |
| 🔵 **Pre-Middle** (Pro) | ✅ YES | Unit tests for one module, simple CRUD, add logging, write fixtures, single-file utilities |
| 🟠 **Middle** | ❌ NO — do yourself | Multi-file features, refactoring with side effects, integration logic, state management |
| 🔴 **Senior** | ❌ NO — do yourself | Architecture, complex algorithms, multi-constraint design, risk manager, graph orchestration, security-critical code |

### Decision rule
```
Ask yourself: "Can a pre-middle developer do this with a clear spec and no follow-up questions?"
  YES → dispatch to subagent
  NO  → do it yourself (Antigravity/Claude)
```

### NEVER delegate to subagents
- Code that must follow 3+ interrelated constraints simultaneously
- Anything touching risk management or security-critical paths
- Multi-file refactoring or architecture changes
- Tasks requiring deep project context or prior conversation knowledge
- LangGraph graph design and agent orchestration
- Code review of complex logic (reviewer agent can only catch surface issues)

## Invocation format

```bash
gemini -p "@<agent_name> <task_prompt>" --yolo --cwd <project_path>
```

Flags:
- `-p` — non-interactive mode (headless), exits after completion
- `--yolo` — auto-approve all tool calls (no manual confirmation)
- `--cwd` — set working directory to project root

## Model routing (auto via agent YAML `model:` field)

| Tier | Model | Level | Good for | Agents |
|------|-------|-------|----------|--------|
| 🔴 Pro | `gemini-3.1-pro-preview` | Pre-middle | Tests, single-file code, data processing, analysis, schemas | python-dev, quant, tester, data-analyst, prompt-smith, researcher |
| 🟡 Flash | `gemini-3-flash-preview` | Junior | Documentation, simple reports | documenter |
| 🟢 Lite | `gemini-3.1-flash-lite-preview` | Intern | Configs, simple edits | devops |

**Removed from dispatch:** `langgraph-architect`, `reviewer` — their tasks are too complex for subagent quality. Antigravity handles these directly.

## Prompt template

Structure: **WHAT** + **CONTEXT** + **OUTPUT** + **VERIFY**

```
@<agent> <WHAT: clear, narrow task description>.
<CONTEXT: relevant files, exact constraints, architecture notes>.
<OUTPUT: where to save, format expectations>.
<VERIFY: what to run after (tests, lint, checks)>.
```

**Key:** The more specific the prompt, the better the result. Vague = garbage.

### Good dispatch examples (appropriate complexity)

```bash
# ✅ Tester: write tests for ONE module (clear scope)
gemini -p "@tester Write pytest tests for src/trading/risk_manager.py. Cover: happy path (valid trade within limits), edge cases (zero amount, negative price, exactly-at-limit), error cases (exceed daily limit, invalid symbol). Use AsyncMock for MOEX client. Save to tests/test_risk_manager.py. Run pytest --tb=short after." --yolo

# ✅ Documenter: update docs (simple task)
gemini -p "@documenter Update .foryou/PROJECT_MAP.md to include new src/news/ module. Add file tree with descriptions. Use Russian and Mermaid diagrams." --yolo

# ✅ Data-analyst: data pipeline (single-file, well-defined)
gemini -p "@data-analyst Write src/market/parser.py — parse MOEX ISS candles JSON response into list[Candle] Pydantic models. Handle empty data, missing fields, invalid dates. Add type hints. Run ruff check after." --yolo

# ✅ Parallel: tests + docs (both simple, independent)
gemini -p "@tester Write tests for src/market/parser.py" --yolo &
gemini -p "@documenter Document src/market/parser.py in .foryou/" --yolo &
```

### BAD dispatch examples (too complex — do yourself)

```bash
# ❌ Multi-file architecture — DO YOURSELF
gemini -p "@langgraph-architect Design the full agent graph..." --yolo

# ❌ Complex refactoring — DO YOURSELF
gemini -p "@python-dev Refactor all agents to use new state schema..." --yolo

# ❌ Security-critical code — DO YOURSELF
gemini -p "@python-dev Implement auth middleware with JWT..." --yolo
```

## Execution from Antigravity

```
1. Classify task complexity (see gate above)
2. If ≤ pre-middle → pick agent, build prompt
3. If > pre-middle → DO IT YOURSELF, skip dispatch
4. Execute via run_command (WaitMsBeforeAsync: 500 → background)
5. For parallel tasks: launch multiple run_command simultaneously
6. Monitor via command_status (WaitDurationSeconds: 300)
7. ALWAYS review subagent output — they make mistakes
8. Fix issues yourself if quality is insufficient
```

## Self-QA cycle (embedded in each agent)

Every agent follows this cycle after completing primary task:

```
1. EXECUTE → write code/analysis
2. SELF-REVIEW → check quality, edge cases, security
3. TEST → run relevant tests (pytest/lint)
4. FIX → address issues found
5. RE-TEST → verify fixes, check coverage
6. COMPATIBILITY → check imports, deps, existing code integration
7. REPORT → structured output with findings
```

## Rules

| Rule | Why |
|------|-----|
| Always classify complexity first | Prevents quality disasters |
| Always `--yolo` for automated calls | No manual confirmation needed |
| Always `--cwd <project_root>` | Agent needs correct working directory |
| Full context in prompt | Subagent has clean context, knows nothing |
| Never pass API keys in prompt | Security: keys stay in .env |
| One narrow task per call | Keep scope focused |
| ALWAYS review output | Subagents produce pre-middle quality at best |

## Error handling

```
Agent failed? →
  1. Read stderr from command_status
  2. If timeout → re-run with simpler scope
  3. If tool error → check agent YAML tools list
  4. If low quality output → don't retry, do it yourself
  5. 2+ failures → do it yourself (don't waste time)
```
