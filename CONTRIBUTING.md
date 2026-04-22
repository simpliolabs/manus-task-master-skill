# Contributing to Manus Task Master Skill

Thank you for your interest in contributing! This repo is a **fork** of [eyaltoledano/claude-task-master](https://github.com/eyaltoledano/claude-task-master) maintained by **SimplioLabs (Nir Appelton)** with a custom Manus conductor skill layer.

## Repository Structure

| Branch | Purpose | Contributions |
|--------|---------|---------------|
| `main` | Upstream Task Master codebase | Synced from [eyaltoledano/claude-task-master](https://github.com/eyaltoledano/claude-task-master) — submit upstream PRs there |
| `manus-skill` | Manus conductor skill layer | Submit PRs here for skill improvements |

## Contributing to the Manus Skill

The Manus skill files live on the `manus-skill` branch under `skills/task-master/`.

### Getting Started

```bash
git clone https://github.com/simpliolabs/manus-task-master-skill.git
cd manus-task-master-skill
git checkout manus-skill
```

### What You Can Contribute

- **Skill improvements** — Better conductor logic, new workflow patterns, refined CLI command coverage
- **Reference updates** — New MCP tools, updated advanced workflows, schema changes
- **Bug fixes** — Incorrect commands, missing parameters, outdated information
- **New references** — Additional workflow patterns, integration guides, best practices

### Submitting a PR

1. Fork this repo
2. Create a branch from `manus-skill`: `git checkout -b improve/your-change`
3. Make your changes in `skills/task-master/`
4. Test the skill by installing it in Manus and running real tasks
5. Submit a PR targeting the `manus-skill` branch

### Skill File Guidelines

- **`SKILL.md`** must stay under 500 lines (Manus context window constraint)
- Use **imperative/infinitive form** in instructions
- Move detailed content to `references/` files, not into SKILL.md
- Keep reference files under 200 lines each with a table of contents
- Test all CLI commands before documenting them

## Contributing to Task Master Core

For changes to the Task Master engine itself (CLI, MCP server, AI providers, core logic), submit PRs directly to the upstream repo:

**[eyaltoledano/claude-task-master](https://github.com/eyaltoledano/claude-task-master)**

Follow their [contribution guidelines](https://github.com/eyaltoledano/claude-task-master/blob/main/CONTRIBUTING.md).

## Getting Help

- **Manus Skill issues** — [Open an issue](https://github.com/simpliolabs/manus-task-master-skill/issues) on this repo
- **Task Master core issues** — [Open an issue](https://github.com/eyaltoledano/claude-task-master/issues) on the upstream repo
- **Task Master community** — [Discord](https://discord.gg/taskmasterai)

## License

By contributing to the Manus skill layer, you agree that your contributions will be licensed under the same terms as the project (MIT with Commons Clause). See [LICENSE](LICENSE) for details.

---

**Maintained by [SimplioLabs](https://github.com/simpliolabs) — Nir Appelton**
