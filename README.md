<div align="center">

# Manus Task Master Skill

**A conductor skill for [Manus](https://manus.im) built on [Task Master AI](https://task-master.dev)**

*Created by [SimplioLabs](https://github.com/simpliolabs) — Nir Appelton*

[![Fork of](https://img.shields.io/badge/fork%20of-claude--task--master-blue)](https://github.com/eyaltoledano/claude-task-master)
[![Task Master](https://img.shields.io/badge/task--master--ai-v0.17+-green)](https://task-master.dev)
[![Manus Skill](https://img.shields.io/badge/manus-skill-purple)](https://manus.im)

</div>

---

## What Is This?

This is a **fork** of [eyaltoledano/claude-task-master](https://github.com/eyaltoledano/claude-task-master) with a custom **Manus conductor skill** layer added by SimplioLabs. It transforms [Manus](https://manus.im) into an autonomous project conductor that orchestrates every task and project through Task Master's structured, AI-powered workflow.

> **Task Master** is the engine. **This skill** is the conductor that teaches Manus how to drive it.

## How It Works on Manus

When this skill is activated, Manus automatically:

1. **Initializes projects** — Installs Task Master, runs `task-master init`, helps draft or parse a PRD, analyzes complexity, and expands tasks into actionable subtasks
2. **Runs the Conductor Loop** every session — `list → next → show → expand → implement → log → done → repeat`
3. **Handles implementation drift** — When plans change, bulk-updates all future tasks to stay aligned
4. **Scales intelligently** — Introduces tags, PRD-driven workflows, and multi-context management only when project complexity demands it
5. **Leverages AI research** — Uses Task Master's research command for real-time information beyond knowledge cutoffs

### The Conductor Loop

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

### Skill Trigger

The skill activates automatically when you mention **"task master"**, **"taskmaster"**, **"TM"**, or ask Manus to **plan, manage, or organize** project tasks.

## Skill Files

All Manus skill files live on the [`manus-skill`](https://github.com/simpliolabs/manus-task-master-skill/tree/manus-skill) branch:

```
skills/task-master/
├── SKILL.md                              # Core conductor workflow (150 lines)
├── README.md                             # Skill documentation
└── references/
    ├── mcp-tools.md                      # Full MCP tool reference (42+ tools, 3 tiers)
    ├── advanced-workflows.md             # Tags, PRD-driven dev, autopilot, git patterns
    └── task-structure.md                 # Task JSON schema, statuses, dependencies, config
```

| File | Lines | What It Covers |
|------|-------|----------------|
| `SKILL.md` | 150 | Core conductor loop, 25+ CLI commands, installation, configuration, tag decision patterns |
| `references/mcp-tools.md` | 154 | Complete MCP tool inventory — core (7), standard (14), all (42+) tiers with parameters |
| `references/advanced-workflows.md` | 158 | Tagged task lists, PRD-driven feature development, autopilot TDD, git integration patterns |
| `references/task-structure.md` | 166 | Task JSON schema, all 6 status values, dependency management, config file formats |

## Install This Skill on Manus

### Option 1: Import from GitHub (Recommended)

1. Open [Manus](https://manus.im) and go to **Settings**
2. Click **Skills** → **+ Add** → **Import from GitHub**
3. Paste this repository link:

```
https://github.com/simpliolabs/manus-task-master-skill
```

4. Done — the skill is now active and will trigger automatically on your next project

### Option 2: Upload Manually

1. Download the skill files from the [`manus-skill`](https://github.com/simpliolabs/manus-task-master-skill/tree/manus-skill) branch (`skills/task-master/` directory)
2. In Manus, go to **Skills** → **+ Add** → **Upload a skill**
3. Upload the `.zip` or `.skill` file

> **Zero setup required.** Manus comes with Node.js and OpenAI pre-configured. The skill automatically installs Task Master (`npm install -g task-master-ai`) on first use. Just import and go.

## Key Capabilities

| Capability | Description |
|------------|-------------|
| **PRD Parsing** | Parse product requirement documents into structured, prioritized tasks |
| **Complexity Analysis** | AI-powered analysis with recommended subtask counts per task |
| **Task Expansion** | Break high-level tasks into detailed, implementable subtasks |
| **Dependency Management** | Automatic dependency tracking, validation, and circular-dependency prevention |
| **Implementation Drift** | Bulk-update future tasks when architecture or plans change |
| **Tagged Contexts** | Isolated task lists for features, experiments, team members |
| **AI Research** | Real-time research with project context for up-to-date information |
| **Autopilot TDD** | Automated test-driven development cycle across subtasks |
| **42+ MCP Tools** | Full programmatic access across 3 tiers (core/standard/all) |

## Branch Structure

| Branch | Purpose |
|--------|---------|
| `main` | Full upstream Task Master codebase (syncs with [eyaltoledano/claude-task-master](https://github.com/eyaltoledano/claude-task-master)) |
| `manus-skill` | Manus conductor skill layer by SimplioLabs |

## Credits

| Component | Author |
|-----------|--------|
| **Task Master AI** | [Eyal Toledano](https://github.com/eyaltoledano), Ralph Khreish |
| **Manus Conductor Skill** | [SimplioLabs](https://github.com/simpliolabs) — Nir Appelton |
| **Built with** | [Manus](https://manus.im) Skill Creator workflow |

## License

Task Master is licensed under **MIT + Commons Clause** by Eyal Toledano and Ralph Khreish. See [LICENSE](LICENSE) for full terms.

The Manus skill integration layer (`skills/task-master/`) is created by SimplioLabs (Nir Appelton) and is available under the same license terms.
