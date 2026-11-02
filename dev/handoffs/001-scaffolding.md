<!--
From AI-Assisted Software Engineering templates v1.2.0 (CC0 1.0):
https://github.com/miguelmog10/ai-assisted-software-engineering/tree/v1.2.0/templates
-->

# Handoff 001: scaffolding

- **From:** Claude, planner-reviewer (Claude Code, desktop app)
- **To:** Claude, implementer (Claude Code, VS Code extension, fresh session)
- **Owner and approver:** Miguel
- **Date:** 2026-11-01
- **Branch:** `chore/scaffolding`
- **Scope:** The process files from the v1.2.0 templates, the first six ADRs, the first product docs (`standard/overview.md`, `standard/accounts/food-cost.md`) built from the discovery records, and a README rewrite. No code, no schema, no rubric, no scripts.

---

## 1. Context

Kitchen Books (complexity accounting for codebases) has its design settled but no process files yet. The design was worked out on claude.ai and copied into `dev/discovery/` as frozen records. This handoff turns those records into the repo's living docs and sets up the files every later handoff relies on. It is the first project to follow AI-Assisted Software Engineering v1.2.0 from handoff 001.

What already exists and must be reused, not rebuilt:

- [`dev/discovery/`](../discovery/README.md): the design. Records 01 to 04 are the content source for this handoff's product docs and ADRs. Read them in the order the folder's README gives. Where they disagree with [05-planner-primer.md](../discovery/05-planner-primer.md), 05 is newer.
- [`dev/discovery/06-notes-for-the-logs.md`](../discovery/06-notes-for-the-logs.md): log entries, checked, ready to move into the new logs.
- The v1.2.0 templates, read **at the tag**, never from `main`: [templates/](https://github.com/miguelmog10/ai-assisted-software-engineering/tree/v1.2.0/templates). A local clone is at `D:\git\miguelmog10\ai-assisted-software-engineering`; read it with `git show v1.2.0:templates/<file>`.
- [Knowledge Automata](https://github.com/miguelmog10/knowledge-automata) (local clone `D:\git\miguelmog10\knowledge-automata`): an example of the same conventions in use, for example `docs/decisions/0000-template.md` and its index. **Read-only.**

Read `AGENTS.md` (loaded through `CLAUDE.md`) before planning. There is no `dev/STATUS.md` yet; this handoff creates it.

**What Miguel and the planner decided (2026-11-01):**

1. **`AGENTS.md` is the only source of rules.** `CLAUDE.md` holds only `@AGENTS.md`. Both are committed on this branch.
2. **Plan exchange through files** in `dev/plans/`, as described in `AGENTS.md`. This handoff is its first trial.
3. **Sessions commit, Miguel pushes.** Each session commits its own files on this branch, staged by path. Miguel pushes, opens the pull request and merges it.
4. **Trailers name the role and model:** `Co-authored-by: Claude implementer (<model>) <noreply@anthropic.com>` for your commits.
5. **Scope as listed in section 6**, confirmed by Miguel. The six ADRs are Accepted: Miguel made each decision during discovery.

**Planner's proposals (Miguel confirms them when approving the plan):**

- Product docs start at **version 0.1 (draft)**, stated in a line at the top, so later handoffs can bump it.
- Decisions D2 to D5 (unit, tests, LLM judgement, normalization) stay in `standard/accounts/food-cost.md` rather than becoming ADRs: they belong to one account and will change with its method version. D1 (COSMIC) gets an ADR because it shapes every later account's scope side.
- Each ADR's date is the date of the decision in the discovery record, not today.
- `dev/costs.md` rows are added by the planner as the last commit before merging, so one session owns the file's numbers. You report your own time in the implementation report.
- The experiments from the primer go into `dev/backlog.md` under *Open questions*, so they are tracked in one place.

**Already committed on this branch:** `dev/discovery/` (all files), `.github/rulesets/` (both exports), and, in the planner's commits, this handoff, `AGENTS.md`, `CLAUDE.md`, `.claude/settings.json` and `dev/plans/001-plan-review.md` (so far only the planner's time log). Do not modify them. If you find a problem in any of them, raise it in your plan.

## 2. How to work on this task

1. **Plan first.** Read this whole document, then write your plan to `dev/plans/001-plan.md` before you create or change any other file, and stop. Keep it minimal: steps, files touched, anything here that doesn't fit, the commit sequence, how you will check the result.
2. **Wait for `approved`** in `dev/plans/001-plan-review.md`. The planner writes that file; Miguel approves from the planner session. Do not edit the review file.
3. **When revising,** bump the version line at the top of your plan and add a short "changes since the last version" note.
4. **If you use plan mode,** the plan you present on exit is only "write `dev/plans/001-plan.md` and stop". Miguel accepting that exit lets you write the plan file. It is not approval of the plan; only `approved` in the review file is.
5. Implement in small commits. Stage only your own files, by path.
6. If anything here is ambiguous or seems wrong, add a numbered question to your plan file and stop until the planner answers under the same number.
7. Finish by updating `dev/STATUS.md` and `dev/backlog.md`, and write an implementation report at the end of your plan file: what was built, what deviated from the plan and why, how you checked it, open points, and the time you spent (start and end).

## 3. Agreed design

The design is in records 01 to 04. Link to it; don't copy it (playbook rule 3). This handoff restates nothing from them except where the primer updated them:

- **Knowledge Automata has more PRs now.** The backfill will cover every merged Knowledge Automata PR when it runs, not "PRs #1 to #8" or "ten PRs". Docs-only PRs should land near the origin of the scatter. Write the product docs that way.
- **"Why now"** is the version revised on 2026-11-01 in record 01: AI lowering two barriers first, managers last. Use it for the README and `standard/overview.md`.
- **Leading, not only trailing.** v1 measures after the fact. The planning gate from record 04 (counting intended scope from a handoff before code exists) is a future idea: mention it in the account doc as future, and track it in the backlog.
- **Copilot is out of this project.** Nothing here mentions it except as history.

## 4. Target structure

```
kitchen-books/
├── README.md                          # rewritten
├── .github/
│   └── pull_request_template.md       # new
├── standard/
│   ├── overview.md                    # new
│   └── accounts/
│       └── food-cost.md               # new
├── docs/decisions/
│   ├── README.md                      # new: index
│   ├── 0000-template.md               # new
│   ├── 0001-license-apache-2.md
│   ├── 0002-standard-first-library-last.md
│   ├── 0003-two-indexes-not-a-ratio.md
│   ├── 0004-provider-agnostic-semantic-scoring.md
│   ├── 0005-raw-fact-snapshots-versioned-method.md
│   └── 0006-scope-as-cosmic-functional-size.md
└── dev/
    ├── STATUS.md                      # new
    ├── backlog.md                     # new
    ├── costs.md                       # new
    ├── principles-in-practice.md      # new
    ├── tool-feedback.md               # new
    └── plans/
        ├── 001-plan.md                # new, yours, written first
        └── 001-plan-review.md         # the planner's, not yours
```

ADR file names are suggestions; keep the numbers and titles.

## 5. Examples

- **Templates adapted in use:** Knowledge Automata's `dev/` files and `docs/decisions/`. Copy the shape, not the content.
- **ADR length:** a few lines per section, as in the template. ADR-0001 can be as short as Knowledge Automata's.

## 6. Requirements

**Every file copied from a v1.2.0 template** keeps the template's first two comment lines (source and CC0 notice) and drops the rest of the comment. Fill in or remove every `[bracket]`.

**Process files** (`dev/`, `.github/pull_request_template.md`, `docs/decisions/0000-template.md`):

- From the matching v1.2.0 template.
- `dev/costs.md`: columns adapted to a subscription, with time instead of credits: `PR | Title | Planner time | Implementer time | Miguel's time | Notes`, plus a one-line note that cost is measured in time for now (rule 8 experiment). No rows yet; the planner adds the first.
- `dev/principles-in-practice.md` and `dev/tool-feedback.md`: every entry from record 06, reworded into the template's entry format, with the record's dates. Facts unchanged.
- `.github/pull_request_template.md`: as the template, plus a checklist line "Plan and review files in `dev/plans/` complete".

**ADRs** (`docs/decisions/`), each Accepted, with its source record linked in Context:

| # | Title | Source |
| --- | --- | --- |
| 0001 | License: Apache 2.0 for everything in the repo (docs, schemas, rubrics, scripts) | 04, "Other project decisions" |
| 0002 | Standard first, test on Knowledge Automata, generalize, library last | 02, decision 10; 01, "Possible first steps" |
| 0003 | Two indexes (complexity change, scope change), no single ratio | 02, decisions 3 and 4; 01, "Composition" |
| 0004 | Provider-agnostic semantic scoring: the standard defines rubric, prompt and output schema; every snapshot records model and prompt version | 02, decisions 7 and 8 |
| 0005 | Snapshots store raw facts under a versioned JSON schema; indexes come from a versioned method and never rewrite a snapshot | 02, decision 9; 04, "Two layers" |
| 0006 | Scope as COSMIC functional size (ISO/IEC 19761), LLM proposes and code checks | 04, D1; 03, "What this changes" |

`docs/decisions/README.md` indexes them like Knowledge Automata's.

**Product docs** (`standard/`), version 0.1 (draft):

- `standard/overview.md`: what Kitchen Books is; books, accounts, snapshots, methods and views; the revised "Why now"; the gap as restated in record 03 (intended scope against added complexity, per PR, open); the order (standard, test, generalize, library). Links to the ADRs for the why and to `accounts/food-cost.md`.
- `standard/accounts/food-cost.md`: the account from record 04, turned from a design record into a standard page. Keep the substance (question, unit, two layers, complexity groups and tools, COSMIC scope counting, normalization, LLM judgement, validation, planning gate as future, illustrative snapshot sketch). Drop the discussion framing ("we agreed", the decisions table). Apply the updates from section 3.
- Both say where they came from, with a link to the discovery records.

**README rewrite:** what Kitchen Books is, in plain words; a short "Why now" (the first two points of the revised list are enough); status; a map of the repo (`standard/`, `docs/decisions/`, `dev/`); how it is built (the two Claude sessions and the plan exchange, one or two lines each, linking `AGENTS.md`); license.

**Writing style** as in `AGENTS.md`: plain English, no em dashes, the first mention of a kitchen name in each document paired with its plain role.

## 7. Real runs

No real run needed.

## 8. Documentation changes

- New ADRs 0001 to 0006, as in section 6.
- `dev/backlog.md`:
  - **Now:** handoff 002, snapshot schema v0.1 and the COSMIC rubric with anchor examples, after Miguel's blind labels of Knowledge Automata's PRs (D6) are committed as a frozen discovery record; handoff 003, backfill scripts over Knowledge Automata; 004, the two views; 005, the write-up. Mark these as a sketch.
  - **Later:** each item under "Deliberately relaxed" in `AGENTS.md`; require the CI status check in the `main` ruleset once CI exists and has run once; Dependabot version updates once there are dependencies; the planning gate; later accounts (Waste Log (dead code account), coupling, Test Health (test value account)); blending the LLM judgement into the index as a new method version; cognitive and coupling tools for languages other than Python; prior-art checks before publishing v1 (Qodo, Greptile and Graphite docs, the CodeScene paper arXiv 2601.02200, the alignment categories in Altunhan et al.); for the principles repo, the release checklist and the handoff-size idea from record 06.
  - **Open questions:** where the sample scripts live (proposal: `examples/knowledge-automata/`, reading the Knowledge Automata clone by a path argument); the final name for the first account; an outcome to validate against beyond D6; whether the `main` ruleset's "require extra approval for unattributed changes" blocks a solo owner's merge (checked on this PR); and the experiments from the primer's "Experiments to log from day one", one line each, results logged in `dev/tool-feedback.md` or `dev/principles-in-practice.md`.
  - **Technical debt:** none yet.
- `dev/principles-in-practice.md`: record 06's entries, plus two entries dated 2026-11-01:
  - Rule 1 (plan first) adapted: the implementer's plan is itself a file, `dev/plans/NNN-plan.md`, written before any other file. The rule's intent holds (no work before approval); its wording ("before writing any file") bends. Candidate wording for the playbook if the plan exchange holds.
  - Experiment: the planner and the implementer share one working tree and one branch, each committing its own files by path. Watch for clobbered or wrongly staged files. If it happens, the fix is separate worktrees, not more rules.
- `dev/tool-feedback.md`: record 06's entries, plus one entry dated 2026-11-01:
  - Claude Code auto-memory was on by default. The planner session wrote its time log there instead of in the repo. `/context` showed it loaded in a fresh session in the same folder, so the implementer would have started with the planner's notes. Turned off per repo with `.claude/settings.json`; a setting enforces what a rule in `AGENTS.md` could only ask for.
- Both logs: any new entries as they come up.
- `dev/STATUS.md`: written at the end. Phase: scaffolding done, handoff 002 next.

## 9. Suggested commit sequence

1. `docs: add implementer plan for handoff 001` (`dev/plans/001-plan.md`), your first commit, right after the planner has committed `dev/plans/001-plan-review.md` with `approved`
2. `chore: add process files from the v1.2.0 templates`
3. `docs: record ADRs 0001 to 0006`
4. `docs: add the standard overview and the Food Cost account`
5. `docs: move discovery notes into the running logs`
6. `docs: rewrite the README`
7. `docs: update STATUS and backlog, add implementation report`

The planner then commits its implementation review and the `dev/costs.md` row. Suggested pull request title: `chore: scaffold process files and first product docs (handoff 001)`.

## 10. Tests

No code, so no test suite. Check, and say in your report how:

- Every relative link in the new and changed files resolves.
- No em dash (U+2014) in any new or changed file.
- No `[bracket]` placeholders left from the templates.
- `git diff d905736 -- dev/discovery .github/rulesets` is empty, and this handoff, `AGENTS.md` and `CLAUDE.md` are unchanged since the planner's commit.
- Each file from a template still has its two source lines.

## 11. Done when

- All files in section 4 exist and meet section 6.
- The checks in section 10 pass.
- `dev/STATUS.md` and `dev/backlog.md` are updated.
- The implementation report is in `dev/plans/001-plan.md`, and the planner's implementation review says `approved`.
- On merge, Miguel checks whether the "unattributed changes" setting blocked it. If it did, he turns it off, updates `.github/rulesets/protect-main.json` in a follow-up, and logs why in `dev/tool-feedback.md`.

## 12. Out of scope

- The snapshot schema, rubrics, prompts and anchor examples (handoff 002).
- Miguel's blind labels (a frozen discovery record, committed before handoff 002).
- Any script, `pyproject.toml`, CI or dependency.
- `schemas/`, `rubrics/` and `examples/` folders: created by the handoffs that fill them.
- Changes to the principles repo or Knowledge Automata.
