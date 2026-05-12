---
name: git-commit
description: >
  Pre-commit checks: staged files, forbidden patterns, lint, tests.
  Builds conventional commit message in Russian. Detects stack
  (Python/TS) for correct lint/test commands. Never `git add .`.
---

# git-commit — Multi-Stack Pre-Commit Protocol

## Step 0: Stack detection
Read `.ai/SKILL_CONFIG.md` → get lint + test commands.

## Procedure

### 1. Review changes
```bash
git status
git diff --staged
```

### 2. Block forbidden files
| Pattern | Reason |
|---------|--------|
| `.env*` | secrets |
| `node_modules/`, `__pycache__/`, `.venv/` | deps |
| `dist/`, `build/`, `.next/`, `.mypy_cache/` | build artifacts |
| `*.log`, `*.pyc` | generated |
| `AKIA[0-9A-Z]{16}`, `sk-`, `ghp_`, `password\s*=\s*['"]` | hardcoded secrets |

Found? → warn + unstage. NEVER commit.

### 3. Size check
Staged > 10 files → warn: "Разбей на атомарные коммиты"

### 4. Atomicity
All changes solve ONE task? Mixed → recommend splitting.

### 5. Quality checks (graceful)

| Stack | Lint | Test |
|-------|------|------|
| Python (`pyproject.toml`) | `ruff check .` | `pytest --tb=short -q` |
| TS (`package.json`) | `pnpm lint` | `pnpm test` |
| Neither | skip, note "линтер не настроен" | skip |

Fail → show errors, propose fix, DO NOT commit.

### 6. Commit message
Conventional commits, scope in English, message in Russian:
```
feat(trading): реализовать детерминированный риск-менеджер
fix(market): исправить парсинг MOEX ISS ответов
refactor(agents): вынести общую логику дебатов
test(risk): покрыть тестами проверку лимитов
docs(readme): обновить инструкцию по запуску
chore(deps): обновить langchain до 0.3.x
```

### 7. Update session
`.ai/SESSION_STATE.md` exists? → update Facts layer.

### 8. Output exact commands
```bash
git add <specific_files>
git commit -m "<type>(<scope>): <msg>"
```

**NEVER** `git add .`

→ After: `session-handoff` if significant milestone
