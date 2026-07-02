---
name: frontend-agents
description: >
  Orchestration protocol for 12 specialized frontend coding agents.
  Use when building frontend features for the SaaS platform.
  Reads .agents/agent.md to determine which agent(s) to invoke,
  loads their knowledge base, and executes via /teamwork-preview.
  Triggers: any frontend task — components, pages, forms, animations,
  auth, API hooks, tests, landing pages, booking UI.
---

# frontend-agents — Frontend Agent Orchestrator

## When to Use

Activate this skill when the task involves ANY frontend work:
- Building UI components, pages, or layouts
- Creating forms with validation
- Adding animations or scroll effects
- Implementing auth flows or route guards
- Connecting to API endpoints
- Writing frontend tests
- Building landing pages
- Creating booking/calendar UI
- Setting up design tokens or typography
- Adding accessibility or ARIA attributes
- Defining TypeScript schemas or Zod contracts
- Managing permissions and RBAC UI

## Step 1: Read the Agent Index

**ALWAYS start by reading the agent index:**

```
Read file: <project_root>/.agents/agent.md
```

This file contains:
- Quick Dispatch Table — "when you need X → call Y"
- Full descriptions with scope and triggers for each agent
- Collaboration rules and dependency order
- Typical feature flow (1→10 pipeline)

## Step 2: Classify the Task

Map the user's request to one or more agents using the Dispatch Table:

| Task Domain | Agent(s) | Priority |
|-------------|----------|----------|
| Design tokens, colors, fonts | @visual-designer | First — foundation |
| Reusable components | @ui-coder | After design tokens |
| Accessibility, states | @ux-engineer | After components |
| Animations, scroll effects | @animator | After components |
| Forms with validation | @form-specialist | After types |
| Auth, JWT, login/register | @auth-specialist | Independent |
| API hooks, fetch wrapper | @api-connector | After types |
| Permissions, role gates | @rbac-ui | After auth |
| TypeScript types, Zod schemas | @type-smith | First — contracts |
| Unit/integration tests | @fe-tester | Last — after feature |
| Calendar, booking, DnD | @booking-ui | After API hooks |
| Landing page, SSG, scroll | @landing-dev | Independent |

### Single-agent task
If the task maps to exactly ONE agent → proceed to Step 3.

### Multi-agent task (feature pipeline)
If the task requires multiple agents, follow the **Typical Feature Flow**:
1. @type-smith → define schemas
2. @visual-designer → tokens (if new styles)
3. @ui-coder → components
4. @ux-engineer → states + a11y
5. @api-connector → query hooks
6. @form-specialist → forms (if needed)
7. @auth-specialist → guards (if needed)
8. @rbac-ui → permissions (if needed)
9. @animator → animations
10. @fe-tester → tests

**Execute agents in dependency order, not in parallel.**

## Step 3: Load Agent Knowledge Base

Before executing, read the agent's full context:

```
1. Read: .agents/<agent-folder>/agent.md        ← role, rules, stack
2. Read: .agents/<agent-folder>/docs-summary.md  ← library reference
3. Read: .agents/<agent-folder>/patterns.md      ← code templates
4. Read: .agents/<agent-folder>/lessons.md       ← known pitfalls
5. Read: .agents/<agent-folder>/memory.md        ← project state
```

**Critical:** The knowledge base contains project-specific decisions, patterns, and pitfalls.
Ignoring it leads to inconsistent code that contradicts existing architecture.

## Step 4: Execute via /teamwork-preview

Invoke the agent using `/teamwork-preview` subagent dispatch:

### Agent Configuration

| Agent | Model | Thinking |
|-------|-------|----------|
| @animator | Gemini 3.5 Flash | — |
| @visual-designer | Gemini 3.5 Flash | — |
| @form-specialist | Gemini 3.5 Flash | — |
| @ui-coder | Claude Opus 4.6 | Thinking |
| @ux-engineer | Claude Opus 4.6 | Thinking |
| @auth-specialist | Claude Opus 4.6 | Thinking |
| @api-connector | Claude Opus 4.6 | Thinking |
| @rbac-ui | Claude Opus 4.6 | Thinking |
| @type-smith | Claude Opus 4.6 | Thinking |
| @fe-tester | Claude Opus 4.6 | Thinking |
| @booking-ui | Claude Opus 4.6 | Thinking |
| @landing-dev | Claude Opus 4.6 | Thinking |

### Prompt Template for Subagent

```
You are @<agent-name>, a specialized frontend agent.

## Your Knowledge Base
<paste content from agent.md>

## Key Patterns
<paste relevant patterns from patterns.md>

## Known Pitfalls
<paste from lessons.md>

## Current Project State
<paste from memory.md>

## Task
<user's specific task>

## Rules
- All code comments and communication in Russian
- Follow patterns from your knowledge base exactly
- Update memory.md with session log at the end
- Record any new lessons in lessons.md
```

## Step 5: Post-Execution Protocol

After the agent completes work:

### 1. Update Memory
Append to `.agents/<agent-folder>/memory.md`:
```markdown
### Session YYYY-MM-DD (Task Description)
- ✅ Done: what was accomplished
- 📁 Files: created/modified files
- 🧠 Decisions: architectural choices made
- ➡️ Next: what to do next
```

### 2. Record Lessons (if any errors occurred)
Append to `.agents/<agent-folder>/lessons.md`:
```markdown
#### !N Error Title
\`\`\`
Problem: what broke
Fix: how it was fixed
Prevention: how to avoid in future
\`\`\`
```

### 3. Chain to Next Agent (if multi-agent pipeline)
If this was part of a multi-agent feature flow, proceed to the next agent in the pipeline.

## Folder Structure Reference

```
.agents/
├── agent.md                                          ← INDEX (read first)
├── animator-gsap-framer-scroll-effects/              ← @animator
├── visual-designer-design-tokens-colors-typography/  ← @visual-designer
├── form-specialist-react-hook-form-zod-validation/   ← @form-specialist
├── ui-coder-react-components-radix-tailwind/         ← @ui-coder
├── ux-engineer-states-accessibility-aria/            ← @ux-engineer
├── auth-specialist-jwt-login-registration-guards/    ← @auth-specialist
├── api-connector-tanstack-query-fetch-hooks/         ← @api-connector
├── rbac-ui-permissions-gates-role-management/        ← @rbac-ui
├── type-smith-typescript-zod-schemas-contracts/      ← @type-smith
├── fe-tester-vitest-rtl-msw-coverage/                ← @fe-tester
├── booking-ui-calendar-slots-dnd-scheduling/         ← @booking-ui
└── landing-dev-ssg-scroll-animations-premium/        ← @landing-dev
```

## Rules

| Rule | Why |
|------|-----|
| ALWAYS read agent.md index first | Know which agent(s) to call |
| ALWAYS load knowledge base before execution | Context prevents inconsistent code |
| ALWAYS follow dependency order in pipelines | Types before components, components before tests |
| ALWAYS update memory.md after execution | Next session needs context |
| ALWAYS record lessons on errors | Prevents repeating mistakes |
| NEVER skip patterns.md | Contains project-specific code templates |
| NEVER mix agent responsibilities | Each agent has a clear scope boundary |
| NEVER use @fe-tester before feature is complete | Tests must test finished code |

## Error Handling

```
Agent produced bad output? →
  1. Check if knowledge base was loaded (patterns.md, lessons.md)
  2. If missing context → reload and re-execute
  3. If agent scope mismatch → switch to correct agent
  4. If complexity too high → orchestrator does it directly (no dispatch)
  5. Record lesson in agent's lessons.md
```
