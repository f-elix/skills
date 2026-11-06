---
name: implement
description: Run when the user wants you to implement some work.
disable-model-invocation: false
---

Start a new agent session to implement the work agreed upon with the user, or described by the user. The new session starts without this conversation's context, so hand it a self-contained brief: the goal, agreed decisions and constraints, paths to any `spec.md` and `implementation-plan.md` (temporary notes: never commit them or include them in the PR), and the rest of these instructions. Do not implement in the current session.

The implementer session follows these steps:

1. Implement the change.
2. Run the project's typecheck, lint, and tests, then format changed files with the project's formatter. Fix failures and rerun until every check passes.
3. Once the checks pass and nothing else remains, run the `/review-your-work` skill. Rerun the checks after each round of fixes.
