# Advanced Workflows

## Table of Contents

1. [Tag Management](#tag-management)
2. [PRD-Driven Feature Development](#prd-driven-feature-development)
3. [Existing Codebase Onboarding](#existing-codebase-onboarding)
4. [Version-Based Development](#version-based-development)
5. [Autopilot TDD Workflow](#autopilot-tdd-workflow)
6. [Git Integration Patterns](#git-integration-patterns)
7. [Master Tag Curation](#master-tag-curation)

## Tag Management

Tags provide isolated task contexts. All tag commands:

```bash
task-master tags                                    # List all tags
task-master tags --show-metadata                    # List with metadata
task-master add-tag <name>                          # Create empty tag
task-master add-tag <name> --description="..."      # With description
task-master add-tag --from-branch                   # Name from current git branch
task-master add-tag <name> --copy-from-current      # Copy tasks from active tag
task-master add-tag <name> --copy-from=<source>     # Copy from specific tag
task-master use-tag <name>                          # Switch active context
task-master rename-tag <old> <new>                  # Rename tag
task-master copy-tag <source> <target>              # Duplicate tag
task-master delete-tag <name>                       # Delete (with confirmation)
task-master delete-tag <name> --yes                 # Delete without confirmation
```

Most task commands accept `--tag=<name>` to operate on a specific context without switching.

### Decision Patterns for Introducing Tags

**Pattern 1 — Feature Branch**: User creates a git branch. Suggest `add-tag --from-branch` to mirror the branch and isolate feature tasks.

**Pattern 2 — Team Collaboration**: User mentions teammates. Create a personal tag (`add-tag my-work --copy-from-current`) to avoid conflicts on the shared master list.

**Pattern 3 — Experiment**: User wants to try risky changes. Create a sandboxed tag (`add-tag experiment-zustand`). If abandoned, simply delete the tag.

**Pattern 4 — Large Feature (PRD-Driven)**: Major initiative requiring formal planning. See PRD-Driven Feature Development below.

**Pattern 5 — Version-Based**: Prototype/MVP tags get simpler, faster tasks. Production tags get comprehensive error handling, testing, and documentation.

## PRD-Driven Feature Development

For significant new features that warrant dedicated planning:

### Step-by-Step

1. **Create tag**: `task-master add-tag feature-xyz --description="XYZ feature tasks"`
2. **Draft PRD**: Collaborate with user to create `.taskmaster/docs/feature-xyz-prd.md`
3. **Parse into tag**: `task-master parse-prd .taskmaster/docs/feature-xyz-prd.md --tag=feature-xyz`
4. **Analyze**: `task-master analyze-complexity --tag=feature-xyz --research`
5. **Expand**: `task-master expand --all --tag=feature-xyz --research`
6. **Add master reference**: Create a high-level task in `master` referencing the feature tag

### PRD Best Practices

- Use `.md` extension for better editor support and rendering
- The more detailed the PRD, the better the generated tasks
- Use `--num-tasks=0` to let Task Master determine task count from complexity
- Use `--append` flag to parse additional PRDs into an existing tag
- Keep PRDs focused; parse multiple smaller PRDs rather than one massive document
- Use the example PRD at `.taskmaster/templates/example_prd.md` as a starting template

### Suggested PRD Prompt

When helping users draft a PRD:

> "Let's create a detailed PRD for this feature. I'll need: (1) the core problem being solved, (2) specific user stories, (3) technical requirements and constraints, (4) preferred tech stack/libraries, (5) database schema if applicable, (6) API endpoints needed, and (7) acceptance criteria. The more specific you are, the better the generated tasks will be."

## Existing Codebase Onboarding

When initializing Task Master on an existing project:

1. **Initialize**: `task-master init` in the project root
2. **Research codebase**: `task-master research "Current architecture and improvement opportunities" --tree --files=src/`
3. **Draft strategic PRD**: Based on research findings, co-author a PRD covering current state, proposed improvements, and implementation strategy
4. **Create tags per initiative**: Parse PRDs into appropriate tags (`refactor-api`, `feature-dashboard`, `tech-debt`)
5. **Curate master**: Keep only high-level deliverables in master

## Version-Based Development

Adjust task generation strategy based on project maturity:

**Prototype/MVP** (`prototype`, `mvp`, `poc`, `v0.x`):
- Focus on speed and basic functionality
- Fewer subtasks, more direct implementation paths
- Add context: `--prompt="This is a prototype - prioritize speed over optimization"`

**Production** (`v1.0+`, `production`, `stable`):
- Emphasize robustness, testing, and maintainability
- More detailed subtasks with error handling and documentation
- Add context: `--prompt="Production-grade - include error handling, testing, and docs"`

## Autopilot TDD Workflow

Autopilot automates the test-driven development cycle across subtasks. Available via MCP `all` tier:

```
autopilot_start   → Begin automated TDD loop on a task's subtasks
autopilot_next    → Advance to the next subtask
autopilot_complete → Mark current subtask done
autopilot_commit  → Commit current work
autopilot_finalize → Complete the autopilot session
autopilot_status  → Check current state
autopilot_resume  → Resume a paused session
autopilot_abort   → Cancel the session
```

The autopilot follows a Red → Green → Commit cycle per subtask, with automatic branch management and PR creation.

## Git Integration Patterns

### Branch-Tag Mirroring

```bash
git checkout -b feature/user-auth
task-master add-tag --from-branch        # Creates "feature-user-auth" tag
# Work within the tag...
task-master use-tag feature-user-auth
task-master list
```

### Commit Conventions

Recommended commit message format tied to tasks:

```
feat(task-4): Implement user authentication
fix(task-4.2): Fix JWT token expiration handling
```

### Reducing Merge Conflicts

Tags isolate `tasks.json` changes per context. When teams work in separate tags, their task files don't conflict. Use `move` to resolve any conflicts when merging back:

```bash
task-master move --from=10,11,12 --to=16,17,18
```

## Master Tag Curation

Once using tags, the `master` tag should contain only:

**Include in master:**
- High-level deliverables with significant business value
- Major milestones and epic-level features
- Critical infrastructure work affecting the entire project
- Release-blocking items

**Keep out of master:**
- Detailed implementation subtasks (belong in feature tags)
- Refactoring work (use `refactor-*` tags)
- Experimental features (use `experiment-*` tags)
- Team member-specific tasks (use person-specific tags)
