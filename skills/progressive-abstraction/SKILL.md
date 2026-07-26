---
name: progressive-abstraction
description: Three-layer issue triage methodology for unfamiliar codebases — never dive into implementation details before judging the fix at the architecture layer. Layer 1 identifies the owning layer and classifies the problem (correctness vs completeness vs product decision), Layer 2 traces the real data flow with verified file:line evidence and surfaces boundary semantics, Layer 3 proposes bounded solutions that declare what they deliberately do not do. Use when analyzing or triaging an issue/bug in an unfamiliar repository, deciding where a fix belongs before writing code, preparing an OSS contribution (issue comment / PR) that must survive maintainer review, or evaluating third-party and AI-generated review opinions against code evidence. For vague greenfield ideas use idea-to-spec first; for concrete modification tasks in a familiar repo use task-intake instead.
---

# Progressive Abstraction: Three-Layer Issue Triage

**Core principle: never enter implementation details before the fix direction is judged at the architecture layer.** The first reaction to an issue is not reading code — it is asking "which layer owns this problem?"

Execute the three layers in order. Each has a concrete output; do not advance while the current layer is unresolved.

## L1: Architecture Direction

Goal: identify the **owning layer** and classify the problem.

1. Read the full issue and the entire comment thread. Record: who reported it, who already diagnosed, whether maintainers are involved, whether the lane is occupied.
2. List the candidate layers of the system (e.g. provider adapter / transport / parse / schema / validation / execution / projection) and pick the one that *should* own the fix, with a one-line justification. Also state where the fix should **not** land — bracketed exclusion ("at the parse boundary, not in the schema") carries more information than a positive description.
3. Classify the problem: **correctness, completeness, or product decision?** The test is whether wrong behavior can actually occur.
   - Wrong behavior can occur → correctness. Fix it directly; ask no one.
   - Only "coverage gaps / doesn't help some cases" → completeness. Do not let anyone (including AI reviewers) reframe it as "incorrect".
   - Whether to fix depends on project stance (e.g. "should we absorb third-party quirks?") → product decision. **Hand it to the maintainers explicitly** in a comment; never make the choice silently for the project.
4. Output: one line fix direction + one line problem classification + one line lane judgment.

## L2: Data Flow & Boundary Semantics

Goal: verify L1 against **real code** and surface boundary semantics that cannot be ignored.

1. Trace the complete data flow from input to failure point, annotating every hop with `file:line`. **Verify every line number by hand** (against current main / the exact version); treat line numbers from anyone (including AI) as unverified hypotheses.
2. Draw the flow: input → processing nodes (file:line) → failure point. The diagram is L2's core deliverable, not decoration.
3. Identify boundary semantics — hidden facts that flip the solution if missed. Common types:
   - **Dual identity**: one artifact means different things to two readers (e.g. a schema advertises to the model *and* enforces on input);
   - **Feedback loops**: the reader of an error message is the model, and it shapes the next turn;
   - **Safety direction**: which side failures lean toward (e.g. "missing a coercion is safe, a wrong one is the accident") — this is what justifies conservative strategies;
   - **Local precedent**: existing handling of the same class in the same file/module (new code should look like the file it lives in).
4. Output: the data-flow diagram + a boundary-semantics list (one line each, with file:line evidence).

## L3: Bounded Solutions

Goal: produce **bounded** options, each declaring what it does *not* do.

1. Present 2-4 options with different landing spots (A/B/C/D). For each: which layer, what it changes, **what it deliberately leaves alone**.
2. Prefer **combinations** (e.g. "normalization + visible warning"): one move restores function, another preserves observability — a silent fix alone is usually half a solution.
3. Boundedness *is* the design. Declare out-of-scope items explicitly ("no per-model special cases", "no `$ref` resolution") and why widening is negative value ("generalization ends in rebuilding a wheel the project already has").
4. Tests as contract: pin **intentional behavior** with tests — including deliberate under-handling — so reviewers see the boundary as a choice, not an oversight.
5. Output: option comparison + recommended combination + scope statement + test strategy.

## Evidence Discipline (applies to all layers)

- **Lock the object**: confirm the exact subject first (model, version, file), then research. If the object is unverifiable (closed source, no public artifact), degrade to the nearest verifiable evidence and mark the boundary in your wording — never extrapolate.
- **First-hand verification**: every key claim needs the original artifact (fetch and read the source file), not secondhand summaries — including other AIs' conclusions, abstracts, or quoted "official behavior".
- **Every sentence defensible**: for anything written into a report, comment, or PR body, ask "if a maintainer challenges this line, what do I show?" If nothing — delete it, or downgrade it to a labeled speculation.
- **Judging third-party opinions** (including AI reviewers): verdict item by item, on code evidence; fix what is true, rebut what is false using the repo's own precedents; when someone claims "correctness", rerun the L1.3 classification test first.

## Execution Etiquette (after the solution is set)

- **Comment before PR**: add increment, never repeat a prior diagnosis (reference it and advance); qualify directional words ("on the harness side of the model boundary", never a bare "downstream"); end by claiming the work ("I can implement this") — proposal + claim = PR, proposal + silence = air.
- **A PR is a proposal, not a decision**: authority stays with maintainers; a shorter body is a safer body. Keep evidence in reserve as comment ammunition — fire it when asked, do not front-load.
- **Response discipline**: answer review feedback within 20 minutes to 2 hours; handle each point publicly and explicitly.

## Anti-Patterns (each one a real scar)

- Writing code the moment an issue arrives;
- Reading large files top to bottom (read along a variable's lifecycle or a single data flow instead);
- Using AI-provided line numbers or classifications as fact;
- Widening scope for "generality" until a helper becomes half a standard library;
- Asserting unverifiable claims in a PR body;
- Restating an existing diagnosis in a comment without increment or a work claim.
