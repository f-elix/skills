---
name: implement
description: Implement the agreed change, verify it, and iterate on code review until clean.
disable-model-invocation: true
---

Implement the work agreed upon with the user, or described by the user.

When the implementation is complete, run the project's typecheck, lint, and tests, then format changed files with the project's formatter. Fix failures and rerun until every check passes.

Once the checks pass and nothing else remains, run the `/code-review` skill in a fresh subagent and address its findings, rerunning the checks after each round of fixes. Repeat until a review returns no findings.
