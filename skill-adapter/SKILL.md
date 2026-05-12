---
name: skill-adapter
description: >
  Detects project stack and generates .ai/SKILL_CONFIG.md mapping each
  skill to its correct reference file. Run on first project entry or
  when user says "adapt skills". Ensures skills auto-load correct refs.
---

# skill-adapter — Project Stack Detection + Skill Mapping

## Trigger
- First entry into a project (no `.ai/SKILL_CONFIG.md` exists)
- User command: "адаптируй скиллы" / "adapt skills"
- After major stack change (new framework, language switch)

## Procedure

### Step 1: Detect stack
```
Read GEMINI.md → extract: language, frameworks, linter, test runner
Read pyproject.toml | package.json → extract: deps, scripts, tools
Read .gemini/agents/ → list available subagents
```

### Step 2: Build mapping

For each skill with `references/` directory:

| Stack Signal | Skill → Reference |
|-------------|-------------------|
| `pyproject.toml` + `pytest` | testing → references/python-pytest.md |
| `package.json` + `vitest\|jest` | testing → references/ts-vitest.md |
| `ruff` in pyproject.toml | git-commit → `ruff check .` |
| `eslint` in package.json | git-commit → `pnpm lint` |
| Python + FastAPI | security-audit → references/python-security.md |
| TypeScript + Express | security-audit → references/ts-security.md |
| Python project | code-review → references/python-checklist.md |
| TS project | code-review → references/ts-checklist.md |

### Step 3: Write `.ai/SKILL_CONFIG.md`

Apply `densecode` skill:

```markdown
# SKILL_CONFIG
## Meta
Stack:Python3.11 | Lang:py | Linter:ruff | Tests:pytest
PkgMgr:pip | Framework:LangGraph+FastAPI | Generated:<ISO_date>

## Skill Mappings
testing → references/python-pytest.md
code-review → references/python-checklist.md
security-audit → references/python-security.md
git-commit → lint:ruff check . | test:pytest --tb=short -q
deploy → Docker multi-stage
architecture-map → mindmap + file tree

## Available Agents
Pro: python-dev, quant, langgraph-architect, reviewer, researcher, tester, data-analyst, prompt-smith
Flash: documenter
Lite: devops

## Lint Command
ruff check . --fix

## Test Command
pytest --tb=short -q

## Forbidden Files (.gitignore check)
__pycache__/ | .venv/ | *.pyc | .env* | .mypy_cache/
```

### Step 4: Notify
"Скиллы адаптированы под <stack>. Config: `.ai/SKILL_CONFIG.md`"

## How other skills use this

Every skill starts with:
```
Step 0: Read .ai/SKILL_CONFIG.md → load matching reference
```

If SKILL_CONFIG.md not found → warn user: "Запусти skill-adapter"

## Multi-language projects

If project has BOTH pyproject.toml AND package.json:
- Map Python references for /src, /backend
- Map TS references for /frontend, /web
- Note in SKILL_CONFIG: `Multi-stack: py@backend + ts@frontend`
