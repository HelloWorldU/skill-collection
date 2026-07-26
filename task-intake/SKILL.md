---
name: task-intake
description: Mandatory alignment gate before modifying code, files, or configuration — restate the requirement, surface assumptions, enumerate scenarios with expected outcomes, then STOP and wait for explicit user approval before writing any implementation code. Use at the start of any task that involves changing code, files, or configuration in a repository, especially multi-file or multi-step changes. Skip the gate only for pure Q&A, pure explanation, or tiny changes where the user explicitly said "just do it" — the implementation invariants still always apply. For vague greenfield ideas that are not yet concrete tasks, use idea-to-spec first; for triaging issues in unfamiliar codebases, use progressive-abstraction.
---

# Task Intake: Alignment Before Implementation

## Goals

- Align on requirements, assumptions, scenarios, and expected outcomes *before* acting.
- Obtain explicit user approval through an alignment gate.
- Ensure implementation starts with clear acceptance criteria.

## Constraints

- **Without explicit user approval, write no implementation code.** Stages 1-3 produce text, written in the reply body.
- **Ask and align in prose, not through tool calls.** Put questions and confirmations directly in the reply; the user answers next turn.

## Inputs

Any request involving modification of code / files / configuration.

Pure Q&A, pure explanation, or tiny changes the user explicitly fast-tracked → stages 1-3 may be skipped; **the Stage 4 invariants always apply**.

## Steps

### Pre-check: Branch Sync

Before anything else, check whether the current branch is behind the remote main. Work on the latest base — discovering conflicts at push time is too late.

### Stage 1 — Requirement Clarification + Reading List

1. **Restate the requirement** in your own words and list your **assumptions**. Ask immediately about any ambiguity; never decide on the user's behalf.
2. List the **documents and files to read**: the nearest `AGENTS.md`, relevant docs, existing skills, and every source file that will be touched.
3. Read them before advancing to Stage 2.

### Stage 2 — Scenarios & Expected Outcomes

Break the task into **concrete scenarios**, each with an **expected outcome**. Must cover:

- ✅ **Happy path**
- ⚠️ **Failure / edge cases** — malformed input, empty values, network failure, concurrency, oversized input, multibyte characters
- 👤 **User perspective** — how the user operates and what they see; for frontend work, treat interactions (keyboard, line wrapping, overflow) as scenarios

> Happy path only = Stage 2 not complete.

### Stage 3 — Alignment Gate (GATE)

Write the Stage 1 + 2 output (requirement restatement, assumptions, reading list, scenarios with expectations, decision points) in the reply body, then **STOP**. Wait for the user's next message.

- No explicit "go / approved / start" → do not enter Stage 4.
- User requests changes → return to Stage 1/2, revise, and align **again**.

### Stage 4 — Implementation & Verification

**Invariants** (violations are detectable on the spot):

- **Real implementation, no mocks** — if it can't be done, stop and say so.
- **Do only what was asked** — no self-added truncation, caps, pagination, guardrails, or retries.
- **Don't touch what you didn't add** — if something looks useless, grep for references and show the user before deleting.
- **Context completeness** — where conversation/history is involved, preserve the full causal chain; never drop earlier information.
- **Return to alignment at major decision points** — if implementation surfaces ambiguity, conflicts, or uncovered boundaries not settled in Stages 1-3, **pause**, re-clarify in the reply body, and resume only after explicit approval.

**Verification**: run checks **covering this change** (test the packages you touched); paste raw output, not just "passed"; show the evidence — the pre-delete grep and the post-change checks alike.

## Output

An alignment-gated implementation plan plus verified code changes.
