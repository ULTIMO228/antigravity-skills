---
name: git-commit
description: >
  Используй перед каждым git commit. Проверяет staged-файлы на мусор, запускает линтер
  и тесты, формирует conventional commit message на русском, обновляет SESSION_STATE.md.
  Предотвращает коммит .env, node_modules и сломанного кода.
---

# git-commit

## Procedure

1. `git status` + `git diff --staged` → review changes
2. **Block forbidden files**: `.env*`, `node_modules/`, `dist/`, `build/`, `.next/`, `*.log`, files matching `AKIA|sk-|ghp_|password\s*=\s*['"]` → warn + unstage
3. **Size check**: staged > 10 files → warn «разбей на атомарные коммиты»
4. **Atomicity**: all changes solve ONE task? If mixed → recommend splitting
5. **Quality** (graceful):
   - `lint` script in package.json? → run. Not found? → skip, note «линтер не настроен»
   - `test` script? → run. Not found? → skip, note «тесты не настроены»
   - Fail → show errors, propose fix, DO NOT commit
6. **Message**: conventional commit, русский:
   ```
   feat(auth): реализовать JWT авторизацию
   fix(api): исправить валидацию email
   refactor(services): вынести логику из контроллеров
   ```
7. Update `.ai/SESSION_STATE.md` if exists
8. Output exact command:
   ```bash
   git add <конкретные_файлы>
   git commit -m "<type>(<scope>): <msg>"
   ```
   NEVER `git add .`
