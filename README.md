# Antigravity Skills

Скиллы и глобальные правила для [Antigravity](https://antigravity.dev) AI-агента.

## Структура

```
skills/                  ← скиллы агента
  architecture-map/      ← карта проекта (агент + человек)
  changelog/             ← генерация CHANGELOG из коммитов
  code-review/           ← ревью кода по чеклисту
  deep-context/          ← сборка контекста для Deep Think моделей
  error-recovery/        ← выход из doom loop
  git-commit/            ← безопасный коммит с conventional commits
  ideas-suggest/         ← предложение улучшений
  refactoring/           ← безопасный рефакторинг (test-first)
  research/              ← исследование незнакомых технологий
  security-audit/        ← аудит по OWASP Top 10
  session-handoff/       ← передача контекста между сессиями
  testing/               ← написание тестов по AAA

GEMINI.md                ← глобальные правила агента
```

## Как использовать

Скиллы подключаются к Antigravity автоматически. Активация — по триггеру в чате
или агент вызывает нужный скилл сам по контексту задачи.

Каждый скилл содержит `SKILL.md` с описанием и пошаговым протоколом выполнения.
