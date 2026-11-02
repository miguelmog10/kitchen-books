<!--
From AI-Assisted Software Engineering templates v1.2.0 (CC0 1.0):
https://github.com/miguelmog10/ai-assisted-software-engineering/tree/v1.2.0/templates
-->

# 0005. Snapshots store raw facts under a versioned JSON schema; indexes come from a versioned method and never rewrite a snapshot

- **Status:** Accepted
- **Date:** 2026-10-30

## Context

The kickoff made JSON snapshots under a versioned schema the single source of truth, with dashboards, pull request reports and AI summaries as views, the same pattern as Knowledge Automata. The first account's design then split what a snapshot holds from how the index is computed, because the weights and normalization are still being learned. This ADR records both parts, so it takes the later date. Source: [02-kickoff-handoff.md](../../dev/discovery/02-kickoff-handoff.md), decision 9 (2026-10-28); [04-food-cost-account.md](../../dev/discovery/04-food-cost-account.md), "Two layers" (2026-10-30).

## Decision

- **Snapshots** are JSON files under a versioned schema. They store raw facts only: the before and after commits, each component's signed change, the scope counts with their evidence, the LLM judgement, and the model, prompt and tool versions.
- **Indexes** are computed from snapshots by a versioned method (for example `food-cost-method v0.1`).
- A committed snapshot is never rewritten. Changing weights or normalization, or blending in the LLM judgement, is a new method version that recomputes the views.

## Consequences

- History stays stable while the method changes, and recomputing is cheap: no rerun of tools or LLMs.
- Views (scatter, trend, reports, AI summaries) are disposable outputs of a snapshot set and a method version.
- Snapshots must capture enough raw facts up front; a fact that was never recorded can't be added to old snapshots later without a new run.
