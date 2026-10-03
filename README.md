# skill-collection

A personal collection of battle-tested agent skills (SKILL.md packages) for AI coding agents. Each skill here was distilled from real open-source work — not invented in a vacuum.

## Skills

Two lenses on the same discipline: *judgment before action*.

| Lens | Skill | Description |
|------|-------|-------------|
| What to build | [idea-to-spec](./skills/idea-to-spec/SKILL.md) | Turn a vague idea into a concrete, approved spec through structured dialogue — surface ambiguities, probe feasibility, write no code until approval. |
| How to fix it | [excellent-engineer](./skills/excellent-engineer/SKILL.md) | A gate for engineering tasks: assess how well the user knows the domain, then route to a workflow. Currently fixes a GitHub issue one step per turn, exposing the level of detail that fits the user's expertise. |

## Philosophy

- **Methodology over mechanics** — these skills encode *how to think* (judgment order, evidence discipline), not just what commands to run.
- **Every rule has a scar** — each instruction exists because a real mistake or a real win proved it. Anti-patterns are documented alongside the workflow.
- **Bounded on purpose** — each skill declares what it deliberately does not do.

## License

MIT
