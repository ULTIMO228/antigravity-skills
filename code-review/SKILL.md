---
name: code-review
description: >
  Используй когда завершена фича, крупный блок кода, или пользователь явно просит ревью.
  Проверяет код по чеклисту: логика, edge-cases, безопасность, производительность,
  соответствие архитектуре. Опирается на .ai/PROJECT_MAP.md для понимания контекста.
---

# code-review

## Procedure

1. Read `.ai/PROJECT_MAP.md` if exists. Identify scope: changed files, affected feature, requirements from chat/plan.
2. Checklist (critical → quality):
   - 🔴 **Critical**: SQL injection, XSS, IDOR, hardcoded secrets, empty catch, missing input validation, unprotected routes
   - 🟡 **Important**: wrong logic, off-by-one, race conditions, unhandled edge cases (null/empty/timeout), N+1 queries, memory leaks, divergence from PROJECT_MAP patterns, plan non-compliance (mode:hard)
   - 🟢 **Quality**: unclear names, code duplication, `any` types, missing tests
3. Read `.ai/LESSONS.md` if exists — check if current code repeats past mistakes
4. Output:
```
## Результат ревью
**Оценка: X/10** | **Scope:** <что проверялось>
### 🔴 Critical
- file:line — проблема → fix
### 🟡 High
- file:line — проблема → рекомендация
### 🟢 Medium
- file:line — замечание → рекомендация
### ✅ Что хорошо
### Рекомендации
```
5. Score < 6 → «не готов к мерджу». Critical items → исправить сразу. Score ≥ 8 → «готов ✅»

→ After: `git-commit` if approved
