<!--
From AI-Assisted Software Engineering templates v1.2.0 (CC0 1.0):
https://github.com/miguelmog10/ai-assisted-software-engineering/tree/v1.2.0/templates
-->

# Backlog

## Now

A sketch of the next handoffs, in order. Each is planned in full when it starts.

- **Miguel's blind labels (D6):** each merged Knowledge Automata pull request labeled from memory (proportionate, over-built, cleanup, removal, or docs and process only), committed as a frozen discovery record before any schema or number exists.
- **Handoff 002:** snapshot schema v0.1 and the COSMIC rubric with anchor examples, after the blind labels are committed.
- **Handoff 003:** backfill scripts over Knowledge Automata.
- **Handoff 004:** the two views (scope versus complexity scatter, cumulative trend).
- **Handoff 005:** the write-up of what the numbers showed, and what to change in the standard.

## Later

- No library or packaging (relaxed at prototype level).
- No watcher or GitHub Action (relaxed at prototype level).
- One measured repo, Knowledge Automata (relaxed at prototype level).
- No coverage target (relaxed at prototype level).
- No formal threat model (relaxed at prototype level).
- CI only once there is code to check (relaxed at prototype level).
- Require the CI status check in the `main` ruleset once CI exists and has run once.
- Dependabot version updates once there are dependencies.
- The planning gate: count intended scope from a handoff before any code exists, and question handoffs too large for one pull request. See [the Food Cost account](../standard/accounts/food-cost.md#future-a-planning-gate).
- Later accounts: Waste Log (dead code account), coupling, Test Health (test value account).
- Blend the LLM proportionality judgement into the index, as a new method version.
- Cognitive complexity and coupling tools for languages other than Python.
- Prior-art checks before publishing v1: the docs of Qodo, Greptile and Graphite; the CodeScene paper (arXiv 2601.02200) beyond its press release; the alignment categories in Altunhan et al.
- For the principles repo: the release checklist (see [principles in practice](principles-in-practice.md), P14 entry of 2026-11-01), and the handoff-size idea: the planning gate could one day answer the principles backlog's question about handoff size with a measure instead of a byte count.
- For the principles repo: the `setup` type added to `tool-feedback.md` in handoff 001, if it holds.

## Open questions

- Where the sample scripts live. Proposal: `examples/knowledge-automata/` in this repo, reading the Knowledge Automata clone by a path argument.
- The final name for the first account (Food Cost is a working name).
- An outcome to validate against beyond D6 (defects, delivery speed, onboarding).
- Whether the `main` ruleset's "require extra approval for unattributed changes" blocks a solo owner's merge. Checked on the pull request for handoff 001.

Experiments from the planner primer, results logged in [`tool-feedback.md`](tool-feedback.md) or [`principles-in-practice.md`](principles-in-practice.md):

- **The setup:** does anything behave differently between the planner (desktop app) and the implementer (VS Code extension), both Claude Code?
- **One instruction file:** does `AGENTS.md` plus the `@AGENTS.md` import hold with no drift?
- **Rule 11 inside one tool:** do two Claude Code sessions catch as much as Claude plus Copilot did on Knowledge Automata?
- **Rule 1:** does plan mode replace the "reply with a plan and wait" step?
- **Rule 4:** is verifying written files still needed when the implementer edits the repo directly?
- **Rule 8:** what is a useful cost measure on a subscription? Log Miguel's time and Claude's time per handoff.
- **Parallel handoffs:** if two implementers ever run at once, does the handoff need to say which files each one owns?
- **Plan exchange through files:** rounds per handoff, what the rounds caught, whether the trail was read again later.

## Technical debt

None yet.
