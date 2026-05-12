---
name: refactoring
description: >
  Safe refactoring: test-first, atomic steps, verify after each.
  Never changes behavior unless asked. Updates PROJECT_MAP on
  structure changes. Detects stack for correct lint/test commands.
---

# refactoring — Safe Refactoring Protocol

## Step 0: Context
Read `.ai/SKILL_CONFIG.md` → get lint + test commands.
Read `.ai/PROJECT_MAP.md` → understand dependencies.

## Procedure

### 1. Lock current behavior
Tests exist? → run, ensure green.
No tests? → write characterization tests first (capture current behavior).

### 2. Plan
Identify: what to refactor, why, expected result.
List atomic steps (each independently valid):
```
Step 1: Extract method X from class Y
Step 2: Rename parameter Z → W
Step 3: Move file A → B, update imports
```

### 3. Execute (one step at a time)
```
For each step:
  1. Make change
  2. Run lint: ruff check . | pnpm lint
  3. Run tests: pytest | pnpm test
  4. All green? → next step
  5. Red? → fix or revert step
```

### 4. Verify
- All tests pass
- No behavior change (unless intentional)
- Imports updated
- No dead code left

### 5. Update
Structure changed? → update `.ai/PROJECT_MAP.md` + `.foryou/PROJECT_MAP.md`

## Rules
- NEVER change behavior and structure simultaneously
- One type of refactoring per commit
- Keep commits small and reviewable

→ After: `code-review` → `git-commit`
