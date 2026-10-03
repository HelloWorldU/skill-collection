---
name: excellent-engineer
description: Use when facing an engineering task. First assesses how well the user knows the task's domain, then routes to the matching workflow. Currently supports fixing a GitHub issue.
---

# Excellent Engineer

Engineering tasks call for high reasoning effort.

## Assess the user's expertise

Search local memory, AGENTS.md, and similar documents, and consider how the user phrases the request, to judge the user's experience and knowledge in the problem's domain, on a range from newcomer to expert. With no evidence, treat the user as a newcomer.

## Route

- Fixing a GitHub issue: read `learning-by-doing/guide.md`
