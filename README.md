# Antigravity Skills

> Библиотека скиллов и глобальных правил для AI-агента Antigravity (Claude).  
> English-first, token-efficient, production-ready.

## Что это

Скиллы — это специализированные инструкции, которые Antigravity подгружает по контексту задачи. Каждый скилл активируется автоматически или по явному триггеру и даёт агенту точный протокол действий.

Принципы системы:
- **Progressive disclosure** — скилл загружается только когда нужен
- **DenseCode** — максимально сжатый формат, минимум токенов
- **Multi-stack** — Python и TypeScript покрыты в каждом скилле
- **References** — детали вынесены в `references/`, SKILL.md остаётся < 200 строк

---

## Скиллы

### 🏗️ Фундамент
| Скилл | Триггер | Описание |
|-------|---------|----------|
| [`densecode`](densecode/) | любая запись в `.ai/` | Стандарт сжатия: English, symbols, abbreviations |
| [`skill-adapter`](skill-adapter/) | вход в проект | Определяет стек, генерирует `.ai/SKILL_CONFIG.md` |
| [`gemini-dispatch`](gemini-dispatch/) | нужен субагент | Оркестрация Gemini CLI субагентов (только pre-middle задачи) |
| [`session-handoff`](session-handoff/) | конец сессии | Сохранение контекста в `.ai/SESSIONS.md` + `SESSION_STATE.md` |
| [`architecture-map`](architecture-map/) | новый проект / структурные изменения | Генерация карты проекта для AI и людей |

### 🔍 Качество кода
| Скилл | Триггер | Описание |
|-------|---------|----------|
| [`testing`](testing/) | после фичи | AAA-паттерн, pytest/Vitest, min 3 теста на функцию |
| [`code-review`](code-review/) | перед мерджем | Чеклист Critical→Quality, оценка 1-10 |
| [`security-audit`](security-audit/) | перед деплоем | OWASP Top 10, secret scan |
| [`refactoring`](refactoring/) | по запросу | Test-first, атомарные шаги |

### 🛠️ Workflow
| Скилл | Триггер | Описание |
|-------|---------|----------|
| [`git-commit`](git-commit/) | коммит | Conventional commits, multi-stack lint+test |
| [`research`](research/) | незнакомая технология | Deep loop: decompose → verify → save |
| [`error-recovery`](error-recovery/) | 2+ неудачных попытки | Выход из doom loop |
| [`ideas-suggest`](ideas-suggest/) | "предложи улучшения" | Анализ + предложения по impact/effort |

### 🐍 Python-специфика
| Скилл | Триггер | Описание |
|-------|---------|----------|
| [`python-patterns`](python-patterns/) | Python код | Typing 3.11+, Pydantic v2, asyncio patterns |
| [`prompt-engineering`](prompt-engineering/) | LLM pipeline | Structured outputs, Pydantic schemas, instructor |

### 🚀 DevOps & Design
| Скилл | Триггер | Описание |
|-------|---------|----------|
| [`deploy`](deploy/) | контейнеризация | Dockerfile, docker-compose, GitHub Actions |
| [`team-sync`](team-sync/) | командная работа | Branch naming, zones, merge protocol |
| [`design-engineering`](design-engineering/) | UI/UX | Анимации, типографика, anti-patterns |
| [`deep-context`](deep-context/) | "hard mode" | Сборка контекста для Deep Think моделей |
| [`changelog`](changelog/) | релиз | CHANGELOG из git-коммитов |
| [`pptx`](pptx/) | презентации | Создание и редактирование .pptx |

---

## Структура скилла

```
skill-name/
├── SKILL.md              ← инструкции (< 200 строк, English, compressed)
└── references/           ← детали по стекам (загружаются по требованию)
    ├── python-*.md
    └── ts-*.md
```

## Как работает dispatch

Сложные задачи (архитектура, алгоритмы, multi-file) — Antigravity делает сам.  
Простые и средние (≤ pre-middle) — делегируются Gemini CLI субагентам:

```bash
gemini -p "@tester Write tests for src/market/parser.py" --yolo
```

Подробнее — [`gemini-dispatch/SKILL.md`](gemini-dispatch/SKILL.md)

## Глобальные правила

[`GEMINI.md`](GEMINI.md) — настройки агента: режимы работы (fast/normal/hard), роутинг моделей, воркфлоу, код-стайл.

---

## Команда

Скиллы разделяются между участниками через git:
```bash
# Обновить скиллы
cd ~/.gemini/antigravity/skills && git pull

# Добавить/изменить скилл → PR → 1 ревью → merge
```
