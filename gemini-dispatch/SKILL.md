---
name: gemini-dispatch
description: >
  Protocol for invoking Gemini CLI subagents from orchestrator (Antigravity).
  Use when task should be delegated to a specialized agent.
  Handles: command building, model routing, parallel execution, result collection.
---

# gemini-dispatch — Subagent Invocation Protocol

## Invocation format

```bash
gemini -p "@<agent_name> <task_prompt>" --yolo --cwd <project_path>
```

Flags:
- `-p` — non-interactive mode (headless), exits after completion
- `--yolo` — auto-approve all tool calls (no manual confirmation)
- `--cwd` — set working directory to project root

## Model routing (auto via agent YAML `model:` field)

| Tier | Model | Role | Agents |
|------|-------|------|--------|
| 🔴 Pro | `gemini-3.1-pro-preview` | Senior: architecture, review, math, research | python-dev, quant, langgraph-architect, reviewer, researcher, tester, data-analyst, prompt-smith |
| 🟡 Flash | `gemini-3-flash-preview` | Junior: docs, simple analysis | documenter |
| 🟢 Lite | `gemini-3.1-flash-lite-preview` | Intern: configs, simple edits | devops |

No need to specify model in prompt — agent YAML `model:` field handles routing.

## Prompt template

Structure: **WHAT** + **CONTEXT** + **OUTPUT** + **VERIFY**

```
@<agent> <WHAT: clear task description>.
<CONTEXT: relevant files, constraints, architecture notes>.
<OUTPUT: where to save, format expectations>.
<VERIFY: what to run after (tests, lint, checks)>.
```

### Examples

```bash
# Tester
gemini -p "@tester Write pytest tests for src/trading/risk_manager.py. Cover: happy path (valid trade within limits), edge cases (zero amount, negative price, exactly-at-limit), error cases (exceed daily limit, invalid symbol). Use AsyncMock for MOEX client. Save to tests/test_risk_manager.py. Run pytest --tb=short after." --yolo

# Reviewer (read-only)
gemini -p "@reviewer Review src/agents/debater.py. Check: logic correctness, error handling, type safety, Pydantic schema usage, async patterns. Report format: 🔴 Critical / 🟡 Important / 🟢 Minor / ✅ Good. Score 1-10." --yolo

# Parallel: review + test simultaneously
# (launch as separate run_command calls)
gemini -p "@reviewer Review src/market/indicators.py" --yolo &
gemini -p "@tester Write tests for src/market/indicators.py" --yolo &
```

## Execution from Antigravity

```
1. Decide: does task need subagent? → which one (check model routing table)
2. Build command with template above
3. Execute via run_command (WaitMsBeforeAsync: 500 → goes to background)
4. For parallel tasks: launch multiple run_command simultaneously
5. Monitor via command_status (WaitDurationSeconds: 300)
6. Parse stdout → synthesize results → report to user
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
| Always `--yolo` for automated calls | No manual confirmation needed |
| Always `--cwd <project_root>` | Agent needs correct working directory |
| Full context in prompt | Subagent has clean context, knows nothing |
| Never pass API keys in prompt | Security: keys stay in .env |
| One task per call | Keep scope focused |
| Check exit code | 0 = success, non-zero = investigate |

## Error handling

```
Agent failed? →
  1. Read stderr from command_status
  2. If timeout → re-run with simpler scope
  3. If tool error → check agent YAML tools list
  4. If logic error → add more context to prompt, retry
  5. 2+ failures → escalate to user (error-recovery skill)
```

## Limitations
- Subagent has NO access to current Antigravity conversation context
- Subagent cannot call other subagents (no nesting)
- Max runtime depends on model limits and max_turns setting
