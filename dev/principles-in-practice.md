<!--
From AI-Assisted Software Engineering templates v1.2.0 (CC0 1.0):
https://github.com/miguelmog10/ai-assisted-software-engineering/tree/v1.2.0/templates
-->

# Principles in practice

A running log of how the [AI-Assisted Software Engineering principles](https://github.com/miguelmog10/ai-assisted-software-engineering/blob/v1.2.0/PRINCIPLES.md) showed up while building this project: where they made the **product** better, where they helped the **team** of humans and AI assistants work together, and where a principle had to **bend**.

| # | Principle | # | Principle |
|---|---|---|---|
| P1 | Design before prompting | P9 | Testing is mandatory |
| P2 | Architecture is a first-class deliverable | P10 | Distrust the happy path |
| P3 | Plan continuously, adapt relentlessly | P11 | Build evidence, not just confidence |
| P4 | Maintain traceability | P12 | Optimize for outcomes, not code |
| P5 | AI generates. Humans own | P13 | Documentation is part of development |
| P6 | Small increments beat big bang development | P14 | Version everything that matters |
| P7 | Treat AI artifacts as engineering artifacts | P15 | Reduce technical debt as you go |
| P8 | Externalize memory | P16 | Reuse what compounds, delete what doesn't |

Entry format: date, principle, side (product, teamwork or bent), what happened, one line of evidence (file, ADR or commit). Newest entries go at the bottom.

<!--
### YYYY-MM-DD: P1 Design before prompting (product | teamwork | bent)

What happened, in one or two sentences.
Evidence: file, ADR or commit.
-->

## Entries

### 2026-11-01: P8 Externalize memory (teamwork)

Discovery happened in a claude.ai project whose docs Claude Code cannot see. Before switching tools, the records were copied into `dev/discovery/`. Lesson: when work moves to a tool that only reads the repo, move the memory first.
Evidence: [`dev/discovery/`](discovery/README.md).

### 2026-11-01: P1 Design before prompting (product)

"Start from zero" restarted the process, not the design. Decisions made under P1 (discovery records 01 to 04) were kept; redoing them would have wasted good work.
Evidence: [`dev/discovery/05-planner-primer.md`](discovery/05-planner-primer.md), "What is settled".

### 2026-11-01: P14 Version everything that matters (teamwork)

Pinning to a tag protects a project only if the tag is right: the principles repo's `v1.2.0` tag first pointed at the v1.1.0 commit. Candidate release checklist: merge the release PR, switch to `main` and pull, tag, push, check the tag's commit, publish the Release. Worth proposing to the principles backlog.
Evidence: [`dev/tool-feedback.md`](tool-feedback.md), entries of 2026-11-01 on the tag.

### 2026-11-01: P11 Build evidence, not just confidence (teamwork)

The release fix was verified against GitHub (`git ls-remote`), not against the local clone or a summary of the commands.
Evidence: [`dev/discovery/06-notes-for-the-logs.md`](discovery/06-notes-for-the-logs.md).

### 2026-11-01: P1 Design before prompting (bent)

Playbook rule 1 (plan first, P1 and P5) says the implementer waits for approval "before it writes any file". Here the implementer's plan is itself a file, `dev/plans/NNN-plan.md`, written before any other file. The rule's intent holds (no work before approval); its wording bends. Candidate wording for the playbook if the plan exchange holds.
Evidence: `AGENTS.md`, "Plan exchange"; [`dev/plans/001-plan.md`](plans/001-plan.md).

### 2026-11-01: P5 AI generates. Humans own (teamwork)

Experiment: the planner and the implementer share one working tree and one branch, each committing its own files by path. Watch for clobbered or wrongly staged files. If it happens, the fix is separate worktrees, not more rules.
Evidence: `AGENTS.md`, "Commits, branches and pull requests".

### 2026-11-01: P5 AI generates. Humans own (teamwork)

Plan exchange, handoff 001: one round, approved with three notes and one answer, amended once before any build commit. The planner's review caught two problems in the plan: a README status line that would go stale on merge, and a diff-based check that could not tell the two sessions' changes apart. The planner's replacement, based on git author, could not work either, since every commit has the same author; Miguel caught it, and the fix reads the `Co-authored-by:` trailers instead.
Evidence: [`dev/plans/001-plan-review.md`](plans/001-plan-review.md), round 1.
