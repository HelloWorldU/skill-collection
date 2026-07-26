# awesome-skills

A personal collection of battle-tested agent skills (SKILL.md packages) for AI coding agents. Each skill here was distilled from real open-source work — not invented in a vacuum.

## Skills

The collection forms a pipeline that mirrors how real work actually flows — from a vague idea to a merged contribution:

| Stage | Skill | Description |
|-------|-------|-------------|
| What to build | [idea-to-spec](./idea-to-spec/SKILL.md) | Turn a vague idea into a concrete, approved spec through structured dialogue — surface ambiguities, probe feasibility, write no code until approval. |
| How to build it | [task-intake](./task-intake/SKILL.md) | Mandatory alignment gate before modifying code/files/config: restate requirements, enumerate scenarios with expected outcomes, stop for explicit approval, then implement under strict invariants. |
| Where to fix it | [progressive-abstraction](./progressive-abstraction/SKILL.md) | Three-layer issue triage for unfamiliar codebases: judge the architecture-level fix direction first, trace data flow and boundary semantics second, propose bounded solutions last. Built for OSS contributions that must survive maintainer review. |

## What is a skill?

A skill is a `SKILL.md` file (YAML frontmatter + Markdown instructions) that gives an AI agent procedural knowledge for a specific kind of task — when to trigger, what steps to follow, what evidence standards to hold. Drop a skill folder into your agent's skills directory (e.g. `.agents/skills/`, `~/.config/agents/skills/`) and the agent picks it up automatically.

## Philosophy

- **Methodology over mechanics** — these skills encode *how to think* (judgment order, evidence discipline), not just what commands to run.
- **Every rule has a scar** — each instruction exists because a real mistake or a real win proved it. Anti-patterns are documented alongside the workflow.
- **Bounded on purpose** — each skill declares what it deliberately does not do.

## License

MIT
