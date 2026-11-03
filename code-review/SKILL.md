---
name: code-review
description: Review GitHub PRs, committed branch changes, or local staged, unstaged, and untracked source changes through independent Standards and Spec reviewers, plus a conditional Design reviewer. Verify findings against a fixed snapshot and report evidence, tradeoffs, and where human review matters. Produces a briefing without posting comments or approving PRs.
license: MIT; see LICENSE
---

# Code review

Review through separate questions: does the implementation follow the standards, does it satisfy the requirements, and is the chosen design appropriate? Evidence for one question cannot substitute for another.

Adapted from [Matt Pocock's code-review skill](https://github.com/mattpocock/skills/blob/d81f3a183412e71a5b1e84ca21bc1a35eea03a60/skills/engineering/code-review/SKILL.md), under the included MIT license. Evidence checks draw on the user's *Evidence-first PR review* artifact. The Design rubric adapts the engineering philosophy in their `review-delegate` skill and John Ousterhout's *A Philosophy of Software Design*.

## Scope

Produce a briefing in chat. Keep the reviewed code unchanged. Git fetches, temporary review checkouts, and targeted verification are permitted; PR comments, pending reviews, approvals, thread resolution, commits, pushes, and merges are outside this skill's scope. Keep `review-delegate` separate.

Select the target from the user's request:

| Request | Review target |
|---|---|
| PR URL/number, or "review the current PR" | The PR's pinned base/head; resolve the current branch's open PR when needed |
| "Review since REF" or "review this branch against REF" | Committed branch changes from that ref's merge-base to HEAD |
| "Review my local changes" or "review my uncommitted changes" | Staged and unstaged changes plus relevant untracked source files, against HEAD |
| "Review staged changes" or "review unstaged changes" | Only the requested local subset |

Honor an explicit combined scope, such as branch changes since a ref plus uncommitted work. For `$code-review` without a target, use the local uncommitted scope when it contains reviewable changes; otherwise ask which PR or baseline to review. State the selected scope before dispatch. Ask only when the target is ambiguous or unavailable, and name any user-imposed coverage gaps. Local review does not require an open PR.

## 1. Pin the review target

Follow [references/target.md](references/target.md) for PRs, stacked PRs, committed branch changes, or working changes. Capture an immutable diff and matching source context before dispatching reviewers. Record the repository, base SHA, head SHA or working snapshot, merge-base, commit list, and scope.

Read the full diff, paging through truncation. Stop on an invalid baseline or empty diff. If deep coverage is impractical, prioritize permissions, data changes, error paths, public contracts, and new abstractions; identify the files not deeply reviewed. Generated files, lockfiles, and repeated call-site edits may receive lighter attention, but contracts, schemas, migrations, configuration, and tests remain in scope.

## 2. Gather requirements and guidance

Find the spec from an explicit user source, linked issues/tickets in PR metadata or commits, then relevant requirement documents. Use the configured issue tracker or available connector; no setup skill is required. If none is available, ask for the source while continuing independent review work. Skip Spec if the user says there is none, or access remains unavailable; distinguish those reasons in the report. Treat the PR description as intent, not an independent spec unless the user designates it as such.

Read applicable `AGENTS.md`, `CLAUDE.md`, conventions, ADRs, and domain guidance. Follow their skill-loading requirements. List relevant standards sources for Standards; load complete applicable documents rather than relying on titles. Keep documented requirements, design constraints, and author claims distinguishable. Existing review comments can help the coordinator avoid repeating delivered feedback, but do not pass them to reviewers.

## 3. Select and dispatch independent reviewers

Run Standards. Run Spec when a requirement source is available. Run Design when the diff introduces or changes meaningful ownership, module boundaries, public contracts, persistence, shared caller responsibilities, or dependencies. Examples include a new service/table, duplicated derived state, a changed API requiring caller coordination, or a new dependency with architectural consequences. There is no line-count minimum. Mechanical dependency bumps and moves do not trigger Design by themselves. Record the reason for running or skipping it.

Use parallel subagents when available. Each reads its brief from this skill directory. Create them with fresh context, such as `fork_turns: "none"` in Codex, and pass only the inputs below. Resolve brief and source paths to absolute paths before dispatch. Use the current model by default. If independent agents are unavailable, perform separate passes and disclose that limitation.

| Reviewer | Static brief | Dynamic inputs |
|---|---|---|
| Standards | [briefs/standards.md](briefs/standards.md) | Target, diff access, commit list, standards paths, explicit scope |
| Spec | [briefs/spec.md](briefs/spec.md) | Target, diff access, commit list, spec source/content, author-stated omissions, explicit scope |
| Design | [briefs/design.md](briefs/design.md) | Target, full diff access, commit list, requirements, applicable design constraints/ADRs, explicit scope |

All reviewers receive the exact source-reading instructions for the snapshot under review. Keep coordinator summaries, suspected findings, settled conclusions, and other reviewers' output out of their inputs. Design may read original commits and requirements to distinguish intent from constraints; those sources do not establish that the chosen design is correct.

## 4. Review and verify as coordinator

While agents work, inspect the core behavior yourself. Own regression and operational risk coverage even when Spec is skipped: permissions, state transitions, concurrency, caching, retries, persistence, migrations, compatibility, changed defaults, rollout/rollback, and tests for plausible failures.

Read [references/evidence-standard.md](references/evidence-standard.md). Apply its falsification checks to your own findings and independently verify each reviewer finding. A subagent's confidence or claimed proof is a candidate for verification, not your proof. Resolve disagreements through pinned source or a targeted check.

Run the smallest useful existing test, reproduction, type check, or live UI inspection when it can settle a claim. Use the captured review snapshot and repository test rules. Keep temporary reproductions outside the user's checkout; do not add a permanent test or alter shared application data as part of review. For visual/timing claims, observe the relevant state live or run a suitable test before describing the failure as observed; classify source-only concerns using the evidence standard. Report unavailable previews, authentication, fixtures, or checks without implying success.

Classify each finding using the evidence standard's proof labels, including `code reading only`. Record what each check actually proves, including whether a proposed alternative was only inspected or compiled, and qualify inferred consequences. Drop disproved findings; retain supported concerns at their actual evidence level and put questions with unchecked factual premises in coverage limits. Findings inherited from the baseline are context, unless the PR creates a new exposure or materially amplifies them.

Before finishing a PR review, confirm the PR still has the pinned base/head. If either moved, refresh and revalidate against the new target, including the new diff. If it keeps moving, report the exact reviewed SHAs and that later changes remain unreviewed.

## 5. Deliver the briefing

Use concise prose, code anchors, and the finding format in the evidence standard. Include:

1. **Summary:** what changed, the reviewed target, important decisions, and review scope.
2. **Coordinator findings:** supported behavior/operational concerns from your own pass, if any, with their proof labels.
3. **Standards**, **Spec**, **Design:** supported findings under their own headings, with their proof labels. Keep reviewer substance intact; correct unsupported claims and proof labels before presenting them. Rank within each section, never collapse the axes into one ranking. Say when a reviewer found no supported findings, skipped, or could not complete its pass.
4. **Verification and limits:** checks and outcomes, unverified questions, and material coverage gaps. Avoid listing every suspicion you disproved.
5. **Human review route:** a short ordered list of files or flows, why each matters, and the decision or question the user should assess.

When two axes identify the same issue, explain it fully under the axis that owns the consequence and cross-reference it under the other. Preserve both assessments without repeating the request. Separate must-fix findings from design tradeoffs. Do not invent findings to populate an empty section or infer approval from empty reports.
