---
name: notebooklm
description: Work with Google NotebookLM via MCP — manage notebooks, add sources, query content, generate audio overviews, mind maps and research. Use when user asks to work with NotebookLM, save information to notebooks, query knowledge bases, or generate AI summaries.
---

# NotebookLM MCP Skill

You have access to Google NotebookLM via the `notebooklm-mcp` MCP server. Use it to manage the user's notebooks, add sources, and query grounded knowledge.

## Authentication

Session is stored in `~/.notebooklm-mcp/auth.json`. If tools start failing with auth errors, run:
```
notebooklm-mcp-server refresh_auth
```
Or tell the user to run `~/npm-global/bin/notebooklm-mcp-auth` in terminal.

## Core Workflow

### 1. Always start by listing notebooks
```
notebook_list → get notebook IDs
```

### 2. Adding sources to a notebook
- URL or YouTube: `notebook_add_url(notebook_id, url)`
- Raw text: `notebook_add_text(notebook_id, title, content)`
- Local file (PDF/MD/TXT): `notebook_add_local_file(notebook_id, path)`
- Google Drive file: `notebook_add_drive(notebook_id, file_id)`

### 3. Querying a notebook
```
notebook_query(notebook_id, query)
```
Returns grounded answers with citations from the notebook sources. Optionally pass `conversation_id` to continue a chat thread.

### 4. Research (web or Drive search)
```
research_start(notebook_id, query, source="web", mode="fast"|"deep")
→ research_poll(notebook_id)   # repeat until done
→ research_import(notebook_id, task_id, sources=[...])
```

### 5. Audio Overview (podcast)
```
audio_overview_create(notebook_id, source_ids=[], language="en")
→ studio_poll(notebook_id)   # check status, get download URL
```

### 6. Mind Maps
```
mind_map_generate(source_ids=[...])
→ mind_map_save(notebook_id, mind_map_json, source_ids, title)
```

## Best Practices

- **Always get notebook_id first** — never guess IDs, always call `notebook_list`
- **notebook_query is grounded** — answers come only from sources in the notebook, zero hallucination
- **research_poll is async** — call it in a loop until status is `DONE`; typically takes 10–60 seconds
- **audio_overview_create is async** — use `studio_poll` to check, may take several minutes
- **notebook_delete is destructive** — always confirm with user before deleting
- **source_ids are optional for audio** — omit to include all sources in the notebook

## Available Tools Summary

| Tool | Purpose |
|------|---------|
| `notebook_list` | List all notebooks |
| `notebook_create` | Create new notebook |
| `notebook_rename` | Rename notebook |
| `notebook_delete` | ⚠️ Delete notebook (irreversible) |
| `notebook_add_url` | Add URL/YouTube as source |
| `notebook_add_text` | Add text as source |
| `notebook_add_local_file` | Add local PDF/MD/TXT |
| `notebook_add_drive` | Add Google Drive file |
| `source_delete` | Remove a source |
| `source_sync` | Sync Drive source |
| `notebook_query` | Ask grounded question |
| `research_start` | Start web/Drive research |
| `research_poll` | Check research progress |
| `research_import` | Import research results |
| `audio_overview_create` | Generate podcast |
| `studio_poll` | Check audio generation status |
| `mind_map_generate` | Generate mind map JSON |
| `mind_map_save` | Save mind map to notebook |
| `mind_map_list` | List saved mind maps |
| `mind_map_delete` | Delete mind map |
| `refresh_auth` | Renew Google session |
