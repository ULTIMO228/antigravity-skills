---
name: session-handoff
description: >
  Saves session state for seamless continuation. Writes append-only
  .ai/SESSIONS.md log + quick .ai/SESSION_STATE.md snapshot.
  Trigger: long session, user says "save progress", switching context.
---

# session-handoff — Session State Management

## Write Protocol

Collect: `git diff`, `git status`, test results, key decisions.

### 1. Append to `.ai/SESSIONS.md` (append-only log)

Apply `densecode` skill. Each entry = one session block:

```markdown
---
## <ISO_datetime> | @<author_name> | branch:<branch> | mode:<mode>
Focus: <what was worked on>
Done: <completed items> ✅
  - item1 => result
  - item2 => result
Decisions: <key choices made> 🧠
  - choice_A > choice_B: <reason>
Next: <prioritized action items>
  !1: <highest priority>
  !2: <important>
   3: <nice to have>
Caution: <traps for next session>
  ⚠️ <warning>
  ❌ <do not do>
---
```

NEVER overwrite existing entries. Always append below last `---`.

### 2. Write `.ai/SESSION_STATE.md` (quick snapshot)

Overwrite with current state. Five layers:

```markdown
## Facts
Proj:<name> | Branch:<branch> | Mode:<mode> | Status:<status>
Files_changed:[...] | Files_created:[...] | Tests:{pass:N, fail:N}
Last_commit:<hash> "<msg>" | Updated:<ISO_datetime>

## Story
1. <what happened, chronological>
2. ...

## Reasoning
<key_decision>: <why> | alternative: <rejected_option>

## Action
!1: <next step>
!2: <important step>
 3: <less important>

## Caution
⚠️ <trap or warning>
❌ <do not do>
🧠 <key insight to remember>
```

### 3. Notify user
"Сохранено. В новой сессии скажи «продолжи» — подхвачу."

## Resume Protocol

If `.ai/SESSION_STATE.md` exists at session start:

```
1. Read SESSION_STATE.md → parse all 5 layers
2. Read last 3 entries from SESSIONS.md → context
3. Tell user: "Загрузил: [краткая сводка]. Продолжаю с [Action !1]."
4. File > 7 days old → warn: "Контекст может быть устаревшим"
5. Check git status → any uncommitted changes?
```

## Team support

In SESSIONS.md, each person writes their own section with `@name` tag.
Multiple people can append without conflicts (append-only).

```markdown
## 2026-05-12T18:30 | @vsevolod | branch:vsevolod/feat/risk-mgr
Done: impl risk_manager.py ✅
Next: !1 tests

## 2026-05-12T19:00 | @gleb | branch:gleb/feat/indicators
Done: impl Hurst Exponent ✅
Next: !1 Kelly Criterion
```

→ After: `git-commit` if needed
