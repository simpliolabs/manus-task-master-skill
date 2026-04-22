# Manus Task Master Skill

A **conductor skill** for [Manus](https://manus.im) that integrates [Task Master AI](https://task-master.dev) to orchestrate structured, task-driven project management.

## What It Does

When activated, this skill transforms Manus into a project conductor that:

- **Initializes projects** with Task Master's structured workflow (PRD parsing, complexity analysis, task expansion)
- **Runs the Conductor Loop** every session: list → next → show → expand → implement → log → done → repeat
- **Handles implementation drift** by bulk-updating future tasks when plans change
- **Scales intelligently** — introduces tags, PRD-driven workflows, and multi-context management only when project complexity demands it

## Skill Structure

```
manus-task-master-skill/
├── SKILL.md                              # Core conductor workflow (150 lines)
└── references/
    ├── mcp-tools.md                      # Full MCP tool reference (42+ tools, 3 tiers)
    ├── advanced-workflows.md             # Tags, PRD-driven dev, autopilot, git patterns
    └── task-structure.md                 # Task JSON schema, statuses, dependencies, config
```

| File | Purpose |
|------|---------|
| `SKILL.md` | Core conductor loop, CLI command reference, installation, configuration, tag decision patterns |
| `references/mcp-tools.md` | Complete MCP tool inventory across core (7), standard (14), and all (42+) tiers |
| `references/advanced-workflows.md` | Tagged task lists, PRD-driven feature development, autopilot TDD, git integration |
| `references/task-structure.md` | Task JSON schema, status values, dependency management, config files |

## How to Use

### As a Manus Skill

Install the skill in Manus by adding it to your skills directory:

```
/home/ubuntu/skills/task-master/
```

The skill triggers automatically when you mention "task master", "taskmaster", "TM", or ask Manus to plan, manage, or organize project tasks.

### Prerequisites

- **Node.js 18+**
- **At least one AI provider API key** (Anthropic, OpenAI, Google, Perplexity, etc.)

Task Master is installed via:

```bash
npm install -g task-master-ai
```

## The Conductor Loop

```
1. LIST    → task-master list                              # See all tasks
2. NEXT    → task-master next                              # Pick the next ready task
3. SHOW    → task-master show <id>                         # Read task details
4. EXPAND  → task-master expand --id=<id> --research       # Break into subtasks
5. IMPLEMENT → (write code, build features)                # Do the work
6. LOG     → task-master update-subtask --id=<id> --prompt="notes"
7. DONE    → task-master set-status --id=<id> --status=done
8. REPEAT
```

## Built With

- [Task Master AI](https://task-master.dev) (source: [eyaltoledano/claude-task-master](https://github.com/eyaltoledano/claude-task-master))
- [Manus Skill Creator](https://manus.im) workflow

## License

MIT
