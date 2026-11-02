<!--
From AI-Assisted Software Engineering templates v1.2.0 (CC0 1.0):
https://github.com/miguelmog10/ai-assisted-software-engineering/tree/v1.2.0/templates
-->

# 0004. Provider-agnostic semantic scoring: the standard defines rubric, prompt and output schema; every snapshot records model and prompt version

- **Status:** Accepted
- **Date:** 2026-10-28

## Context

Some components, such as the size of the scope a change delivers, can't be computed by code. An LLM can propose them, but its scores vary between runs and between models. If the standard named one provider, it would tie every user to it, and a model swap would look like a change in the code. Source: [02-kickoff-handoff.md](../../dev/discovery/02-kickoff-handoff.md), decisions 7 and 8.

## Decision

- The standard defines the rubric, the prompt and the output schema for each semantic component. It does not name a provider or a model.
- The LLM scores against the fixed rubric and cites evidence from the diff. Code checks the output against its schema and its cited evidence before it is stored (model proposes, code checks).
- Every snapshot records the model and the prompt version that produced it.

## Consequences

- Anyone can run the standard with the model they have.
- A change in the numbers can be traced to a model or prompt change instead of being mistaken for a code change.
- Rubrics and prompts become versioned files, immutable once a snapshot cites them.
- Comparing snapshots made with different models needs care; the recorded versions make that visible.
