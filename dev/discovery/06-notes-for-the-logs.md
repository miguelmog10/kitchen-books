# Notes for the logs

Entries from discovery, ready to move into `dev/tool-feedback.md` and `dev/principles-in-practice.md` once handoff 001 creates them. Edit the wording to fit the templates; the facts are checked.

## For `dev/tool-feedback.md`

| Date | Tool | What happened | What we do now |
| --- | --- | --- | --- |
| 2026-11-01 | Git and GitHub (principles repo) | The `v1.2.0` tag was created after switching back to `main` and before the release branch was merged. It pointed at the v1.1.0 commit (`75ccd6b`), so links to `v1.2.0` showed v1.1.0 and `field-notes/` gave a 404. | Merge first, then tag on `main`. The tag was moved to `d2c957d` the same day. |
| 2026-11-01 | GitHub rulesets | The tag ruleset blocked deleting the wrong tag (`GH013`). It was disabled for this one fix and turned back on; the CHANGELOG's Unreleased section records the exception. | Treat the rule as right: a published tag normally never moves. Exceptions are written down. |
| 2026-11-01 | `git ls-remote` | `git ls-remote --tags origin v1.2.0` lists only the tag object, not the commit it points to (the `v1.2.0^{}` line has a different name). | To check a tag's commit, run `git ls-remote --tags origin` without the name, and read the `^{}` line. |
| 2026-11-01 | GitHub Releases | Deleting the tag left no v1.2.0 draft, because no Release had been published for it, only the tag. A v1.2.0 Release was then created on the moved tag. | Tag and Release are separate steps; check both. |
| 2026-10-30 | Claude (claude.ai) linked to the computer | Every file written into this folder from claude.ai was read back and compared by SHA-256 (playbook rule 4). All matched. | Keep verifying writes that cross tools. Whether this is still needed inside Claude Code is an experiment. |
| 2026-11-01 | Claude (claude.ai) linked to the computer | Rewriting `01-concept-note.md` and `05-planner-primer.md` from the same paths delivered older versions: the concept note with only its header change, the primer unchanged. The read-back by hash caught it. Writing from a new path fixed it, and the hashes then matched. | Same failure as Knowledge Automata's handoff 003. Rule 4 is still needed for files that cross tools; when rewriting a file, write it from a new path. |
| 2026-11-01 | Claude (claude.ai) | The claude.ai session could not open GitHub web pages (not on its network allowlist), but `git ls-remote` worked and showed the tag problem. | Checking git refs directly is more reliable than reading web pages. |
| 2026-11-01 | Claude Code | Claude Code reads `AGENTS.md` on its own only when no `CLAUDE.md` exists (from v2.1.277). Any `CLAUDE.md` or `CLAUDE.local.md` on the path turns that off. | Commit a `CLAUDE.md` with only `@AGENTS.md`; check with `/memory`. See the principles repo's `field-notes/claude-code.md` at v1.2.0. |
| 2026-11-01 | Setup | Planner in the Claude desktop app (Code), implementer in the VS Code extension, both Claude Code, separate fresh sessions. | Keep this setup for the whole of v1 so the comparison with Knowledge Automata stays clean. |
| 2026-11-01 | Principles backlog | The v1.2.0 backlog lists a Copilot-instructions experiment for Kitchen Books, but Kitchen Books doesn't use Copilot. | Move that experiment to Knowledge Automata. |

## For `dev/principles-in-practice.md`

| Date | Principle | Entry |
| --- | --- | --- |
| 2026-11-01 | P8 Externalize memory | Discovery happened in a claude.ai project whose docs Claude Code cannot see. Before switching tools, the records were copied into `dev/discovery/`. Lesson: when work moves to a tool that only reads the repo, move the memory first. |
| 2026-11-01 | P1 Design before prompting | "Start from zero" restarted the process, not the design. Decisions made under P1 (records 01 to 04) were kept; redoing them would have wasted good work. |
| 2026-11-01 | P14 Version everything that matters | Pinning to a tag protects a project only if the tag is right. Candidate release checklist: merge the release PR, switch to `main` and pull, tag, push, check the tag's commit, publish the Release. Worth proposing to the principles backlog. |
| 2026-11-01 | P11 Build evidence | The release fix was verified against GitHub (`git ls-remote`), not against the local clone or a summary of the commands. |

## For the principles repo, when it fits

- **Release checklist** (from the tag mix-up above): a candidate for the principles backlog, with this project as the evidence.
- **Handoff size:** the principles backlog asks whether handoffs need a size limit. Food Cost's planning gate (counting intended scope from a handoff before any code exists) could one day answer that with a measure instead of a byte count.
