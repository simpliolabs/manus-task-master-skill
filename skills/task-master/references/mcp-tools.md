# Task Master MCP Tool Reference

## Table of Contents

1. [Tool Tiers](#tool-tiers)
2. [MCP Server Setup](#mcp-server-setup)
3. [Core Tools (7)](#core-tools)
4. [Standard Tools (14)](#standard-tools)
5. [All Tools (42+)](#all-tools)

## Tool Tiers

Task Master uses tiered tool loading to optimize context window usage:

| Tier | Count | Tokens | Use Case |
|------|-------|--------|----------|
| `core` | 7 | ~5,000 | Daily development, minimal overhead (default) |
| `standard` | 14 | ~10,000 | Regular task management workflows |
| `all` | 42+ | ~21,000 | Full suite: research, autopilot, dependencies, tags |

Set via `TASK_MASTER_TOOLS` env var in MCP config. Default is `core`.

## MCP Server Setup

Add to your MCP configuration (e.g., `.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "task-master-ai": {
      "command": "npx",
      "args": ["-y", "task-master-ai"],
      "env": {
        "TASK_MASTER_TOOLS": "all",
        "ANTHROPIC_API_KEY": "your-key",
        "PERPLEXITY_API_KEY": "your-key",
        "OPENAI_API_KEY": "your-key"
      }
    }
  }
}
```

For long-running AI operations, set `timeout: 300` in your MCP client config.

## Core Tools

These 7 tools cover the daily development loop:

| MCP Tool | CLI Equivalent | Description |
|----------|---------------|-------------|
| `get_tasks` | `task-master list` | List tasks, filter by status, include subtasks |
| `next_task` | `task-master next` | Get next available task based on deps/priority |
| `get_task` | `task-master show <id>` | Show task details; supports comma-separated IDs |
| `set_task_status` | `task-master set-status` | Set status for tasks/subtasks; batch supported |
| `update_subtask` | `task-master update-subtask` | Append timestamped notes to subtask details |
| `parse_prd` | `task-master parse-prd` | Parse PRD to generate tasks (AI-powered) |
| `expand_task` | `task-master expand --id=<id>` | Break task into subtasks (AI-powered) |

### Key Parameters

**get_tasks**: `status` (filter), `withSubtasks` (bool), `tag` (context)

**get_task**: `id` (required, e.g. `"15"`, `"15.2"`, `"1,5,10.2"`), `tag`

**set_task_status**: `id` (required, comma-separated), `status` (required: pending/in-progress/done/deferred/cancelled/blocked), `tag`

**update_subtask**: `id` (required, e.g. `"5.2"`), `prompt` (required), `research` (bool), `tag`

**parse_prd**: `input` (file path), `numTasks` (number), `force` (bool), `tag`

**expand_task**: `id`, `num` (subtask count), `research` (bool), `prompt` (extra context), `force` (replace existing), `tag`

## Standard Tools

Adds 7 more tools to core:

| MCP Tool | CLI Equivalent | Description |
|----------|---------------|-------------|
| `initialize_project` | `task-master init` | Set up project structure and config |
| `analyze_project_complexity` | `task-master analyze-complexity` | AI complexity analysis of all tasks |
| `expand_all` | `task-master expand --all` | Expand all eligible pending tasks |
| `add_subtask` | `task-master add-subtask` | Add subtask or convert existing task |
| `remove_task` | `task-master remove-task` | Permanently delete task/subtask |
| `add_task` | `task-master add-task` | AI-generated new task from prompt |
| `complexity_report` | `task-master complexity-report` | View formatted complexity analysis |

## All Tools

The full tier adds these additional tools:

### Task Management
| MCP Tool | Description |
|----------|-------------|
| `update` | Bulk update future tasks from a starting ID |
| `update_task` | Update a single task with AI |
| `remove_subtask` | Remove a specific subtask |
| `clear_subtasks` | Clear all subtasks from a task |
| `move_task` | Move tasks/subtasks within hierarchy |
| `scope_up_task` | Promote subtask to parent level |
| `scope_down_task` | Demote task to subtask |

### Dependencies
| MCP Tool | Description |
|----------|-------------|
| `add_dependency` | Add dependency between tasks |
| `remove_dependency` | Remove a dependency |
| `validate_dependencies` | Check for invalid dependencies |
| `fix_dependencies` | Auto-fix dependency issues |

### Tag Management
| MCP Tool | Description |
|----------|-------------|
| `list_tags` | List all tags with task counts |
| `add_tag` | Create new tag (empty, from branch, or copy) |
| `use_tag` | Switch active tag context |
| `rename_tag` | Rename an existing tag |
| `copy_tag` | Duplicate a tag and its tasks |
| `delete_tag` | Delete a tag and all its tasks |

### Research & Configuration
| MCP Tool | Description |
|----------|-------------|
| `research` | AI-powered research with project context |
| `models` | View/set AI model configuration |
| `rules` | Add/remove rule profiles |
| `response-language` | Set response language |

### Autopilot (TDD Workflow)
| MCP Tool | Description |
|----------|-------------|
| `autopilot_start` | Begin automated TDD subtask loop |
| `autopilot_resume` | Resume paused autopilot session |
| `autopilot_next` | Advance to next subtask in autopilot |
| `autopilot_status` | Check current autopilot state |
| `autopilot_complete` | Mark current autopilot subtask done |
| `autopilot_commit` | Commit current autopilot work |
| `autopilot_finalize` | Finalize autopilot session |
| `autopilot_abort` | Abort autopilot session |

### Generation
| MCP Tool | Description |
|----------|-------------|
| `generate` | Generate task files from tasks.json |

## Custom Tool Lists

Specify exact tools via comma-separated list:

```json
"TASK_MASTER_TOOLS": "get_tasks,next_task,set_task_status,expand_task,research"
```

Tool names are case-insensitive. Hyphens and underscores are interchangeable. Invalid names are silently skipped (with warning).
