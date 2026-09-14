# skill-collection

A personal collection of battle-tested agent skills (SKILL.md packages) for AI coding agents. Each skill here was distilled from real open-source work — not invented in a vacuum.

## Skills

Two lenses on the same discipline: *judgment before action*.

| Lens | Skill | Description |
|------|-------|-------------|
| What to build | [idea-to-spec](./skills/idea-to-spec/SKILL.md) | Turn a vague idea into a concrete, approved spec through structured dialogue — surface ambiguities, probe feasibility, write no code until approval. |
| Where to fix it | [progressive-abstraction](./skills/progressive-abstraction/SKILL.md) | Three-layer issue triage for unfamiliar codebases: judge the architecture-level fix direction first, trace data flow and boundary semantics second, propose bounded solutions last. Built for OSS contributions that must survive maintainer review. |

## Provenance

Every rule here has a scar behind it. Visible receipts:

- `progressive-abstraction` — distilled from [MoonshotAI/kimi-code#2175](https://github.com/MoonshotAI/kimi-code/pull/2175) and the triage of [#2118](https://github.com/MoonshotAI/kimi-code/issues/2118): three review rounds (two AI reviewers, one human-grade), every verdict decided on code evidence.

## Philosophy

- **Methodology over mechanics** — these skills encode *how to think* (judgment order, evidence discipline), not just what commands to run.
- **Every rule has a scar** — each instruction exists because a real mistake or a real win proved it. Anti-patterns are documented alongside the workflow.
- **Bounded on purpose** — each skill declares what it deliberately does not do.

## License

MIT
