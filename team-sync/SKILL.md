---
name: team-sync
description: >
  Team collaboration protocol for 4-person team. Branch naming,
  zone ownership, merge protocol, session logging. Prevents conflicts
  when multiple developers work on same project with AI tools.
---

# team-sync — 4-Person Team Collaboration

## Team setup
| Member | Tool | Zones |
|--------|------|-------|
| Defined in `.ai/TEAM_ZONES.md` per project | Antigravity / Claude Code | Assigned modules |

## Branch naming
```
<name>/<type>/<description>
```
Examples:
- `vsevolod/feat/risk-manager`
- `gleb/fix/indicators-calculation`
- `erik/refactor/news-pipeline`
- `member4/test/trading-coverage`

Types: `feat`, `fix`, `refactor`, `test`, `docs`, `chore`

## Zone ownership

Create `.ai/TEAM_ZONES.md` at project start:

```markdown
# TEAM_ZONES
## Assignments
vsevolod: src/agents/ + src/graph/ + src/llm/
gleb: src/market/ + src/knowledge/
erik: src/trading/ + src/news/
member4: tests/ + src/telemetry/

## Shared (coordinate before touching)
src/config.py | src/types.py | GEMINI.md | pyproject.toml
```

## Rules

### Working in own zone
→ Work freely. No coordination needed.

### Working in foreign zone
→ Check TEAM_ZONES.md → Notify owner in team chat first.
→ Get acknowledgement before starting.

### Shared files (config, types)
→ One person commits, others rebase.
→ Announce in chat: "Обновляю config.py — не трогайте 5 мин"

## Merge protocol
```
1. git rebase develop (before PR)
2. PR → assign reviewer (zone owner or any teammate)
3. Min 1 approval required
4. Squash merge to develop
5. Delete feature branch after merge
```

## Session logging (multi-person)
Each person appends to `.ai/SESSIONS.md` with `@name` tag:
```markdown
## 2026-05-12T18:30 | @vsevolod | branch:vsevolod/feat/risk-mgr
Done: impl risk_manager.py ✅
Next: !1 tests for risk limits

## 2026-05-12T19:00 | @gleb | branch:gleb/feat/indicators
Done: Hurst Exponent calculation ✅
Next: !1 Kelly Criterion impl
```

## Conflict resolution
| Situation | Action |
|-----------|--------|
| Both edit different zones | No conflict — merge freely |
| Both edit shared file | First commits wins, second rebases |
| Merge conflict | Whoever touched shared code resolves |
| Architecture disagreement | Discuss → vote → majority wins |

## Skills sharing
Skills repo = shared git. Update workflow:
1. Someone updates a skill → PR to skills repo
2. Review by 1 teammate
3. Merge → all pull: `cd ~/.gemini/antigravity/skills && git pull`

## Daily sync
Quick async check in team chat:
- What I did today
- What I'm doing tomorrow
- Any blockers?
