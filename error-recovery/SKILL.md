---
name: error-recovery
description: >
  Используй когда агент зациклился (Doom Loop): 2 или более неудачных попыток исправить
  одну и ту же ошибку. Принудительная остановка, откат к рабочему состоянию, анализ root cause
  с нуля, предложение альтернативных подходов. Предотвращает бесконечные циклы отладки.
---

# error-recovery

## Red Flags (auto-detect)
- Same diff applied & reverted 2+ times
- Error changes message but won't go away after 3 attempts
- Each fix creates a new error (cascade)
- Agent modifies code unrelated to original error

## Recursion Limit
**MAX 3 invocations per error.** After 3rd → escalate to `deep-context` or human.

## Procedure

1. **STOP** 🛑 No more quick fixes.
2. **Rollback**: find last working state → `git stash` (uncommitted) or `git reset --soft HEAD~N` (ask user for N) → verify code works
3. **Root cause from scratch** (forget previous hypotheses):
   - What exactly broke? (full error text)
   - Where? (file, line, stack)
   - When? (after which change)
   - What changed? (`git diff` from working state)
   - Why did previous fixes fail?
4. **Propose 2-3 fundamentally different approaches**:
```
### Вариант A: <name>
Подход: ... | Плюсы: ... | Минусы: ... | Усилие: 🟢/🟡/🔴 | Уверенность: X%
### Вариант B: <name>
...
### Вариант C: Workaround
Подход: обходной путь | Плюсы: быстро | Минусы: не root cause | 🟢 | X%
```
5. Ask user which variant. Implement step-by-step with tests.
6. Variant failed? → back to step 4 (respect recursion limit)
7. **After fix**: record in `.ai/LESSONS.md` — что сломалось, почему, как избежать

→ Escalation: `deep-context` if no variant works
