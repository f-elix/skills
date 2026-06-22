---
name: grill-with-code
description: A grilling session that sharpens a plan by writing additive code artifacts as decisions crystallize.
disable-model-invocation: true
---

Interview the user relentlessly about every aspect of this plan until we reach a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. For each question, provide your recommended answer.

Ask the questions one at a time, waiting for feedback on each question before continuing.

If a question can be answered by exploring the codebase, explore the codebase instead.

## Core behavior

Write as you go.

As soon as an API shape, type, interface, module boundary, file boundary, or folder structure is clear from the discussion, create it in code instead of waiting for the whole design to be finished.

Questions stay primary.

Assume the user may be writing code alongside you during the session.

Before editing a file that may have changed recently, reread it. Do not assume you are the only writer, and do not overwrite fresh user changes just because they differ from your last plan.

## Scope of allowed code changes

Default to additive work only.

Allowed:

- create new files and folders
- scaffold parallel implementations next to existing code
- define interfaces, types, schemas, protocols, and public contracts
- add empty functions, methods, classes, and modules
- add focused tests for newly created code only
- add brief comments or TODOs where behavior is intentionally deferred

Not allowed by default:

- destructive edits
- replacing or deleting existing code
- updating existing call sites
- broad refactors through the existing codebase
- renames or migrations that mutate current implementations in place

If touching an existing file would help with integration, ask first.

## Quality bar while grilling

Prefer compilable scaffolding, but do not get bogged down in repository cleanup.

- Keep newly written code internally coherent
- Use stubs such as `throw new Error("Not implemented")` when needed
- Run only quick local checks when cheap and useful
- Ignore unrelated failing tests, pre-existing type errors, and obscure repo-wide failures unless they block the next design step

If a simple issue in code you just wrote is easy to fix, fix it. Otherwise, note it and keep moving.

## Handling uncertainty

Be explicit about what is decided versus deferred.

When behavior is unresolved but the contract is settled, scaffold the contract and mark the open question with a brief note such as:

- `TODO: decide retry policy`
- `TODO: define failure semantics`
- `// contract agreed, implementation pending`

Do not let placeholder code imply that an unresolved behavior has already been designed.

## Implementation boundary

Default to narrow scope.

Write only the code needed to embody the decision that was just made. Prefer stubbing outward-facing boundaries over propagating the design broadly.

You may continue past stubs into implementation only when all three are true:

1. the public contract is explicit
2. the behavior follows directly from existing patterns or a resolved decision, with no meaningful ambiguity left
3. implementation can remain additive under these safety rules

## Naming and placement

When creating a parallel implementation, colocate it near the current implementation when practical and choose names contextually so the new work is easy to compare with the old.

Avoid names that hide uncertainty. Prefer names that signal a real variant, draft, or next-step design.
