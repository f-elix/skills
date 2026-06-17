---
name: code-quality-review
description: Coordinates opinionated PR code reviews by fetching GitHub PR context, running focused reviewer personas, and synthesizing ranked findings. Use when reviewing pull requests, evaluating PR quality, or deciding which parts of a change deserve human review attention.
---

# Code Quality Review

Use this skill to offload the tedious first pass of PR review while keeping the user in the review loop.

## User Input

- GitHub PR URL or number, or an open PR associated with the current branch.

## Coordinator Workflow

1. Resolve the PR from a supplied GitHub PR URL or number, or from the current branch's open PR.
2. If no PR can be resolved, or multiple PRs are possible, ask the user for a PR URL or number.
3. Fetch PR metadata with `gh pr view --json title,body,files,commits,baseRefName,headRefName,url` and fetch the diff with `gh pr diff`.
4. Read applicable project guidance such as `AGENTS.md`, `CLAUDE.md`, `CONTEXT.md`, ADRs, and docs relevant to changed files.
5. Classify changed files into review hotspots, architecturally important files, persona-relevant files, and mostly mechanical files.
6. Expand read-only source context only for selected hotspots where the diff is insufficient. Use PR head and optionally base.
7. Spawn every persona listed in Persona Set. File triage controls context size, not whether a persona runs.
8. Collect persona findings using the [review finding format](review-finding-format.md), not final user-facing output.
9. Deduplicate and merge overlapping findings, keeping persona attribution as internal evidence.
10. Assign final severity, then rank findings by severity, confidence, and expected review value.
11. Produce a human review briefing that directs the user's attention.

## Subagent Invocation

For each persona, pass only dynamic PR context:

- PR summary.
- Applicable repository constraints.
- Relevant diff chunks, selected source excerpts, and selected file list.
- Omitted mechanical-file summary.

Do not restate static reviewer instructions. Tell each subagent to read and follow its persona file in [personas/](personas/).

## Required Output

Final output order:

1. `PR Summary`
2. `Findings`
3. `Human Review Route`
4. `File Triage`

### File Triage

- List the files and diff chunks sent to each persona.
- List mostly mechanical files omitted from focused persona review.
- Explain why each high-priority file deserves human attention.
- Treat files as mostly mechanical only when they are generated files, lockfiles, snapshots, formatting-only churn, bulk renames or moves with no meaningful logic change, or repeated call-site updates caused by a reviewed API change.
- Never omit entry points, public contracts, schemas, migrations, auth or permission code, persistence code, error handling, config defaults, or tests for new behavior.
- Use the GitHub PR diff as the primary review evidence.
- Treat review hotspots as the highest-priority files, flows, or diff sections for human attention.
- Expand only selected hotspots with read-only source context from the PR head, and optionally base, when the diff alone is insufficient.
- Treat changed tests as review evidence for intended behavior, risk, and coverage gaps.
- Send relevant test diffs especially to Change Risk, Maintainability, and Consistency reviewers.

### PR Summary

- Explain what the PR changes.
- Identify the most important design decisions.
- Name the complexity introduced by the PR.
- Name the abstractions added or changed.
- Separate architecturally important files from mostly mechanical files.
- Rank files worth deep human review by importance.

### Human Review Route

- List the top files or flows the user should inspect manually, in order.
- Explain why each file or flow matters in one concise phrase.
- State the main question the user should answer while reviewing it.
- Keep this section short and oriented toward directing human attention.

### Findings

- Within `Findings`, order issues by severity.
- Use severity definitions from the [review finding format](review-finding-format.md).
- Treat persona severity as a recommendation; the coordinator owns final severity after deduplication.
- Include file and line references when available.
- Explain why each finding matters under the engineering philosophy.
- Distinguish must-fix issues from judgment calls.
- Keep summaries and findings concise. Prefer direct bullets over explanation-heavy prose.
- If all personas return no high-signal findings, the coordinator should say `No major findings` and still provide the PR summary and Human Review Route.
- Do not impose a fixed maximum number of findings; include every high-signal finding that materially affects review quality.
- Do not prominently attribute every final finding to personas.
- Mention supporting personas only when it improves confidence or explains severity, such as `supported by Architecture + Maintainability`.

## Final Output Template

```md
## PR Summary
- [What changed]
- Key decisions: [short list]
- Complexity/abstractions: [short list]

## Findings
- [Severity] [Title] (`path:line`)
  Why it matters: [one sentence]
  Human review: [what to inspect or decide]

## Human Review Route
- `path/or/flow`: [why it matters]. Question: [what to decide]

## File Triage
- High-priority: [files/flows]
- Mechanical/omitted: [files grouped by reason]
- Context expanded: [hotspot excerpts used, if any]
```

## Boundaries

- Do not post GitHub review comments automatically.
- Do not approve, request changes, merge, or mutate the PR.
- Do not treat the synthesized review as a replacement for human review.
- Prefer fewer, higher-signal findings over exhaustive commentary.
- Do not fetch or pass existing PR review comments, review threads, or discussion comments to subagents.
- Do not run local checks, tests, linters, typecheckers, builds, or repository commands other than GitHub PR metadata, diff fetching, and read-only source context for selected hotspots.

## Persona Set

Spawn all five personas for every review.

- [Architecture Reviewer](personas/architecture-reviewer.md)
- [Complexity Reviewer](personas/complexity-reviewer.md)
- [Consistency Reviewer](personas/consistency-reviewer.md)
- [Maintainability Reviewer](personas/maintainability-reviewer.md)
- [Change Risk Reviewer](personas/change-risk-reviewer.md)
