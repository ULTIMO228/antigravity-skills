---
name: architecture-map
description: >
  Generates dual project maps: compressed .ai/PROJECT_MAP.md (densecode)
  and beautiful .foryou/PROJECT_MAP.md (Russian, Mermaid, file tree).
  Trigger: new project, major feature, structure change (>2 files).
---

# architecture-map — Dual Project Map Generator

## Procedure

### Step 0: Context
Read `.ai/SKILL_CONFIG.md` if exists → detect stack.

### Step 1: Scan
```
Scan: file_tree → pkg manifest (pyproject.toml | package.json)
Identify: {modules, roles, deps, entries, data_flows}
Read: GEMINI.md → project rules + stack
```

### Step 2: Write `.ai/PROJECT_MAP.md` (densecode)

Apply `densecode` skill. Format:

```markdown
# PROJECT_MAP
## Meta
Proj:<name> | Stack:{<frameworks>} | Lang:<lang> | PkgMgr:<mgr>
Entry:<file> | Port:<port> | Updated:<ISO_date>

## Tree
/src → main code
  /agents → LLM agents | gold_std:debater.py
  /market → MOEX ISS client + indicators
  /trading → Risk Manager + strategies + executor
  /llm → LLM client, schemas, models
  /knowledge → Knowledge Graph (NetworkX)
  /news → RSS + Browser Use
  /graph → LangGraph assembly
  /config.py → Pydantic Settings (singleton)

## Flows
Flow:trade → signal:detect → risk:check → order:place => executed ✅
Flow:research → rss:fetch → llm:analyze → kg:update => insight
Flow:decision → agents:debate → judge:decide → risk:validate => action | reject

## Deps
agents → {llm, knowledge, market}
trading → {market, config}
graph → {agents, trading}

## Gold Standards
agent: src/agents/debater.py
config: src/config.py
```

### Step 3: Write `.foryou/PROJECT_MAP.md` (human-readable)

Russian language. Beautiful formatting. Include:

#### 3a. Overview section
```markdown
# 🗺️ Карта проекта: <Name>
## Обзор
<что делает, для кого, ключевые решения>

## Стек
| Категория | Технология | Версия | Назначение |
|-----------|-----------|--------|------------|
```

#### 3b. Architecture diagram (Mermaid)
Use `references/mermaid-templates.md` for templates.

#### 3c. File tree with descriptions
```
moex-cortex/
├── src/
│   ├── config.py           — Конфигурация (Pydantic Settings, .env)
│   ├── main.py             — Точка входа + scheduler
│   ├── agents/             — LLM-агенты системы
│   │   ├── watcher.py      — Мониторинг рыночных событий
│   │   ├── analyst.py      — Анализ ситуации
│   │   └── debater.py      — Дебаты Bull vs Bear
│   ├── market/             — Работа с MOEX
│   │   ├── client.py       — ISS API клиент
│   │   └── indicators.py   — Технические индикаторы
│   └── trading/            — Торговая логика
│       ├── risk_manager.py — Детерминированный риск-менеджер
│       └── strategies.py   — Стратегии из PLAYBOOK
├── tests/                  — Тесты (pytest)
├── .ai/                    — Контекст для ИИ (DenseCode)
└── .foryou/                — Документация для людей
```

#### 3d. Data flow diagrams (Mermaid sequenceDiagram)
#### 3e. Key decisions table

```markdown
## Ключевые решения
| Решение | Почему | Альтернативы |
|---------|--------|-------------|
```

### Step 4: Verify
Both files cover all modules. .ai/ is compressed. .foryou/ is beautiful.

### Step 5: Incremental updates
New module/flow/dep → update BOTH files.
Deleted module → remove from BOTH.

→ After: consider `session-handoff`
