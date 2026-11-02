# Kitchen Books: overview

- **Version:** 0.1 (draft)
- **Source:** built from the discovery records in [`dev/discovery/`](../dev/discovery/README.md) (records 01 to 05). The reasons behind the design are in the [ADRs](../docs/decisions/README.md).

## What it is

Kitchen Books (complexity accounting for codebases) is an open standard for tracking how a codebase's complexity grows compared with the scope it delivers, one merged pull request at a time.

Codebases keep getting more complex. Some of that growth is essential, because new features need it. Some is accidental: features that are no longer supported but never removed, changes built with more complexity than they need, or agents that generate more than the task asked for. Two developers can deliver the same feature and leave very different amounts of complexity behind.

Kitchen Books measures how much complexity each change adds and compares it with the scope that change delivered. Growth that matches scope is fine. Growth far beyond scope is a warning. The aim is to see that warning on every pull request, not years later: to move the understanding of complexity to the left, the way CI/CD did for quality and docs-as-code did for documentation.

It is for anyone who writes code, from a solo developer to a team, and for the AI reviewers that read its snapshots before a review.

## The books

Keeping the books means recording numbers over time and reading them back. The standard has five parts:

- **Books:** the whole record for one repo.
- **Accounts:** one metric each. An account defines what it measures, its components, the tools that compute them and how they combine. The first is [Food Cost (complexity versus scope account)](accounts/food-cost.md). Possible later accounts: Waste Log (dead code account), coupling, and Test Health (test value account).
- **Snapshots:** JSON records, one per merged pull request, under a versioned schema. A snapshot stores raw facts only: what changed, measured by tools, and what an LLM proposed, with its evidence and the model and prompt that produced it. A committed snapshot is never rewritten ([ADR-0005](../docs/decisions/0005-raw-fact-snapshots-versioned-method.md)).
- **Methods:** the versioned rules that turn snapshots into indexes, for example how components are normalized and weighted. Changing a weight is a new method version, not a new snapshot.
- **Views:** what people and agents read, computed from snapshots by a method: a scope versus complexity scatter, a cumulative trend, reports on pull requests, and a summary for AI reviewers.

Each account keeps **two indexes**, complexity change and scope change, and never divides one by the other ([ADR-0003](../docs/decisions/0003-two-indexes-not-a-ratio.md)). Scope is measured as COSMIC functional size ([ADR-0006](../docs/decisions/0006-scope-as-cosmic-functional-size.md)). Where an LLM is needed, the standard defines the rubric, the prompt and the output schema, not the provider, and code checks every output ([ADR-0004](../docs/decisions/0004-provider-agnostic-semantic-scoring.md)).

## Why now

1. **AI lowered two barriers, not one.** It lowered the barrier to implementation, so we can generate far more code than before. It also lowered the barrier to proper engineering: managing technical debt, refactoring, measuring. Tracking complexity is one of those practices teams set aside because there was never time; AI makes it cheap enough to do. Agentic coding often over-builds, and a complexity spike on what was planned as a small change shows up before anyone reads the diff. CodeScene's own research reports at least 30% higher defect risk for AI-written code in unhealthy files ([Borg and Tornhill, 2026](https://tools.prnewswire.com/en-us/live/20813/release/20260128EN71904)); it comes from the vendor, but it supports measuring AI-written changes.
2. **People love adding, not deleting.** With or without AI, at home or at work, a new feature is exciting and removing one rarely is, so codebases accumulate. A visible metric makes us step back before we just add more. A trend line rewards cleanup, refactoring and deprecations, because the index goes down.
3. **For anyone who writes code, solo developer to team.** Complexity is expected to grow as features are added. The metric shows whether the growth is expected and under control, or whether something doesn't add up, whether the code came from you, other people or agents.
4. **Managers ask for it too.** Management teams are asking how to measure code complexity before they make decisions about it. They are one stakeholder among several, not the only one.

## The gap

Tools already track complexity on new code (SonarQube, CodeScene), size changes (OMG Automated Enhancement Points, the closest prior art) or ask an LLM to estimate a pull request's complexity (CodeRabbit). Research shows LLMs can size requirements with COSMIC and check whether a pull request matches its issue.

What none of them does is compare the **intended** scope of a change (from its ticket or handoff) with the complexity it actually **added**, per pull request, in an open format, with the evidence kept for human and AI reviewers. That comparison is what Kitchen Books adds. The search and its sources are in [03-prior-art.md](../dev/discovery/03-prior-art.md).

## The order

Kitchen Books is built in four steps ([ADR-0002](../docs/decisions/0002-standard-first-library-last.md)):

1. **The standard:** this overview, the accounts, the snapshot schema, the rubrics and prompts.
2. **Test:** sample scripts backfill snapshots over every merged pull request of one real project, [Knowledge Automata](https://github.com/miguelmog10/knowledge-automata), and draw the views.
3. **Generalize:** refine the standard from what the first project teaches, then try more projects.
4. **Library:** a packaged library and CI watcher, only after several projects show real value.

v1 is steps 1 and 2. It is not a library, a watcher, a GitHub Action or a hosted dashboard.
