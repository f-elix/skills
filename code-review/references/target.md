# Pinning the target

## GitHub PR

Resolve the repository and PR number from the user's URL/number or current branch. Inspect metadata, body, files, commits, and diff using `gh pr view` and `gh pr diff`, or equivalent read-only GitHub tools. Obtain exact base/head SHAs from the PR API, which exposes `base.sha` and `head.sha`:

```sh
gh api repos/OWNER/REPO/pulls/NUMBER
```

Fetch matching objects without switching the user's branch. A PR head can be fetched through `refs/pull/NUMBER/head` from the target repository, including fork PRs. Fetch the actual base branch too. Verify fetched objects match the captured SHAs before using them. If the PR changed during capture, refresh the metadata and snapshot; a stale local ref is not evidence.

Use `git merge-base BASE_SHA HEAD_SHA` and `git diff BASE_SHA...HEAD_SHA` to capture the review diff, plus `git log BASE_SHA..HEAD_SHA` for intent. Compare with the GitHub diff; investigate disagreements before dispatch. Use the PR's actual base, including stacked feature branches, rather than assuming `main`. Keep the recorded SHAs fixed throughout a pass.

Read source with `git show HEAD_SHA:path` and search it with `git grep -n PATTERN HEAD_SHA`. Use the matching merge-base for introduction/inheritance comparisons. Read surrounding code, callers, tests, and siblings from the same snapshot. The user's checkout may be another branch or contain unrelated local work.

Inspect existing review threads/bodies only as coordinator, to avoid redelivering the same issue. Existing feedback is not proof; verify any relevant claim yourself. Requirements explicitly supplied by the user remain available to reviewers.

For execution, use an isolated temporary checkout at HEAD_SHA with the required project dependencies and test configuration. Respect applicable project instructions for checkout creation. If execution requires unavailable setup, record the gap and continue source tracing. Never run a migration or alter live/shared data merely to validate review feedback.

## Committed branch changes

Resolve the requested baseline and HEAD to SHAs, capture the merge-base and `git diff BASE_SHA...HEAD_SHA`, and read those objects throughout review. Ask for a baseline if it cannot be inferred from the request. Tell the user whether uncommitted work was excluded.

## Local uncommitted changes

Default "local changes" to the final working contents relative to pinned HEAD. Include staged and unstaged edits, additions, deletions, and renames, plus relevant untracked source, test, and documentation files. Enumerate status and untracked files explicitly; an empty tracked diff does not mean the review is empty.

```sh
git status --short
git diff --binary --full-index HEAD
git ls-files --others --exclude-standard
```

Capture the patch, included untracked file contents, and matching surrounding source into a temporary snapshot. For the default scope, materialize pinned HEAD outside the user's checkout, apply the captured tracked patch there, then copy the included untracked files at their relative paths. Preserve deletions and file modes. Record HEAD, the chosen scope, included/excluded files, and snapshot identity in a manifest. Avoid unrelated ignored files and credentials. A local-only review has no new commit list; say so rather than seeking a PR.

Use the captured snapshot for both source reading and execution. Give reviewers snapshot paths and the captured patch, rather than a live diff command that can change during review. HEAD-only source reads omit local edits and cannot verify this target. Keep the user's files, staging choices, and branch unchanged.

### Requested subsets

| Scope | Diff and matching source |
|---|---|
| Staged only | `git diff --cached --binary --full-index HEAD`; source from the captured index, including staged additions and deletions |
| Unstaged only | `git diff --binary --full-index`; baseline is the captured index, reviewed source is that index plus the unstaged patch |
| Committed branch changes plus local work | Resolve the requested ref and HEAD, pin their merge-base, then use `git diff --binary --full-index MERGE_BASE_SHA` and included untracked files |

For staged-only review, materialize HEAD plus the captured staged patch; do not substitute working-file contents that may contain further unstaged edits. For unstaged-only review, materialize the captured index first, then apply the unstaged patch; staged changes are surrounding context, not findings introduced by that patch. Include untracked files in a subset only when the user requests them. Record excluded subsets in the briefing.

For combined branch/local scope, the snapshot represents final working contents and the comparison baseline is the pinned merge-base, not the latest base-branch tip. This includes the branch's committed work and local edits without treating unrelated base-branch commits as changes to review.

If files or the index change during capture, recapture before dispatch. Before concluding, confirm the relevant local contents and HEAD still match; refresh or explicitly report changes that were not reviewed.

## Anchors

For PR findings, prefer a link to the pinned GitHub source line or a verified PR diff link. For an isolated checkout or working snapshot, use an absolute local file link. Confirm line numbers against the reviewed contents; use the deletion side when the issue is removed code. Distinguish the changed anchor from supporting callers or baseline lines.
