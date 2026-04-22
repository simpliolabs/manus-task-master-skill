---
name: task-master
description: >
  Task Master AI integration for structured task-driven project management on Manus.
  Use when: initializing projects, managing tasks, breaking down work, tracking progress,
  orchestrating development workflows, parsing PRDs, or when the user mentions
  "task master", "taskmaster", "TM", or asks to plan/manage/organize project tasks.
---

# Task Master — Manus Conductor Skill

Task Master (https://task-master.dev) is an AI-powered task management system. This skill makes Manus a **conductor** — orchestrating every project through structured task-driven workflows using the `task-master` CLI.

## Installation

```bash
npm install -g task-master-ai
```

Verify: `task-master --version`. Requires Node.js 18+.

## Project Initialization

1. **New project**: `task-master init` in the project root
2. **Create or obtain a PRD** at `.taskmaster/docs/prd.md` (use `.taskmaster/templates/example_prd.md` as reference)
3. **Parse PRD**: `task-master parse-prd .taskmaster/docs/prd.md`
4. **Analyze complexity**: `task-master analyze-complexity --research`
5. **Expand all tasks**: `task-master expand --all --research`

If no PRD exists, collaborate with the user to draft one before parsing. The more detailed the PRD, the better the generated tasks.

## The Conductor Loop

This is the core workflow to facilitate for every project session:

```
1. LIST    → task-master list                              # See all tasks
2. NEXT    → task-master next                              # Pick the next ready task
3. SHOW    → task-master show <id>                         # Read task details
4. EXPAND  → task-master expand --id=<id> --research       # Break into subtasks
5. IMPLEMENT → (write code, build features)                # Do the work
6. LOG     → task-master update-subtask --id=<id> --prompt="notes"  # Record progress
7. DONE    → task-master set-status --id=<id> --status=done         # Mark complete
8. REPEAT
```

Always operate on the current tag context (defaults to `master`).

## Core CLI Commands

| Action | Command | Notes |
|--------|---------|-------|
| Initialize | `task-master init` | Run once per project |
| Parse PRD | `task-master parse-prd <file>` | `--num-tasks=N`, `--force`, `--append` |
| List tasks | `task-master list` | `--status=<s>`, `--with-subtasks` |
| Next task | `task-master next` | Respects dependencies + priority |
| Show task | `task-master show <id>` | Supports `1.2` subtasks, `1,3,5` multi-ID |
| Add task | `task-master add-task --prompt="desc"` | `--priority=high`, `--dependencies=1,2` |
| Add subtask | `task-master add-subtask -p <parent> -t "title"` | Or convert: `--task-id=<id>` |
| Set status | `task-master set-status --id=<id> --status=done` | Batch: `--id=1,2,3` |
| Expand task | `task-master expand --id=<id>` | `--num=N`, `--force`, `--research` |
| Expand all | `task-master expand --all` | `--research`, `--force` |
| Update task | `task-master update-task --id=<id> --prompt="..."` | `--append`, `--research` |
| Update subtask | `task-master update-subtask --id=<id> --prompt="..."` | Appends with timestamp |
| Bulk update | `task-master update --from=<id> --prompt="..."` | Updates all from ID onward |
| Remove task | `task-master remove-task --id=<id>` | `--yes` to skip confirm |
| Move task | `task-master move --from=<id> --to=<id>` | Subtask↔standalone, reorder, batch |
| Add dependency | `task-master add-dependency --id=<id> --depends-on=<id>` | |
| Remove dependency | `task-master remove-dependency --id=<id> --depends-on=<id>` | |
| Validate deps | `task-master validate-dependencies` | |
| Fix deps | `task-master fix-dependencies` | Auto-fix invalid deps |
| Analyze complexity | `task-master analyze-complexity --research` | |
| View report | `task-master complexity-report` | |
| Research | `task-master research "query"` | `--id=`, `--files=`, `--tree`, `--save-to=` |
| Configure models | `task-master models` | `--set-main=`, `--set-research=`, `--setup` |
| Clear subtasks | `task-master clear-subtasks --id=<id>` | `--all` for all tasks |

**AI-powered commands** (`parse-prd`, `expand`, `add-task`, `update`, `update-task`, `update-subtask`, `analyze-complexity`, `research`) may take up to 60 seconds. Inform the user to wait.

## Task Statuses

`pending` → `in-progress` → `done` | `deferred` | `cancelled` | `blocked`

Setting a parent task to `done` automatically marks all subtasks as `done`.

## Implementation Drift

When the implementation diverges from the plan:

- **Single task**: `task-master update-task --id=<id> --prompt="Now using X instead of Y" --research`
- **Multiple future tasks**: `task-master update --from=<id> --prompt="Switched to MongoDB" --research`

## When to Introduce Tags

**Default stance**: Work in the `master` tag. Only introduce tags when a clear pattern emerges.

| Pattern | Trigger | Action |
|---------|---------|--------|
| Feature branch | User creates `git checkout -b feature/x` | `task-master add-tag --from-branch` |
| Team collaboration | User mentions teammates | `task-master add-tag my-work --copy-from-current` |
| Experiment | User wants to try risky changes | `task-master add-tag experiment-x` |
| Large feature | Major multi-step initiative | Create tag → draft PRD → `parse-prd --tag=feature-x` |
| Version-based | Prototype vs production maturity | Adjust task depth/complexity accordingly |

For tag management commands and PRD-driven workflows, see `references/advanced-workflows.md`.

## Research Command

Use `task-master research` to get fresh, up-to-date information beyond AI knowledge cutoffs:

```bash
task-master research "Best practices for JWT auth in Node.js"
task-master research "How to optimize this?" --id=15 --files=src/api.js --tree
task-master research "OAuth implementation" --save-to=15.3
```

## Project Structure

```
project/
├── .taskmaster/
│   ├── tasks/tasks.json       # Main task database (never edit manually)
│   ├── docs/prd.md            # Product requirements (.md recommended)
│   ├── reports/               # Complexity analysis reports
│   ├── templates/             # PRD templates
│   ├── config.json            # AI model config (use `task-master models`)
│   └── state.json             # Tag state (do not edit)
└── .env                       # API keys (CLI usage)
```

**Never manually edit** `tasks.json`, `config.json`, or `state.json`. Always use CLI commands.

## Configuration

At least one AI provider API key is required in `.env`:

```
ANTHROPIC_API_KEY=...
PERPLEXITY_API_KEY=...    # Recommended for research
OPENAI_API_KEY=...
GOOGLE_API_KEY=...
```

Configure models: `task-master models --setup` (interactive) or `task-master models --set-main=<model>`.

## References

- **Full MCP tool reference (42+ tools, 3 tiers)**: See `references/mcp-tools.md` — read when using Task Master via MCP server instead of CLI
- **Advanced workflows (tags, PRD-driven, autopilot)**: See `references/advanced-workflows.md` — read when projects need multi-context management or structured planning
- **Task structure and schema details**: See `references/task-structure.md` — read when needing to understand task JSON format, fields, or dependency rules
