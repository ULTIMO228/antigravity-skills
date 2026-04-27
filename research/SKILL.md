---
name: research
description: >
  Используй когда встречаешь незнакомую технологию, библиотеку, API или паттерн. Вместо
  того чтобы гадать — проведи исследование: проверь официальную документацию, changelog,
  GitHub issues, совместимость версий. Предотвращает использование устаревших API и
  несовместимых решений.
---

# research

## Procedure

1. **Identify gap**: что именно незнакомо? Библиотека? API? Паттерн? Версия?
2. **Check sources** (в этом порядке):
   - Official docs (всегда первый источник) → `read_url_content` на docs URL
   - GitHub repo: README, CHANGELOG.md, releases page → breaking changes?
   - GitHub issues/discussions → known bugs, workarounds
   - `package.json` / lock file → exact installed version
   - Migration guides (если обновляли версию)
3. **Version check**: installed version matches docs version? Old docs ≠ current API.
4. **Verify compatibility**:
   - Does lib X work with framework Y at version Z?
   - Check peer dependencies
   - Check minimum Node/Python/runtime version
5. **Summarize findings** to user:
   ```
   Исследовал <library>:
   - Версия: <installed> (latest: <latest>)
   - Ключевое: <что важно знать>
   - Gotchas: <подводные камни>
   - Breaking changes от нашей версии: <если есть>
   ```
6. If research insufficient → recommend `deep-context` for deeper analysis

## Anti-patterns (чего НЕ делать)
- ❌ Не использовать устаревшие примеры со StackOverflow без проверки версии
- ❌ Не предполагать API по названию — всегда проверять docs
- ❌ Не копировать код из примеров без понимания что он делает
- ❌ Не игнорировать deprecation warnings
