---
name: refactoring
description: >
  Используй когда пользователь просит отрефакторить код. Обеспечивает безопасный рефакторинг:
  сначала фиксация текущего поведения тестами, затем маленькие шаги с проверкой после каждого.
  Никогда не меняет поведение если не просили. Обновляет PROJECT_MAP при изменении структуры.
---

# refactoring

## Core Rule
> Рефакторинг = изменение структуры БЕЗ изменения поведения. Нужно поменять поведение → это отдельная задача.

## Procedure

1. **Lock behavior**: tests exist? → run, record count. No tests → invoke `testing`. User refuses tests → snapshot: capture current outputs for key inputs.
2. **Plan**: identify target (duplication? complexity? naming? structure?) → break into atomic steps → order: less risky first
3. **Execute each step**:
   - ONE change → run tests → pass? → commit `refactor(<scope>): <msg>` → next
   - Fail? → revert, analyze
4. **Common patterns**:
   | Проблема | Действие |
   |----------|----------|
   | Дублирование | Extract shared fn/component |
   | Длинная функция (>30 lines) | Split into named sub-functions |
   | Сложные условия | Extract to named predicates |
   | Magic numbers | Named constants |
   | God-object | Split by SRP |
   | Deep nesting | Early return / guard clauses |
   | Mixed concerns | Separate layers (handler→service→repo) |
5. **STOP criteria** — рекомендуй переписать с нуля если:
   - >5 intertwined responsibilities
   - Can't write tests without mocking everything
   - Every change breaks something unrelated
6. **Final**: run ALL project tests. Count BEFORE == count AFTER. Update PROJECT_MAP if structure changed.

→ After: `code-review` → `git-commit`
