---
name: context-compiler
description: >
  Сбор исчерпывающего контекста проекта для задачи через MCP-сервер context-compiler.
  Автономный агент сканирует кодовую базу и опрашивает NotebookLM, создавая .ai/CONTEXT.md.
  Используй перед любой сложной задачей (>2 файлов), при работе с незнакомым проектом,
  или когда пользователь говорит "собери контекст", "проанализируй проект", "context".
---

# context-compiler — Full Project Context Builder

## When to trigger

- Complex task (>2 files, unfamiliar module)
- First entry into a project → need stack, patterns, deps
- User says: "собери контекст", "проанализируй проект", "context"
- `hard` mode → mandatory first step
- Before architectural decisions

Skip if: simple edits (1-2 files), `.ai/CONTEXT.md` is fresh, no notebook_id.

## How to call

```
call_mcp_tool(
  ServerName="context-compiler",
  ToolName="build_context",
  Arguments={
    "task": "Task description",
    "notebook_id": "NotebookLM notebook ID",
    "workspace": "/path/to/project"
  }
)
```

| Param | Req | Description |
|-------|:---:|-------------|
| `task` | ✅ | Specific task description |
| `notebook_id` | ✅ | NotebookLM notebook ID |
| `workspace` | — | Project path (default: saas-platform) |

To get notebook_id → `notebook_list` via MCP `notebooklm-mcp` → find by name.

## Pipeline (under the hood)

1. Bootstrap → project_tree(depth=4), configs, git_log(30)
2. Decomposition → identify domains (API, DB, UI, Celery, Auth...)
3. Collection → 25+ NLM questions × 7 categories + code verification
4. Self-audit → 7 confidence aspects, gap filling (up to 7 rounds)
5. Compilation → save_context → `.ai/CONTEXT.md`

Duration: 3-10 min. Model: Gemma 4 31B-IT (1500 RPD free tier).

## Output: .ai/CONTEXT.md

Sections: Stack (versions + SOURCE) | Implemented (modules, files, API) | Patterns (with code examples) | Dependencies (table) | Anti-patterns | Integrations (connection map) | Testing | Env vars | Confidence (7 aspects).

History copy → `.ai/context_history/CONTEXT_YYYYMMDD_HHMMSS.md`.

## Workflow

```
Complex task → build_context(task, notebook_id) → view_file(.ai/CONTEXT.md) → implement
```

## Requirements

- MCP `context-compiler` running + `GEMINI_API_KEY` in env
- Project loaded into NotebookLM (notebook_id exists)

## Related skills

- `notebooklm` → notebook management, adding sources
- `architecture-map` → project map (after context)
- `deep-context` → deep reasoning tasks
