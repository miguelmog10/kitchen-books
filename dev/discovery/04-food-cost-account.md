> **Frozen discovery record.** Copied unchanged from the Kitchen Books project on claude.ai ("Kitchen Books: first account (Food Cost)", decisions recorded 2026-10-30). See [README.md](README.md).

# Kitchen Books: first account (Food Cost)

- **Author:** Miguel, with Claude
- **Date:** 2026-10-28, decisions recorded 2026-10-30
- **Status:** Design agreed. Decisions D1 to D6 settled by Miguel and Claude (voice session). Ready to become `standard/accounts/food-cost.md` in the first implementation handoff.
- **Read with:** "Kitchen Books: concept note" and "Kitchen Books: prior-art search".

## What the account answers

For each iteration: **did the complexity it added match the scope it delivered, compared with how this repo usually behaves?**

The answer is relative to the repo's own history. A dot above the diagonal means "this change added more complexity per unit of scope than this repo's typical change", not "this code is bad" in absolute terms.

## Unit of measure (D2)

**One merged pull request.** In Miguel's workflow a handoff is an epic-sized block of work, and one PR closes it. Individual commits are too noisy to measure. Handoffs, sprints and releases are sums of PRs, so they come for free as roll-ups.

**Knowledge Automata today:** ten merged PRs. Five close handoffs (PR #1 for 001, #3 for 002, #4 for 003, #5 for 004, #8 for 005). The other five are CI, docs and process changes (#2, #6, #7, #9, #10). The backfill covers all ten: the docs-only PRs should land near the origin, which is a free sanity check. Handoff 006 will add PR #11.

## Two layers: raw facts and the index

1. **The snapshot stores raw facts only:** the before and after commits, each component's signed change, the scope counts with their evidence, the LLM judgement and the model and prompt versions.
2. **The index is computed from snapshots** by a versioned method (`food-cost-method v0.1`). Changing weights or normalization, or later blending in the LLM judgement, never rewrites a snapshot and never needs a rerun: it is a new method version that recomputes the views.

This keeps history stable while the method is still being learned.

## Complexity side (technical change)

**Production code only (D3).** Test code is recorded in the snapshot but kept out of the index. More tests usually mean healthier code, and coverage tools already exist. Tests can also add complexity without adding value; that question is parked for a later Test Health account.

Components are grouped so that correlated ones don't outvote the others. Each group gets one third of the weight; inside a group, components are averaged.

| Group | Component | Tool (reuse, don't rebuild) |
|---|---|---|
| **Size** | Change in logical lines of code (no blanks or comments) | Lizard (`nloc`) |
| **Logic** | Change in total cyclomatic complexity | Lizard (`CCN`) |
| | Change in total cognitive complexity | complexipy (Python) |
| **Structure** | Change in number of functions and modules | Lizard, file listing |
| | Change in internal import edges (coupling) | grimp or pydeps (Python) |
| | Change in external dependencies | lock or requirements file diff |

Knowledge Automata is Python (3.11+, `uv`), so every tool in the table applies to the first test. Other languages get Lizard's components first; their cognitive and coupling tools are chosen when a second project needs them.

Removals count at full value: deleting code really lowers the complexity of what remains. (AEP discounts deletions because it measures effort; we measure the resulting codebase.)

## Scope side (functional change) (D1)

Scope is **functional size, counted with COSMIC** (ISO/IEC 19761, maintained by the COSMIC organization). Using an existing standard follows the "don't reinvent the wheel" rule. Each functional process the change adds, modifies or removes is counted by its data movements:

- **Entry:** data comes in from a functional user (a person, a CLI call, another system).
- **Exit:** data goes out to a functional user.
- **Read / Write:** data is read from or written to persistent storage.
- One data movement is one point. Added and modified movements count +1, removed ones count −1. A pure refactor scores 0.

For LLM projects the anchor examples need to say, for instance, that a call to a model provider is an Exit plus an Entry, and that writing a JSON record to disk is a Write.

**How it is counted (model proposes, code checks):**

1. The LLM reads the diff and the PR description and lists the functional processes and data movements, each citing a file and line in the diff.
2. Code checks the list: cited lines exist in the diff, each process has an Entry and at least one Exit or Write, duplicates are removed. Then code counts.
3. Three runs; the snapshot keeps the median and the spread, and flags runs that disagree a lot.

**Intended scope** is counted the same way from the handoff or issue text, before looking at the code. It is not part of the index. Intended versus delivered is shown on its own, as a scope-creep signal.

## Normalization and composition (D5)

Think of it as T-shirt sizing, where "medium" is this repo's usual PR.

- Each component's signed change is divided by that component's **median absolute change across the repo's PRs**, so a value of 1 means "as much as a typical PR here".
- Complexity index = equal-weight mean of the three groups.
- Scope index = delivered scope change divided by the median absolute scope change.
- Both axes are then in the same unit (typical PRs), which is what makes the diagonal meaningful.
- With ten PRs the medians are rough; they firm up as history grows. Raw snapshots make recomputing cheap.

## LLM proportionality judgement (D4)

On every PR the LLM reads the diff alongside the handoff and scores it from −2 (under-built) to +2 (over-built), against a fixed rubric, quoting the lines it relies on.

It is recorded as an **annotation, not part of the index**:

- LLM scores drift between runs and models; the index should stay steady.
- It is an independent cross-check. A dot well above the diagonal that the LLM also calls over-built is strong evidence; when they disagree, one of them is wrong, and that is worth a look.
- Because the raw scores and evidence are in every snapshot, they can be blended into the index later as a new method version, without rerunning anything.

## How v1 is validated (D6)

Ten PRs are too few for statistics. Instead, before seeing any numbers, Miguel labels each Knowledge Automata PR from memory: proportionate, over-built, cleanup, removal, or docs and process only. Then we check whether each dot lands in the matching region of the scatter. Disagreements are the most useful output: either the method is wrong or the memory is.

## Future idea: a planning gate

The intended-scope count needs only the handoff or ticket text, so it can run **before any code exists**. Teams (product owners, project managers, Miguel and Claude) could turn it into a rule: question a handoff whose intended scope is large for a single PR, and split it before work starts. That moves the check further left, from the PR back to planning.

Knowledge Automata already shows the need: its handoffs grew from 34 KB to 55 KB (handoff 006). Not part of v1; a candidate for a later account or a watcher.

## Snapshot sketch (illustrative, not the schema)

```json
{
  "schema": "kitchen-books/snapshot/v0.1",
  "account": "food-cost",
  "unit": {"type": "pull_request", "id": 5, "base": "<sha>", "head": "<sha>"},
  "complexity": {
    "production": {"nloc": 412, "ccn": 37, "cognitive": 29, "functions": 18,
                   "modules": 3, "import_edges": 11, "external_deps": 1},
    "tests": {"nloc": 220, "functions": 14}
  },
  "scope": {
    "delivered": {"cfp_median": 9, "cfp_runs": [9, 9, 11], "processes": ["..."]},
    "intended": {"cfp_median": 6, "source": "dev/handoffs/004-decision-slice.md"}
  },
  "judgement": {"proportionality": 1, "evidence": ["..."]},
  "provenance": {"model": "<id>", "prompt": "scope-cosmic v0.1",
                 "tools": {"lizard": "<version>"}}
}
```

## Decisions

| # | Decision | Agreed |
|---|---|---|
| D1 | How scope is measured | COSMIC functional size (ISO/IEC 19761) from the diff; the LLM proposes, code checks |
| D2 | First unit of measure | Merged pull request; roll up to handoff and release |
| D3 | Tests in the complexity index | Out; recorded separately; value of tests parked for a Test Health account |
| D4 | LLM proportionality judgement | Annotation only; raw scores kept so it can be blended in later |
| D5 | Normalization | Median absolute change over the repo's own PRs; raw facts in snapshots |
| D6 | v1 validation | Miguel's blind labels of Knowledge Automata's PRs against the scatter |

## Other project decisions

- **License:** Apache 2.0 for everything in the repo (docs, schemas, rubrics, scripts), as in Knowledge Automata. Recorded as ADR-0001 in the first implementation handoff.

## Still open

- Where the sample scripts live: in the Kitchen Books repo under `examples/`, or in the Knowledge Automata repo.
- Final name for the first account (Food Cost is a working name).
