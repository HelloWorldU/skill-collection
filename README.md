# awesome-skills

A personal collection of battle-tested [agent skills](https://agentskills.io) (SKILL.md packages) for AI coding agents. Each skill here was distilled from real open-source work — not invented in a vacuum.

## Skills

| Skill | Description |
|-------|-------------|
| [progressive-abstraction](./progressive-abstraction/SKILL.md) | Three-layer issue triage for unfamiliar codebases: judge the architecture-level fix direction first, trace data flow and boundary semantics second, propose bounded solutions last. Built for OSS contributions that must survive maintainer review. |

## What is a skill?

A skill is a `SKILL.md` file (YAML frontmatter + Markdown instructions) that gives an AI agent procedural knowledge for a specific kind of task — when to trigger, what steps to follow, what evidence standards to hold. Drop a skill folder into your agent's skills directory (e.g. `.agents/skills/`, `~/.config/agents/skills/`) and the agent picks it up automatically.

## Philosophy

- **Methodology over mechanics** — these skills encode *how to think* (judgment order, evidence discipline), not just what commands to run.
- **Every rule has a scar** — each instruction exists because a real mistake or a real win proved it. Anti-patterns are documented alongside the workflow.
- **Bounded on purpose** — each skill declares what it deliberately does not do.

## License

MIT
