# Handoff 001: implementer plan

- **Owner:** Claude, implementer (Claude Code, VS Code extension, Claude Opus 5.5). Only the implementer edits this file.
- **Handoff:** [001-scaffolding.md](../handoffs/001-scaffolding.md)
- **Review:** [001-plan-review.md](001-plan-review.md)
- **Version:** 1 (2026-11-01)

Not using plan mode. This file stays uncommitted until the review says `approved`; then it is my first commit (`AGENTS.md`, plan exchange step 6).

## Read before planning

`AGENTS.md`, the handoff, discovery records 01 to 06 in the README's order, the v1.2.0 templates (`git show v1.2.0:templates/<file>`), the v1.2.0 playbook rule table, and Knowledge Automata's `docs/decisions/` and `dev/` files for shape (read-only).

## Steps and files

### 1. Process files (commit 2)

All from the v1.2.0 templates. Each keeps the header comment's first two lines (source and CC0) inside `<!-- -->`, as in `CLAUDE.md`, and drops the rest of that comment. Every `[bracket]` filled or removed.

| File | Template | Adaptation |
| --- | --- | --- |
| `dev/costs.md` | `costs.md` | Columns `PR \| Title \| Planner time \| Implementer time \| Miguel's time \| Notes`, no rows. One-line note: cost is measured in time for now, a rule 8 experiment on a subscription. Intro says the planner adds each row as the last commit before merging. |
| `dev/principles-in-practice.md` | `principles-in-practice.md` | As the template, with an empty `## Entries` (entries come in commit 5) |
| `dev/tool-feedback.md` | `tool-feedback.md` | As the template, with an empty `## Entries` (entries come in commit 5) |
| `.github/pull_request_template.md` | `pull_request_template.md` | As the template, plus the checklist line "Plan and review files in `dev/plans/` complete" |
| `docs/decisions/0000-template.md` | `adr.md` | As the template. Its `NNNN`, `YYYY-MM-DD` and `Proposed \| Accepted \| ...` are the template's own fields, not `[bracket]` placeholders, so they stay. |

The two logs keep the template's second comment (the entry-format example), as Knowledge Automata's do. It is the entry pattern, not the header comment.

`dev/STATUS.md` and `dev/backlog.md` also come from their templates (`STATUS.md`, `backlog.md`), but are created in commit 7 with their full content, so neither is ever committed with placeholders (see "Commit sequence").

### 2. ADRs (commit 3)

`docs/decisions/README.md` indexes them like Knowledge Automata's (number, title, status table), linking `0000-template.md`. Each ADR follows the template, Accepted, a few lines per section, source record linked in Context. File names as in the handoff.

| # | Date | Why that date |
| --- | --- | --- |
| 0001 License | 2026-10-30 | Open in 02 (2026-10-28), decided in 04 |
| 0002 Standard first, library last | 2026-10-28 | 02, decision 10 |
| 0003 Two indexes, no ratio | 2026-10-28 | 02, decisions 3 and 4 |
| 0004 Provider-agnostic semantic scoring | 2026-10-28 | 02, decisions 7 and 8 |
| 0005 Raw-fact snapshots, versioned method | 2026-10-30 | JSON snapshots under a versioned schema date from 02 decision 9 (2026-10-28); the "raw facts only, index from a versioned method, never rewrite" part is from 04's "Two layers". The ADR records the whole decision, so it takes the later date, and Context says so. |
| 0006 COSMIC scope | 2026-10-30 | 04, D1 |

Titles in each H1 are the handoff's titles. The index uses the same titles, shortened only if a cell gets unreadable.

### 3. Product docs (commit 4)

Both start with a line "Version 0.1 (draft)" and a line saying where they came from, linking `dev/discovery/`.

- `standard/overview.md`: what Kitchen Books is; books, accounts, snapshots, methods and views; "Why now" (all four points of the revised list in 01); the gap as restated in 03; the order (standard, test, generalize, library); links to ADRs 0002 to 0006 for the why and to `accounts/food-cost.md`. Later accounts named with their roles (Waste Log (dead code account), Test Health (test value account), coupling).
- `standard/accounts/food-cost.md`: record 04 as a standard page, in this order: the question; unit (merged PR, roll-ups); two layers; complexity side (production only, the three groups and tools table, removals at full value); scope side (COSMIC, data movement types, LLM anchors for LLM projects, model proposes and code checks, three runs, intended scope as a separate signal); normalization and composition; LLM proportionality judgement as an annotation; validation (blind labels before numbers); future: the planning gate; illustrative snapshot sketch, marked "not the schema". Drops "we agreed", the decisions table and the voice-session framing. Section 3 updates applied: "every merged Knowledge Automata PR" with no count, docs-only PRs expected near the origin, no Copilot. Says "Food Cost" is a working name.

### 4. Running logs (commit 5)

Entries in the templates' formats, oldest first (newest at the bottom), with the record's dates. Facts unchanged.

`dev/tool-feedback.md`: record 06's ten rows, plus the auto-memory entry (2026-11-01, Claude Code, confusing). The template's types are confusing, missing, bug, delight. My mapping, which the planner can change:

| Record 06 row | Type |
| --- | --- |
| Git and GitHub: tag on the wrong commit | confusing |
| GitHub rulesets: `GH013` blocked the delete | delight |
| `git ls-remote`: only the tag object | confusing |
| GitHub Releases: tag and Release are separate | confusing |
| claude.ai: hash read-back, all matched (2026-10-30) | delight |
| claude.ai: rewrite delivered older versions | bug |
| claude.ai: no GitHub web pages | missing |
| Claude Code: `AGENTS.md` ignored when `CLAUDE.md` exists | confusing |
| Setup: planner and implementer apps | see question 1 |
| Principles backlog: Copilot experiment for Kitchen Books | bug |

The record's "What we do now" column becomes the entry's "Suggested improvement" line (the template allows "any workaround that worked").

`dev/principles-in-practice.md`: record 06's four rows, with sides P8 teamwork (memory moved before the tool switch), P1 product (the design was kept), P14 teamwork (a release-process lesson) and P11 teamwork. Evidence lines point at the file or command the record names (`dev/discovery/`, record 06, `git ls-remote`). Plus the two 2026-11-01 entries from handoff section 8:

- P1 Design before prompting (bent): playbook rule 1 (P1, P5) adapted to a plan file. Evidence: `AGENTS.md`, "Plan exchange"; this file.
- P5 AI generates. Humans own (teamwork): shared working tree and branch, each session staging its own files by path. Evidence: `AGENTS.md`, "Commits, branches and pull requests".

Record 06's "For the principles repo" items go to the backlog's *Later* (step 6), as the handoff lists.

### 5. README (commit 6)

Rewrite: what it is in plain words; "Why now" with the first two points of the revised list; status (scaffolding in progress, handoff 002 next); a map of `standard/`, `docs/decisions/`, `dev/`; how it is built (planner and implementer sessions, the plan exchange, one or two lines each, linking `AGENTS.md`, the principles and playbook at v1.2.0); license.

### 6. STATUS, backlog, report (commit 7)

- `dev/backlog.md`: Now, Later, Open questions and Technical debt exactly as handoff section 8 lists them. Experiments from the primer one line each under Open questions.
- `dev/STATUS.md`: one line per item. Phase: scaffolding built on `chore/scaffolding`, waiting for the implementation review and merge; next, Miguel's blind labels, then handoff 002.
- Implementation report at the end of this file, with my start and end times.

## Commit sequence

As handoff section 9, one change: STATUS and backlog are created in commit 7 rather than commit 2, so no file is ever committed with placeholder content.

1. `docs: add implementer plan for handoff 001` (this file), after the planner commits the review with `approved`
2. `chore: add process files from the v1.2.0 templates`
3. `docs: record ADRs 0001 to 0006`
4. `docs: add the standard overview and the Food Cost account`
5. `docs: move discovery notes into the running logs`
6. `docs: rewrite the README`
7. `docs: update STATUS and backlog, add implementation report`

Each commit stages its files by path after `git status`, ends with `Co-authored-by: Claude implementer (Claude Opus 5.5) <noreply@anthropic.com>`, and its body says why. No push.

## How I will check the result

A short script in my session scratchpad (not in the repo), run before commit 7 and again after it:

- Every relative Markdown link in the new and changed files resolves to an existing file or folder.
- No U+2014 (em dash) in any new or changed file. I will also avoid U+2013.
- No `[...]` placeholder text left from the templates (grep for `\[[A-Z][^]]*\.\]` and for the templates' exact placeholder sentences).
- `git diff d905736 -- dev/discovery .github/rulesets` is empty, and `git diff 5e4bd0d -- dev/handoffs/001-scaffolding.md AGENTS.md CLAUDE.md .claude/settings.json dev/plans/001-plan-review.md` shows no change of mine.
- Each file from a template still has its two source lines (grep for the `From AI-Assisted Software Engineering templates v1.2.0` line and the URL line after it).
- A read-through of each new file against handoff section 6 and the style rules (kitchen names paired with their role on first mention per document).

## Questions

1. **Record 06's "Setup" row in `dev/tool-feedback.md`.** It is a decision about the setup, not feedback on a tool, so none of the four types fits. Proposal: keep it in `tool-feedback.md` as "Claude Code, desktop app and VS Code extension (setup)", adding "setup" as a fifth type, since the setup is what later entries compare against. Alternative: move it to `principles-in-practice.md` as a P5 teamwork entry. I will use the proposal unless the review says otherwise; this does not block approval.

No other problems found in the handoff, `AGENTS.md`, `CLAUDE.md` or `.claude/settings.json`.
