> **Frozen discovery record.** Copied unchanged from the claude.ai doc "Kitchen Books: kickoff handoff" (2026-10-28), converted to Markdown. See [README.md](README.md).

# Kitchen Books: kickoff handoff

2026-10-28 · Miguel

This handoff starts the Kitchen Books project from the brainstorm of 2026-10-28. Read it with the concept note "Kitchen Books: concept note" before planning anything.

## Purpose

Kitchen Books (complexity accounting for codebases) is an open standard for tracking how a codebase's complexity grows compared with the scope it delivers, one iteration at a time.

- **Problem:** complexity keeps growing. Some growth is essential (new features); some is accidental (unsupported features never removed, poor architecture, agents that over-build).
- **Contribution:** existing tools (SonarQube, CodeScene) track complexity over time. Kitchen Books compares the complexity added with the scope delivered, using deterministic metrics plus an LLM's scored judgement.
- **Audiences:** management dashboards, developer reports on pull requests, and AI architect reviewers (Opus, Fable) that read the raw data before a review.
- **Kitchen family:** sits beside Open Kitchen (agent flow watcher), Mise en Place (style guide) and Cookbook (recipes).

## Decisions so far

| # | Decision | Why |
| --- | --- | --- |
| 1 | Kitchen Books is the umbrella standard; each metric is one account in the books. | Leaves room for metrics not yet thought of. |
| 2 | First account: complexity change versus scope change (working name Food Cost). | This comparison is the gap existing tools leave. |
| 3 | Two composite indexes (complexity, scope), each a weighted combination of components normalized against the repo's own history. Equal weights to start. | Weights get tuned later against outcomes such as defects. |
| 4 | No single ratio. | It explodes as scope approaches zero (refactors) and a negative over a negative looks healthy. |
| 5 | Views: a scope versus complexity scatter (one dot per iteration, shaded by date) and a cumulative trend with one line per index. | The scatter shows each change; the gap between the lines shows accumulated accidental complexity. |
| 6 | Deterministic components come from existing tools (for example Radon for Python, Lizard for many languages), run in CI. | Do not reinvent the wheel. |
| 7 | Semantic components: an LLM scores against a fixed rubric and cites evidence; code checks the output (model proposes, code checks). | Covers what code cannot measure, such as scope size. |
| 8 | Provider-agnostic: the standard defines rubric, prompt and output schema, not the provider. Every snapshot records model and prompt version. | A model swap must not look like a code change. |
| 9 | Snapshots are JSON under a versioned schema, the single source of truth; dashboards, PR reports and AI summaries are views. | Same pattern as Knowledge Automata. |
| 10 | Order: standard first, test on Knowledge Automata, generalize, library last. | Package only after several projects show real value (P6, P16). |

## Scope of v1

v1 is the standard, proven on one real project. No library, no packaged watcher.

**In scope**

- A search for existing work on scope-normalized complexity, recorded with sources.
- The definition of the first account: components, normalization, weights and how scope is measured.
- The snapshot JSON schema, versioned.
- The LLM rubric and prompt as versioned files, with anchor examples.
- Sample scripts that backfill snapshots over Knowledge Automata's history (PRs #1 to #8) and draw the two views.
- A short write-up of what the numbers showed, and what to change in the standard.

**Out of scope**

- A Python library or `pip install`.
- A GitHub Action or watcher for other projects.
- Hosted dashboards.
- Accounts beyond the first (dead code, coupling, test health).

## Proposed repo layout

A starting point that reuses Knowledge Automata's conventions (AGENTS.md, ADRs, STATUS.md, handoffs). To be confirmed in the first session.

```
kitchen-books/
├── AGENTS.md                 # How humans and AI assistants work here
├── README.md                 # What Kitchen Books is, in plain words
├── LICENSE
├── standard/                 # PRODUCT: the standard itself
│   ├── overview.md           # Books, accounts, snapshots, views
│   └── accounts/
│       └── food-cost.md      # First account: complexity vs scope
├── schemas/
│   └── snapshot.v1.json
├── rubrics/                  # Versioned LLM rubrics and prompts
├── docs/decisions/           # ADRs
├── examples/
│   └── knowledge-automata/   # Sample scripts, snapshots and views
└── dev/                      # PROCESS: STATUS.md, backlog.md, handoffs/, logs
```

First ADRs to write: the license, the standard-first approach, two indexes instead of a ratio, and provider-agnostic semantic scoring.

## First session and open questions

The first chat in the new project settles the two items below before any repo files are written (P1, design before prompting).

1. **Search for existing work** on scope-normalized complexity, including newer AI code-review tools, and record what exists and what the gap really is.
2. **Define the first account:** pick the components, how each is normalized, and how scope is measured.

Then Miguel creates the repo, and the first implementation handoff sets up the layout above.

**Open questions**

- [ ] How to define and measure scope reliably (ticket or handoff size, LLM estimate, or both).
- [ ] Which granularity comes first: pull request, handoff or release.
- [ ] Which outcome validates the index (defects, delivery speed, onboarding).
- [ ] Where the sample scripts live: in the Kitchen Books repo under `examples/`, or in the Knowledge Automata repo.
- [ ] License (Apache 2.0, like Knowledge Automata, or another).
- [ ] Final name for the first account (Food Cost is a working name).
