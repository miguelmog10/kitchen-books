> **Frozen discovery record.** Copied unchanged from the Kitchen Books project on claude.ai ("Kitchen Books: prior-art search", 2026-10-28). See [README.md](README.md).

# Kitchen Books: prior-art search

- **Author:** Miguel, with Claude
- **Date:** 2026-10-28
- **Status:** First pass, done. Revisit before v1 of the standard is published.
- **Question:** Does anything already compare the complexity an iteration adds with the scope it delivers?

## Short answer

Partly. One established standard measures the size of a change and splits it into functional and technical parts (OMG Automated Enhancement Points). Several tools track complexity on new code, and research shows LLMs can size requirements and check whether a PR matches its issue. Nothing found puts these together: **intended scope (from the ticket or handoff) compared with the complexity actually added, per PR, open and lightweight, built for agentic coding.** The gap is real but narrower than the concept note assumed, and the closest prior art gives us ideas to borrow.

## What exists

### Complexity on new code (tools)

| Tool | What it does | What it does not do |
|---|---|---|
| **SonarQube** | Cyclomatic and cognitive complexity, issues on new code (`new_` metrics), quality gates on PRs. Technical debt ratio = remediation cost / (cost per line × lines of code). | The ratio's denominator is code size, not delivered scope. A bloated PR with clean code passes. |
| **CodeScene** | Code Health (a 1 to 10 score), hotspots, PR delta checks. An MCP server lets AI agents read Code Health and follow guardrails. Its own research (Borg and Tornhill, 2026) reports at least 30% higher defect risk for AI-written code in unhealthy files. | Measures the health of the code before and after a change. Does not relate the change to the ticket or its intent. |
| **GitClear** | Diff Delta (durable change versus churn), duplication, moved code, refactoring share. Reports rising duplication and falling refactoring with AI adoption. | Measures output and churn, not output against scope. Its churn findings are disputed by an independent 151-repo study. |
| **CodeRabbit** | Per-PR "estimated complexity" and "estimated review effort" from an LLM. Issue validation checks a PR against its linked issue. | Scale and inputs are not documented. No comparison between how big the change is and how big the issue was. |
| **Delivery metrics platforms** (DX, LinearB and similar) | PR size, cycle time, throughput. | Track delivery, not the complexity-to-scope relationship. |

### Size of a change (standards)

**OMG Automated Enhancement Points (AEP)**, implemented by CAST, is the closest prior art. Between two versions it finds added, modified and deleted functions and weights them:

- **Functional part (AEFP):** changes to transactions and data functions, in function points.
- **Technical part (AETP):** changes to code outside any transaction, converted to function-point terms with an equivalence ratio.
- **Effort complexity** per changed object comes from McCabe complexity, lines of code, fan-in and SQL complexity.
- Deleted functions weigh 0.4, added functions 1.

What we can borrow: the **functional versus technical split**, a fixed weight for deletions, and the idea of a ratio learned from the app itself. What it does not do: AEP sizes the change to estimate effort and productivity. It reads functional size from code structure, so it never asks whether the change matched what was asked for. It is enterprise tooling, mostly for classic business stacks.

### Sizing scope with LLMs (research)

- **COSMIC sizing from requirements:** a hybrid of GPT-4 (finds functional processes and data groups) and a rule engine (derives data movements) measured functional size within 6.6% average error on three public case studies (Laqrichi, 2024). CosMet (De Vito et al., 2026) does the same from use cases and cut measurers' time by 60 to 80%. This is the "model proposes, code checks" pattern applied to scope.
- **Story points from issue text:** the best off-the-shelf LLMs reach a Spearman correlation of about 0.41 zero-shot and about 0.46 with five examples, averaged across 16 projects (Shetty et al., 2026). Better than a supervised baseline, but still noisy. Ranking works better than absolute numbers.
- **PR to issue alignment:** fine-tuned LLMs classify whether a PR matches its issue; the diff, the issue body and the PR body drive the prediction (Altunhan et al., 2026). This checks match, not proportion.

### Complexity as an LLM experiences it (research)

LM-CC (Xie et al., 2026) builds a complexity metric from a model's token entropy. After controlling for length, classic metrics (cyclomatic, Halstead, cognitive) showed no consistent link with LLM task performance; LM-CC did. Worth watching for a later account ("how hard is this code for agents"), not for v1.

## What this changes for Kitchen Books

1. **Restate the gap.** Not "nobody normalizes complexity", but: nobody compares **intended** scope with **added** complexity per PR, in an open format, with the evidence kept for reviewers and AI architects.
2. **Measure scope as functional size, not story points.** COSMIC-style counting (data movements per functional process) is a standard, can be counted from both the ticket and the diff, gives zero for refactors and negative values for removals, and LLM-plus-rules results are already good. Story-point estimates are too noisy to anchor the index.
3. **"Interactions added" belongs on the scope side.** In COSMIC terms, interactions are data movements, which is functional size. Counting them as complexity would double-count scope.
4. **Borrow from AEP:** keep a functional and a technical view of each change, and give deletions a reduced but non-zero weight.
5. **Cite CodeScene's 2026 research** in the "Why now" section: it is independent support for measuring health on AI-written changes, though it comes from the vendor.

## Not yet checked

- Newer AI review tools (Qodo, Greptile, Graphite) did not show a scope-proportionality feature in a quick search. Confirm from their docs before publishing.
- The CodeScene paper itself (arXiv 2601.02200), beyond its press release.
- The exact alignment categories in Altunhan et al.

## Sources

- [SonarQube: metrics definitions](https://docs.sonarsource.com/sonarqube-server/2025.2/user-guide/code-metrics/metrics-definition)
- [CodeScene MCP server](https://codescene.io/docs/developer-tools/mcp/codescene-mcp-server.html)
- [Borg and Tornhill: AI coding assistants and code health, press release, Jan 2026](https://tools.prnewswire.com/en-us/live/20813/release/20260128EN71904)
- [GitClear research summary](https://rywalker.com/research/gitclear)
- [CodeRabbit dashboard metrics](https://docs.coderabbit.ai/guides/dashboard-metrics)
- [CodeRabbit issue validation](https://docs.coderabbit.ai/changelog/issue-validation)
- [CAST: OMG Automated Enhancement Points (AEP)](https://doc.castsoftware.com/export-v2/aipcore/aip-core-technical-information/cast-omg-compliant-automated-function-points/cast-automated-enhancement-points-estimation-aep/)
- [Laqrichi: hybrid LLM and rule-based COSMIC measurement, IWSM-Mensura 2024](https://ceur-ws.org/Vol-3852/paper9.pdf)
- [De Vito et al.: LLM-based COSMIC sizing from use cases (CosMet), 2026](https://cosmic-sizing.org/2026/04/30/31737/)
- [Shetty et al.: story point estimation using LLMs, 2026](https://arxiv.org/html/2603.06276v1)
- [Altunhan et al.: classifying PR-issue alignment with fine-tuned LLMs, 2026](https://arxiv.org/abs/2609.01087v1)
- [Xie et al.: rethinking code complexity through LLMs (LM-CC), 2026](https://arxiv.org/html/2602.07882v1)
