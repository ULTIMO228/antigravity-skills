---
name: research
description: >
  Deep research loop for unfamiliar tech, APIs, patterns. Decomposes
  query, searches multiple sources, cross-verifies, fills gaps, saves
  findings to .foryou/research/. Prevents outdated API usage.
---

# research — Deep Research Loop

## Procedure

### Step 0: Context
Read `.ai/SKILL_CONFIG.md` → know current stack + versions.

### Step 1: Decompose
Break query → sub-questions:
```
"How does Hurst Exponent detect market regimes?"
→ Q1: What is Hurst Exponent? (math definition)
→ Q2: How to calculate in Python? (libraries, code)
→ Q3: How to interpret H values for regime detection?
→ Q4: Existing implementations in trading systems?
```

### Step 2: Search (parallel when possible)
Priority order:
1. Official docs → `read_url_content` on docs URL
2. GitHub repo: README, CHANGELOG, releases → breaking changes?
3. GitHub issues/discussions → known bugs, workarounds
4. Academic papers / reliable sources → for domain topics
5. Package manifest → exact installed version

### Step 3: Verify
| Check | How |
|-------|-----|
| Version match | installed version matches docs version? |
| Compatibility | lib X + framework Y at version Z? |
| Peer deps | check requirements |
| Deprecation | any deprecation warnings? |
| Cross-reference | ≥ 2 sources agree? |

### Step 4: Gap check
Missing info after Step 2-3?
→ Refine query, re-search with new keywords.
→ Max 3 iterations to prevent infinite loop.

### Step 5: Synthesize
Structure findings:
```
Исследовал <topic>:
- Версия: <installed> (latest: <latest>)
- Ключевое: <core findings>
- Gotchas: <pitfalls and edge cases>
- Breaking changes: <if any from our version>
- Рекомендация: <what to do>
- Источники: <URLs>
```

### Step 6: Save
Write `.foryou/research/<topic>.md` — full report in Russian.
Beautiful formatting with headers, code examples, links.

### Step 7: Update session
Append note to `.ai/SESSIONS.md`: `Research:<topic> => <1-line summary>`

## Anti-patterns
- ❌ Using outdated StackOverflow answers without version check
- ❌ Assuming API from name — always verify docs
- ❌ Copy-paste code without understanding
- ❌ Ignoring deprecation warnings
- ❌ Single-source conclusions

→ If research insufficient: recommend `deep-context` skill
