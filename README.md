# Kitchen Books

**Complexity accounting for codebases.** Kitchen Books is an open standard for tracking how a codebase's complexity grows compared with the scope it delivers, one merged pull request at a time.

Growth that matches scope is fine. Growth far beyond scope is a warning: features that were never removed, over-built changes, or agents that generate more than the task needed. Kitchen Books keeps the books, so that warning shows up on every pull request instead of years later.

Each metric is an account in the books. The first is Food Cost (complexity versus scope account). It keeps two indexes, complexity change and scope change, each compared with the repo's own history, and never divides one by the other. Scope is counted as COSMIC functional size; complexity comes from existing tools such as Lizard. Read the [overview](standard/overview.md) and the [Food Cost account](standard/accounts/food-cost.md).

It is for anyone who writes code, from a solo developer to a team, and for the AI reviewers that read its snapshots.

## Why now

1. **AI lowered two barriers, not one.** It lowered the barrier to implementation, so we can generate far more code than before. It also lowered the barrier to proper engineering: tracking complexity used to be set aside because there was never time, and AI makes it cheap enough to do. Agentic coding often over-builds, and a complexity spike on a change planned as small shows up before anyone reads the diff.
2. **People love adding, not deleting.** A new feature is exciting and removing one rarely is, so codebases accumulate. A visible trend line makes us step back before we add more, and rewards cleanup, because the index goes down.

The full list is in the [overview](standard/overview.md#why-now).

## Status

Standard v0.1, draft. Not yet tested on a real project; the first test will be [Knowledge Automata](https://github.com/miguelmog10/knowledge-automata). It is not a library, a watcher, a GitHub Action or a hosted dashboard. For where the work is right now, see [`dev/STATUS.md`](dev/STATUS.md).

## Map of the repo

| Folder | What it holds |
| --- | --- |
| [`standard/`](standard/overview.md) | The product: the standard itself, its overview and its accounts |
| [`docs/decisions/`](docs/decisions/README.md) | Why it is built this way (ADRs) |
| [`dev/`](dev/STATUS.md) | How it is built: status, backlog, handoffs, plans, running logs, costs, and the frozen discovery records |

## How it is built

Miguel owns the project and approves every plan and merge. Two Claude Code sessions do the work, each with its own context:

- **Planner-reviewer** (desktop app): plans each handoff with Miguel, then reviews the implementer's plan and the actual changes.
- **Implementer** (VS Code extension, a fresh session per handoff): writes a plan, waits for approval, builds in small commits and reports.

They exchange plans, questions, reports and reviews through files in [`dev/plans/`](dev/plans/), so the whole trail stays in the repo. The rules are in [`AGENTS.md`](AGENTS.md). The process follows the [AI-Assisted Software Engineering](https://github.com/miguelmog10/ai-assisted-software-engineering/blob/v1.2.0/PRINCIPLES.md) principles and their [playbook](https://github.com/miguelmog10/ai-assisted-software-engineering/blob/v1.2.0/PLAYBOOK.md), v1.2.0.

## License

[Apache 2.0](LICENSE) for everything in the repo ([ADR-0001](docs/decisions/0001-license-apache-2.md)).
