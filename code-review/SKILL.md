---
name: code-review
description: >
  Structured code review: security, logic, edge cases, architecture
  compliance. Loads stack-specific checklist from references/.
  Scores 1-10, blocks merge if < 6. Call after feature completion.
---

# code-review — Multi-Stack Code Review

## Step 0: Context
Read `.ai/PROJECT_MAP.md` → understand architecture.
Read `.ai/SKILL_CONFIG.md` → load matching reference:
- Python → read `references/python-checklist.md`
- TS → read `references/ts-checklist.md`

## Procedure

### 1. Scope
Identify: changed files, affected feature, requirements from chat/plan.

### 2. Checklist (critical → quality)

| Severity | Checks |
|----------|--------|
| 🔴 Critical | SQL injection, XSS, IDOR, hardcoded secrets, empty catch, missing input validation, unprotected routes |
| 🟡 Important | Wrong logic, off-by-one, race conditions, unhandled edge cases (null/empty/timeout), N+1 queries, memory leaks, divergence from PROJECT_MAP patterns |
| 🟢 Quality | Unclear names, code duplication, `any`/`Any` types, missing tests, inconsistent style |

### 3. Stack-specific checks
Load reference file → apply additional checks per language.

### 4. Lessons check
Read `.ai/LESSONS.md` if exists → current code repeats past mistakes?

### 5. Output
```
## Результат ревью
**Оценка: X/10** | **Scope:** <what was reviewed>

### 🔴 Critical
- file:line — проблема → fix

### 🟡 Important
- file:line — проблема → рекомендация

### 🟢 Quality
- file:line — замечание → рекомендация

### ✅ Что хорошо

### Рекомендации
```

### 6. Decision
| Score | Action |
|-------|--------|
| < 6 | "Не готов к мерджу" — fix Critical items first |
| 6-7 | "Условно готов" — fix Important items |
| ≥ 8 | "Готов ✅" |

→ After: `git-commit` if approved
