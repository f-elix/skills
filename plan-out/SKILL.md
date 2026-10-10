---
name: plan-out
description: Collaboratively design a code change and record agreed spec decisions and an implementation plan.
disable-model-invocation: true
---

Collaborate with the user to design the implementation of a feature, bug fix, refactor, or other code change. Inspect relevant code, offer your perspective and recommendation, and compare meaningful alternatives. Ask focused questions, wait for the user's perspective, and revise the design together. Read `/software-design-philosophy` as a design guide and use `/show-me` for visual representation. Focus on callstacks, interfaces, types, APIs, inputs and outputs. Show diffs where applicable. Keep text sparse, concise, precise, and meaningful. Stay in planning until the user asks for implementation.

As decisions settle, record agreed business rules and expected behavior in `spec.md`, and the implementation plan in `implementation-plan.md`, under a task-specific directory following repository conventions. Keep both current as the discussion evolves. Use short bullets; capture decisions and necessary rationale in the spec, and concrete ordered changes and verification in the plan. Mark open questions separately from agreed decisions. Finish when the user agrees on the design and both files reflect it.
