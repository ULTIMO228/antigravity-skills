---
name: skill-writer
description: >
  Guide for creating new Antigravity skills. Covers SKILL.md structure,
  YAML frontmatter, body format, references/, scripts/, examples/.
  Trigger: user asks to create a skill, write a skill, or "new skill".
---

# skill-writer — How to Create Antigravity Skills

## Skill = directory in customization root

```
skills/
  my-skill/
    SKILL.md              ← required: instructions + YAML frontmatter
    references/           ← optional: deep docs loaded on demand
      python-patterns.md
      ts-patterns.md
    scripts/              ← optional: helper scripts
    examples/             ← optional: reference implementations
    resources/            ← optional: templates, assets
```

Standard roots:
- Global: `~/.gemini/config/skills/`
- Workspace: `.agents/skills/`

Skills in standard roots auto-discover. Non-standard paths → register in `skills.json`.

## SKILL.md Structure

### 1. YAML Frontmatter (required)

```yaml
---
name: skill-name          # kebab-case, unique
description: >            # 2-4 lines, used for trigger matching
  What the skill does. When to activate it.
  Specific trigger words/phrases the user might say.
---
```

Rules:
- `name` + `description` = **only fields used for trigger matching**
- Description must contain trigger phrases (both EN and RU if bilingual user)
- Keep description under 4 lines — dense, specific, no fluff
- Use `>` for multiline YAML strings

### 2. Body (Markdown instructions)

```markdown
# skill-name — Short Title

## When to trigger / Step 0
...
## Procedure / Steps
...
```

## Body Format Rules

### Language
- **English only** in SKILL.md body
- Exception: trigger phrases in description can be bilingual
- Exception: user-facing output templates can be in Russian

### Style (follow existing patterns)
| Do | Don't |
|----|-------|
| Tables for structured data | Long paragraphs |
| `→` for flow/sequence | "and then you should" |
| `\|` for alternatives | "or alternatively" |
| Numbered steps | Unstructured prose |
| Code blocks for commands | Inline descriptions |
| One line = one fact | Multi-sentence lines |
| Imperative verbs | "You should consider" |

### Size
- Target: 40-120 lines
- Max: 500 lines (use `references/` for overflow)
- Under 40 lines → probably too shallow

### Typical sections

```markdown
# name — Short Title

## Step 0: Context                    ← prerequisites, what to read first
## When to trigger                    ← activation conditions
## Procedure                          ← numbered steps with tables
## Output format                      ← what the skill produces
## Anti-patterns                      ← what NOT to do
## Related skills                     ← → after: `other-skill`
```

## Optional Directories

### references/
Deep documentation loaded **on demand** (not auto-loaded).

```
references/
  python-patterns.md    ← loaded when stack=Python
  ts-patterns.md        ← loaded when stack=TypeScript
```

Use when: >500 lines of docs, stack-specific content, API references.
Reference from SKILL.md: `Read references/python-patterns.md`

### scripts/
Helper scripts that extend agent capabilities.

```
scripts/
  analyze.sh           ← run via run_command
  template.py
```

### examples/
Reference implementations for the agent to follow.

```
examples/
  good-test.py         ← "write tests like this"
  bad-test.py          ← "don't write tests like this"
```

## Trigger Matching

The system matches skills by `name` + `description` fields against user request.

Tips for good matching:
- Include Russian trigger words: "собери контекст", "ревью кода"
- Include English equivalents: "build context", "code review"
- Include command-like phrases: user says "review" → skill triggers
- Be specific: "webhook handler" > "backend stuff"

## Integration Patterns

### Stack detection
```markdown
## Step 0
Read `.ai/SKILL_CONFIG.md` → load matching reference:
- Python → read `references/python-checklist.md`
- TS → read `references/ts-checklist.md`
```

### DenseCode output
```markdown
Write to `.ai/` → apply `densecode` skill first
```

### Chaining
```markdown
→ After: `git-commit` | `code-review`
```

### MCP tool call
```markdown
call_mcp_tool(ServerName="...", ToolName="...", Arguments={...})
```

## Checklist Before Publishing

- [ ] `name` is kebab-case, unique across all skills
- [ ] `description` has trigger phrases (EN + RU)
- [ ] Body is **English only**
- [ ] No hardcoded paths, secrets, tokens
- [ ] Size: 40-120 lines (use references/ for overflow)
- [ ] Tables over paragraphs for structured data
- [ ] Imperative verbs, no fluff
- [ ] Tested: agent correctly triggers on relevant user request
- [ ] Related skills linked with `→`

## Example: Minimal Skill

```markdown
---
name: my-tool
description: >
  Brief description of what this does and when to use it.
  Trigger: user says "do the thing" or "сделай штуку".
---

# my-tool — Short Title

## When to trigger
- Condition A
- Condition B
- User says: "trigger phrase"

## Procedure

### 1. Gather context
Read `.ai/PROJECT_MAP.md` → understand scope.

### 2. Execute
| Input | Action | Output |
|-------|--------|--------|
| X | do Y | Z |

### 3. Verify
Run `command` → check result.

→ After: `related-skill`
```
