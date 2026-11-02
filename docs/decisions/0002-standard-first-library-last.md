<!--
From AI-Assisted Software Engineering templates v1.2.0 (CC0 1.0):
https://github.com/miguelmog10/ai-assisted-software-engineering/tree/v1.2.0/templates
-->

# 0002. Standard first, test on Knowledge Automata, generalize, library last

- **Status:** Accepted
- **Date:** 2026-10-28

## Context

Kitchen Books (complexity accounting for codebases) could start as a tool, but nobody knows yet whether its numbers give useful insight. Packaging before that is known would lock in a method that is still being learned. Source: [02-kickoff-handoff.md](../../dev/discovery/02-kickoff-handoff.md), decision 10; [01-concept-note.md](../../dev/discovery/01-concept-note.md), "Possible first steps".

## Decision

Build in this order:

1. **The standard:** the accounts, the snapshot schema, the rubrics and prompts.
2. **Test it** on one real project, Knowledge Automata, with sample scripts kept in this repo.
3. **Generalize:** refine the standard from what the first project teaches, then try more projects.
4. **Library last:** a Python library and a CI watcher only after several projects show real value.

## Consequences

- v1 is documents, a schema, rubrics and sample scripts. No library, watcher, GitHub Action or hosted dashboard.
- The standard can change cheaply while it is being learned (P6, P16).
- Anyone who wants to use it before the library exists has to run the sample scripts by hand.
