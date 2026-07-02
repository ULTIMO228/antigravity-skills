---
name: cli-bridge
description: Синхронизирует и настраивает двустороннюю связь скиллов Antigravity CLI с Antigravity 2.0.
---

# cli-bridge — Мост скиллов Antigravity CLI ↔ 2.0

Скилл обеспечивает бесшовную интеграцию и двустороннюю связь между экосистемами Antigravity CLI (1.x) и настольного приложения Antigravity 2.0.

## Как это работает
Скилл связывает директории `~/.gemini/antigravity-cli/skills` и `~/.gemini/antigravity/skills` с директорией глобальных кастомизаций Antigravity 2.0 (`~/.gemini/config/skills/`) с помощью символических ссылок. Любое добавление или редактирование скиллов в CLI мгновенно отражается в Antigravity 2.0 и наоборот.

## Восстановление моста
Если символические ссылки были нарушены или удалены, вы можете восстановить их, выполнив команду:
```bash
node /Users/seva/.gemini/antigravity/scratch/antigravity-skills-bridge/bridge.js
```
