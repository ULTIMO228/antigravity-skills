---
name: session-handoff
description: >
  Используй когда сессия затянулась, пользователь просит сохранить прогресс, или нужно
  продолжить работу в новом окне. Создаёт или обновляет .ai/SESSION_STATE.md по протоколу
  Five-Layer Handoff в формате DenseCode. Обеспечивает бесшовный переход между сессиями.
---

# session-handoff

## Write Protocol

Collect data (`git diff`, `git status`, test results) → write `.ai/SESSION_STATE.md` в DenseCode. 5 layers:

```
## Facts
Proj:<name> | Branch:<branch> | Mode:<mode> | Status:<status>
Files_changed:[...] | Files_created:[...] | Tests:{pass:N, fail:N}
Last_commit:<hash> "<msg>" | Updated:<ISO_datetime>

## Story (что сделано, хронология)
1. Impl:feature_X -> результат ✅/⚠️/❌
2. ...

## Reasoning (почему так решили — чтобы не пересматривать)
JWT>sessions: stateless+scaling | bcrypt>argon2: existing_db compat

## Action (что дальше, ! = приоритет)
!1: <следующий шаг>
!2: <важный шаг>
 3: <менее важный>

## Caution (ловушки для следующей сессии)
⚠️ Redis NOT configured -> don't test rate_limiting
❌ DO NOT modify migrations directly
🧠 auth.service.ts = gold_std
```

Verify: все решения в Reasoning, все ловушки в Caution. Notify user: «Сохранено. В новой сессии скажи "продолжи" — подхвачу.»

## Resume Protocol (чтение в новой сессии)

If `.ai/SESSION_STATE.md` exists at session start:
1. Read → parse all layers
2. Tell user: «Загрузил: [краткая сводка]. Продолжаю с [Action !1].»
3. File > 7 days old → warn: «Контекст может быть устаревшим»
