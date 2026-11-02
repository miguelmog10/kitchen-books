<!--
From AI-Assisted Software Engineering templates v1.2.0 (CC0 1.0):
https://github.com/miguelmog10/ai-assisted-software-engineering/tree/v1.2.0/templates
-->

# 0003. Two indexes (complexity change, scope change), no single ratio

- **Status:** Accepted
- **Date:** 2026-10-28

## Context

The first account, Food Cost (complexity versus scope account), compares the complexity a change adds with the scope it delivers. The obvious summary is one number, complexity divided by scope. But scope can be zero (a refactor) or negative (a feature removed). Source: [02-kickoff-handoff.md](../../dev/discovery/02-kickoff-handoff.md), decisions 3 and 4; [01-concept-note.md](../../dev/discovery/01-concept-note.md), "Composition".

## Decision

Keep two indexes side by side, never divided into one:

- **Complexity change:** a weighted combination of complexity components.
- **Scope change:** a weighted combination of scope components.

Each is normalized against the repo's own history, with equal weights to start. The views plot one against the other.

## Consequences

- Refactors and removals stay readable: a ratio explodes as scope approaches zero, and a negative over a negative looks healthy for the wrong reason.
- There is no single headline number to game (Goodhart's law). Readers look at a point on a plane, or two lines over time.
- Weights can be tuned later against outcomes such as defects, as a new method version (ADR-0005).
