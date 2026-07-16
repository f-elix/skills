---
name: review-delegate
description: Delegates a PR's first review pass across design, code quality, consistency, and change risk. Use when reviewing pull requests or deciding where human review attention matters most.
---

# Review Delegate

Use this skill to offload the tedious first pass of PR review while keeping the user in the review loop.

## User Input

- GitHub PR URL or number, or an open PR associated with the current branch.
- Optional scope: one or more persona names, `focused`, or `full`.

## Coordinator Workflow

1. Resolve the PR from a supplied GitHub PR URL or number, or from the current branch's open PR.
2. If no PR can be resolved, or multiple PRs are possible, ask the user for a PR URL or number.
3. Fetch PR metadata with `gh pr view --json title,body,files,commits,baseRefName,headRefName,url` and fetch the diff with `gh pr diff`.
4. Read applicable project guidance such as `AGENTS.md`, `CLAUDE.md`, `CONTEXT.md`, ADRs, and docs relevant to changed files.
5. Classify changed files and select personas:
   - Treat only generated files, lockfiles, snapshots, formatting-only churn, pure moves, and repeated call-site updates as mechanical.
   - Keep entry points, public contracts, schemas, migrations, permissions, persistence, error handling, config defaults, and tests in review scope.
   - Named personas: run exactly those.
   - `focused`: run the one or two personas most relevant to the material changes.
   - `full` or no supplied scope: run all personas.
6. Use the PR diff as primary evidence. Expand read-only source context only for selected personas where the diff is insufficient. Use PR head and optionally base.
7. Spawn the selected personas.
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

Speak in plain human language, with no jargon. Write simply and concisely, like one human talking to another.

Final output order:

1. `PR Summary`
2. `Findings`
3. `Code Quality Notes` when `code-quality` runs
4. `Human Review Route`

### PR Summary

- Explain what the PR changes.
- Identify the most important design decisions.
- Identify the most important readability, organization, and changeability concerns.
- Name the complexity introduced by the PR.
- Name the abstractions added or changed.
- Separate design-important and code-quality-important files from mostly mechanical files.
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
- Ground each finding in what changed in the code and why that change causes the issue, not only a broad category such as "complexity" or "maintainability".
- Explain why each finding matters under the engineering philosophy.
- Include concrete code-quality findings when readability, organization, naming, locality, test shape, or failure behavior materially affects review quality.
- Distinguish must-fix issues from judgment calls.
- Keep summaries and findings concise. Prefer direct bullets over explanation-heavy prose.
- If all personas return no high-signal findings, the coordinator should say `No major findings` and still provide the PR summary and Human Review Route.
- Do not impose a fixed maximum number of findings; include every high-signal finding that materially affects review quality.
- Do not prominently attribute every final finding to personas.
- Mention supporting personas only when it improves confidence or explains severity, such as `supported by Design + Code Quality`.

### Code Quality Notes

Include this section only when `code-quality` runs.

- Include detailed readability and cleanup notes that are too small for `Findings` but still useful for review.
- Keep each note concrete: name the line or small code shape, why it hurts readability, and the suggested change.
- Prefer notes about names, comments, dead code, duplicate code, parameter shape, local simplification, existing helpers, file placement, and local idioms.
- Include small nitpicks when the fix is obvious and the comment would help the author polish the PR.
- Do not include personal taste, formatting-only issues, or anything a formatter/linter should catch.
- If there are no useful notes, write `No code quality notes`.

## Final Output Template

```md
## PR Summary
- [What changed]
- Key decisions: [short list]
- Complexity/abstractions: [short list]

## Findings
- [Severity] [Title] (`path:line`)
  Evidence: [what changed in the code, or a quote]
  Why it matters: [one sentence]
  Human review: [what to inspect or decide]

<!-- Include only when code-quality runs. -->
## Code Quality Notes
- `path:line`: [specific readability or cleanup note]. Suggestion: [short fix]

## Human Review Route
- `path/or/flow`: [why it matters]. Question: [what to decide]
```

## Boundaries

- Do not post GitHub review comments automatically.
- Do not approve, request changes, merge, or mutate the PR.
- Do not treat the synthesized review as a replacement for human review.
- Prefer fewer, higher-signal `Findings`. `Code Quality Notes` may be more detailed when the notes are concrete and useful.
- Do not fetch or pass existing PR review comments, review threads, or discussion comments to subagents.
- Do not run local checks, tests, linters, typecheckers, builds, or repository commands other than GitHub PR metadata, diff fetching, and read-only source context for selected hotspots.

## Personas

- `design`: [Design Reviewer](personas/design-reviewer.md)
- `code-quality`: [Code Quality Reviewer](personas/code-quality-reviewer.md)
- `consistency`: [Consistency Reviewer](personas/consistency-reviewer.md)
- `risk`: [Change Risk Reviewer](personas/change-risk-reviewer.md)

In `focused` scope, select by each persona's documented Focus. Prefer `risk` when the PR changes behavior, data, permissions, configuration, or public contracts.
