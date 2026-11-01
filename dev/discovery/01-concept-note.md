> **Frozen discovery record.** Copied from the Kitchen Books project on claude.ai ("Scope-normalized complexity index concept note", 2026-10-28), with "Why now" revised by Miguel on 2026-11-01 before the first commit. See [README.md](README.md).

# Kitchen Books: concept note

- **Author:** Miguel, with Claude
- **Date:** 2026-10-28
- **Status:** Idea, not started

## Name and structure

**Kitchen Books** (complexity accounting for codebases) is the umbrella standard. Keeping the books means tracking numbers over time. Each metric is one account in the books.

- **First account:** complexity change versus scope change (this note). Working name: Food Cost.
- **Possible later accounts:** dead code (Waste Log), coupling, test health.

Kitchen Books sits beside Open Kitchen (agent flow watcher), Mise en Place (style guide) and Cookbook (recipes).

## The idea

Codebases keep getting more complex. Some of that growth is essential, because new features need it. Some is accidental: features that are no longer supported but never removed, or features built with poor architecture that add more complexity than they need. Two developers can deliver the same feature and leave very different amounts of complexity behind.

This project measures how much complexity each iteration adds (per pull request, handoff, sprint or epic) **and compares it with the scope that iteration delivered**. Growth that matches scope is fine. Growth far beyond scope is a warning.

It shifts the understanding of product complexity to the left, the same way CI/CD shifted quality and docs-as-code shifted documentation.

## Why now

*Revised 2026-11-01: reordered so the main driver comes first, and widened from managers to everyone who writes code.*

1. **AI lowered two barriers, not one.** It lowered the barrier to implementation, so we can generate far more code than before. It also lowered the barrier to proper engineering: managing technical debt, refactoring, measuring. Tracking complexity is one of those practices teams set aside because there was never time; AI makes it cheap enough to do (P15). Agentic coding often over-builds, and a complexity spike on what was planned as a small change shows up before anyone reads the diff.
2. **People love adding, not deleting.** With or without AI, at home or at work, a new feature is exciting and removing one rarely is, so codebases accumulate. A visible metric makes us step back before we just add more. Deleting what doesn't compound (P16) is often skipped; a trend line rewards cleanup, refactoring and deprecations, because the index goes down.
3. **For anyone who writes code, solo developer to team.** Complexity is expected to grow as features are added. The metric shows whether the growth is expected and under control, or whether something doesn't add up, whether the code came from you, other people or agents.
4. **Managers ask for it too.** Management teams are asking how to measure code complexity before they make decisions about it. They are one stakeholder among several, not the only one.

## What already exists

- **SonarQube:** cognitive complexity and quality gates on new code in pull requests.
- **CodeScene:** code health, hotspots (complexity combined with change history) and pull request checks over time.
- Engineering metrics platforms track delivery, not the complexity-to-scope ratio.

**Gap (to verify with a proper search):** normalizing the complexity added against the scope delivered, with semantic judgement from an LLM on top of deterministic metrics. That normalization is this project's contribution.

## Design

### A standard first, then watchers

1. **The standard** is the contract: the snapshot schema, the components, the LLM rubric and the normalization method.
2. **Watchers** are reference implementations, for example a CI job that writes a JSON snapshot to the repo on every merge or tag.
3. **Views** are generated from the snapshots: a management dashboard, developer reports in pull requests, and an AI-ready summary that architect reviewers (Opus, Fable) read before a review.

This follows the Knowledge Automata pattern: structured JSON as the single source of truth, with views for each audience.

### A composite index

- **Deterministic components (code computes them):** lines of code, modules touched, cyclomatic and cognitive complexity, coupling and dependencies, interactions added. Reuse existing tools where possible.
- **Semantic components (an LLM proposes, code checks):** estimated size of the scope delivered, and judgement on whether the implementation is proportionate. Each score uses a fixed rubric with anchor examples and cites evidence from the diff.
- **Scope reference:** the expected scope stated in the ticket, epic or handoff, compared with the actual change.

### Composition: two indexes, not a ratio

Each side is a weighted combination of its components, normalized against the repo's own history. Start with equal weights; tune them later against outcomes such as defects.

- **Complexity change:** the combined complexity added or removed by an iteration.
- **Scope change:** the combined scope delivered. It can be zero (a refactor or dead-code removal keeps behavior the same) or negative (a feature removed).

A single ratio (complexity divided by scope) is avoided: it explodes as scope approaches zero, and a negative divided by a negative looks healthy for the wrong reason.

### Views

- **Scope versus complexity scatter:** scope change on the horizontal axis, complexity change on the vertical, one dot per pull request or handoff. Dots near the diagonal are proportionate; dots far above it are bloat; dots straight below the center are refactors or cleanups; dots down and to the left are feature removals. Dots are shaded by date, or animated release by release.
- **Cumulative trend:** two lines over time, cumulative complexity and cumulative scope. Lines climbing together are healthy; a widening gap is accumulated accidental complexity; a refactor shows as the gap closing.

## Risks and cautions

- **Correlated components:** averaging cancels errors only when they are independent. Lines of code, modules touched and interactions tend to move together and can share blind spots.
- **Goodhart's law:** once a single number becomes a target, people game it. Dashboards must always let you drill down into the components.
- **LLM noise:** semantic scores vary between runs. Use rubrics, multiple runs and recorded evidence.
- **Validation:** the index has value only if it predicts something people care about, such as defects, slower delivery or harder onboarding. Without that, it's just a nicer-looking number.

## Possible first steps

1. **v1 is the standard only:** search for existing work, then define the first account, the snapshot schema and the LLM rubric.
2. **Test on Knowledge Automata:** backfill snapshots over its handoff history (PRs #1 to #8) with sample scripts kept in the repo, and check whether the numbers give useful insights.
3. **Generalize:** refine the standard from what the first project teaches, then try it on more projects.
4. **Library last:** package a Python library and CI watcher only after several projects show real value.

## Open questions

- How to define and measure scope reliably.
- Which granularity comes first: pull request, handoff or release.
- What outcome to validate the index against.
- Name for the project.
