<!--
From AI-Assisted Software Engineering templates v1.2.0 (CC0 1.0):
https://github.com/miguelmog10/ai-assisted-software-engineering/tree/v1.2.0/templates
-->

# Tool feedback

Notes on tools, SDKs and documentation used while building, so useful feedback is not lost and can later become upstream issues, pull requests or posts.

Entry format: date, tool, type (confusing, missing, bug, delight, setup), note, suggested improvement. The `setup` type is added in this project: it records how the tools are set up, the baseline that later entries compare against.

<!--
### YYYY-MM-DD: Tool name (type)

Note: what happened.
Suggested improvement: what would have helped, and any workaround that worked.
-->

## Entries

### 2026-10-30: Claude (claude.ai) linked to the computer (delight)

Note: every file written into the repo folder from claude.ai was read back and compared by SHA-256 (playbook rule 4). All matched.
Suggested improvement: keep verifying writes that cross tools. Whether this is still needed inside Claude Code is an experiment.

### 2026-11-01: Git and GitHub, principles repo (confusing)

Note: the `v1.2.0` tag was created after switching back to `main` and before the release branch was merged. It pointed at the v1.1.0 commit (`75ccd6b`), so links to `v1.2.0` showed v1.1.0 and `field-notes/` gave a 404.
Suggested improvement: merge first, then tag on `main`. The tag was moved to `d2c957d` the same day.

### 2026-11-01: GitHub rulesets (delight)

Note: the tag ruleset blocked deleting the wrong tag (`GH013`). It was disabled for this one fix and turned back on; the principles repo's CHANGELOG, in its Unreleased section, records the exception.
Suggested improvement: treat the rule as right: a published tag normally never moves. Exceptions are written down.

### 2026-11-01: `git ls-remote` (confusing)

Note: `git ls-remote --tags origin v1.2.0` lists only the tag object, not the commit it points to (the `v1.2.0^{}` line has a different name).
Suggested improvement: to check a tag's commit, run `git ls-remote --tags origin` without the name, and read the `^{}` line.

### 2026-11-01: GitHub Releases (confusing)

Note: deleting the tag left no v1.2.0 draft, because no Release had been published for it, only the tag. A v1.2.0 Release was then created on the moved tag.
Suggested improvement: tag and Release are separate steps; check both.

### 2026-11-01: Claude (claude.ai) linked to the computer (bug)

Note: rewriting `01-concept-note.md` and `05-planner-primer.md` from the same paths delivered older versions: the concept note with only its header change, the primer unchanged. The read-back by hash caught it. Writing from a new path fixed it, and the hashes then matched. Same failure as Knowledge Automata's handoff 003.
Suggested improvement: rule 4 is still needed for files that cross tools. When rewriting a file, write it from a new path.

### 2026-11-01: Claude (claude.ai) (missing)

Note: the claude.ai session could not open GitHub web pages (not on its network allowlist), but `git ls-remote` worked and showed the tag problem.
Suggested improvement: checking git refs directly is more reliable than reading web pages.

### 2026-11-01: Claude Code (confusing)

Note: Claude Code reads `AGENTS.md` on its own only when no `CLAUDE.md` exists (from v2.1.277). Any `CLAUDE.md` or `CLAUDE.local.md` on the path turns that off.
Suggested improvement: commit a `CLAUDE.md` with only `@AGENTS.md`, and check with `/memory`. See the principles repo's [`field-notes/claude-code.md`](https://github.com/miguelmog10/ai-assisted-software-engineering/blob/v1.2.0/field-notes/claude-code.md) at v1.2.0.

### 2026-11-01: Claude Code, desktop app and VS Code extension (setup)

Note: the planner runs in the Claude desktop app (Code), the implementer in the VS Code extension, both Claude Code, in separate fresh sessions.
Suggested improvement: keep this setup for the whole of v1, so the comparison with Knowledge Automata stays clean.

### 2026-11-01: AI-Assisted Software Engineering backlog (bug)

Note: the v1.2.0 backlog lists a Copilot-instructions experiment for Kitchen Books, but Kitchen Books doesn't use Copilot.
Suggested improvement: move that experiment to Knowledge Automata.

### 2026-11-01: Claude Code auto-memory (confusing)

Note: auto-memory was on by default. The planner session wrote its time log there instead of in the repo. `/context` showed it loaded in a fresh session in the same folder, so the implementer would have started with the planner's notes.
Suggested improvement: turned off per repo with `.claude/settings.json` (`"autoMemoryEnabled": false`). A setting enforces what a rule in `AGENTS.md` could only ask for.

### 2026-11-01: Git Bash on Windows, in Claude Code (confusing)

Note: while checking this handoff's files for em dashes, `grep -P '\x{2014}'` failed with "grep: -P supports only unibyte and UTF-8 locales": the Bash tool's shell does not run in a UTF-8 locale by default.
Suggested improvement: set the locale for the command and match the bytes, for example `LC_ALL=C.UTF-8 grep -c $'\xe2\x80\x94' file`, which worked.
