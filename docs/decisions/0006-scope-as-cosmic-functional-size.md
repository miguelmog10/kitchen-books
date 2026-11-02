<!--
From AI-Assisted Software Engineering templates v1.2.0 (CC0 1.0):
https://github.com/miguelmog10/ai-assisted-software-engineering/tree/v1.2.0/templates
-->

# 0006. Scope as COSMIC functional size (ISO/IEC 19761), LLM proposes and code checks

- **Status:** Accepted
- **Date:** 2026-10-30

## Context

The scope side of every account needs a measure of what a change delivers. Story-point estimates from LLMs are too noisy to anchor an index. COSMIC functional size is an existing standard, can be counted from both a ticket and a diff, gives zero for a refactor and a negative value for a removal, and LLM-plus-rules counting has already shown good results. Source: [04-food-cost-account.md](../../dev/discovery/04-food-cost-account.md), D1; [03-prior-art.md](../../dev/discovery/03-prior-art.md), "What this changes for Kitchen Books".

## Decision

Measure scope as COSMIC functional size (ISO/IEC 19761):

- Each functional process a change adds, modifies or removes is counted by its data movements (Entry, Exit, Read, Write), one point each. Added and modified movements count +1, removed ones -1.
- The LLM lists the processes and movements, each citing a line in the diff. Code checks the list against the diff and the COSMIC rules, removes duplicates, then counts.
- Interactions added are data movements, so they belong on the scope side, not the complexity side.

## Consequences

- Scope uses a published standard instead of an invented one ("don't reinvent the wheel").
- Intended scope (from a handoff or issue) and delivered scope (from the diff) are counted the same way and can be compared.
- The rubric needs anchor examples for cases COSMIC doesn't spell out, such as calls to a model provider.
- Any later account that measures scope uses this measure, so changing it is a new ADR.
