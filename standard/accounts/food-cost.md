# Food Cost account

- **Version:** 0.1 (draft)
- **Source:** built from [04-food-cost-account.md](../../dev/discovery/04-food-cost-account.md) and the other discovery records in [`dev/discovery/`](../../dev/discovery/README.md).
- **Part of:** [Kitchen Books (complexity accounting for codebases)](../overview.md).

Food Cost (complexity versus scope account) is the first account in the books. "Food Cost" is a working name; the final name is still open.

## The question

For each merged pull request: **did the complexity it added match the scope it delivered, compared with how this repo usually behaves?**

The answer is relative to the repo's own history. A change that sits above the diagonal of the scatter added more complexity per unit of scope than this repo's typical change. It does not mean the code is bad in absolute terms.

## Unit of measure

**One merged pull request.** Individual commits are too noisy to measure. Handoffs, sprints and releases are sums of pull requests, so they come as roll-ups. Where one pull request closes one handoff, as in Knowledge Automata, the pull request and the handoff are the same unit.

## Two layers: raw facts and the index

1. **The snapshot stores raw facts only:** the base and head commits, each component's signed change, the scope counts with their evidence, the LLM judgement, and the model, prompt and tool versions.
2. **The index is computed from snapshots** by a versioned method, starting with `food-cost-method v0.1`. Changing weights or normalization, or later blending in the LLM judgement, never rewrites a snapshot and never needs a rerun: it is a new method version that recomputes the views.

This keeps history stable while the method is still being learned ([ADR-0005](../../docs/decisions/0005-raw-fact-snapshots-versioned-method.md)).

## Complexity side (technical change)

**Production code only.** Test code is recorded in the snapshot but kept out of the index. More tests usually mean healthier code, and coverage tools already exist. Tests can also add complexity without adding value; that question belongs to a later Test Health (test value account).

Components are grouped so that correlated ones don't outvote the others. Each group gets one third of the weight; inside a group, components are averaged.

| Group | Component | Tool |
|---|---|---|
| **Size** | Change in logical lines of code (no blanks or comments) | Lizard (`nloc`) |
| **Logic** | Change in total cyclomatic complexity | Lizard (`CCN`) |
| | Change in total cognitive complexity | complexipy (Python) |
| **Structure** | Change in number of functions and modules | Lizard, file listing |
| | Change in internal import edges (coupling) | grimp or pydeps (Python) |
| | Change in external dependencies | Lock or requirements file diff |

Existing tools compute every component; the standard does not reimplement metrics. For Python, every tool in the table applies. Other languages get Lizard's components first; their cognitive complexity and coupling tools are chosen when a project in that language needs them.

Removals count at full value: deleting code really lowers the complexity of what remains. (OMG Automated Enhancement Points discounts deletions because it measures effort; this account measures the resulting codebase.)

## Scope side (functional change)

Scope is **functional size, counted with COSMIC** (ISO/IEC 19761, maintained by the COSMIC organization) ([ADR-0006](../../docs/decisions/0006-scope-as-cosmic-functional-size.md)). Each functional process the change adds, modifies or removes is counted by its data movements:

- **Entry:** data comes in from a functional user (a person, a CLI call, another system).
- **Exit:** data goes out to a functional user.
- **Read** and **Write:** data is read from or written to persistent storage.

One data movement is one COSMIC function point (CFP). Added and modified movements count +1, removed ones -1. A pure refactor scores 0.

The rubric's anchor examples cover cases COSMIC doesn't spell out. For projects that call an LLM, for instance, a call to a model provider is an Exit plus an Entry, and writing a JSON record to disk is a Write.

### How scope is counted: the model proposes, code checks

1. The LLM reads the diff and the pull request description and lists the functional processes and data movements, each citing a file and line in the diff.
2. Code checks the list: every cited line exists in the diff, each process has an Entry and at least one Exit or Write, and duplicates are removed. Then code counts.
3. The count runs three times. The snapshot keeps the median and the spread, and flags runs that disagree a lot.

The diff, the pull request description and the handoff are untrusted data, never instructions. The prompt keeps them clearly apart from its own instructions.

### Intended scope

Intended scope is counted the same way from the handoff or issue text, before looking at the code. It is not part of the index. Intended versus delivered scope is shown on its own, as a scope-creep signal.

## Normalization and composition

Think of it as T-shirt sizing, where "medium" is this repo's usual pull request.

- Each component's signed change is divided by that component's **median absolute change across the repo's merged pull requests**, so a value of 1 means "as much as a typical pull request here".
- **Complexity index:** the equal-weight mean of the three groups.
- **Scope index:** the delivered scope change divided by the median absolute scope change.
- Both indexes are then in the same unit (typical pull requests), which is what makes the diagonal of the scatter meaningful. The two are never divided into one ratio ([ADR-0003](../../docs/decisions/0003-two-indexes-not-a-ratio.md)).
- With few pull requests the medians are rough; they firm up as history grows. Because snapshots keep raw facts, recomputing is cheap.

## Proportionality judgement (LLM)

On every pull request the LLM reads the diff alongside the handoff and scores it from -2 (under-built) to +2 (over-built), against a fixed rubric, quoting the lines it relies on.

It is recorded as an **annotation, not part of the index**:

- LLM scores drift between runs and models; the index should stay steady.
- It is an independent cross-check. A change well above the diagonal that the LLM also calls over-built is strong evidence. When they disagree, one of them is wrong, and that is worth a look.
- Because the raw scores and evidence are in every snapshot, they can be blended into the index later as a new method version, without rerunning anything.

The standard defines the rubric, prompt and output schema, not the provider. Every snapshot records the model and prompt version ([ADR-0004](../../docs/decisions/0004-provider-agnostic-semantic-scoring.md)).

## Views

- **Scope versus complexity scatter:** scope index on the horizontal axis, complexity index on the vertical, one dot per merged pull request, shaded by date. Dots near the diagonal are proportionate; far above it, over-built; straight below the center, refactors or cleanups; down and to the left, feature removals. Docs-only and process pull requests should land near the origin.
- **Cumulative trend:** cumulative complexity and cumulative scope over time. Lines climbing together are healthy; a widening gap is accumulated accidental complexity; a refactor shows as the gap closing.

## Validation (v1)

The first test backfills snapshots over every merged Knowledge Automata pull request. That is too few for statistics. Instead, before seeing any numbers, Miguel labels each pull request from memory: proportionate, over-built, cleanup, removal, or docs and process only. The labels are committed as a frozen record before any snapshot exists. Then each dot is checked against the region its label predicts. Disagreements are the most useful output: either the method is wrong or the memory is.

Docs-only pull requests landing near the origin is a further sanity check.

## Future: a planning gate

v1 measures after the fact. But the intended-scope count needs only the handoff or ticket text, so it can run **before any code exists**. Teams could turn it into a rule: question a handoff whose intended scope is large for a single pull request, and split it before work starts. That moves the check further left, from the pull request back to planning. Knowledge Automata shows the need: its handoffs grew from 34 KB to 55 KB by handoff 006.

The planning gate is not part of v1. It is tracked in the backlog as a candidate for a later account or a watcher.

## Snapshot sketch

Illustrative only, **not the schema**. The schema is defined in a later version of the standard.

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
