---
title: Engineering Philosophy
---

# Engineering Philosophy

- Complexity is the primary enemy.
- Prefer simple solutions over clever solutions.
- Prefer explicitness over magic.
- Optimize for long-term maintainability.
- Keep modules deep: small interfaces, substantial internal value.
- Complexity is observable through the shared red flags below.
- Abstractions must hide meaningful design decisions, not just wrap other code.
- Important facts should have one source of truth; avoid duplicating state, policy, or derived knowledge across the system.
- Prefer existing sources of truth, primitives, and idioms over recreating near-duplicates.
- Prefer local reasoning; avoid designs that require callers to understand distant details.
- Pull unavoidable complexity downward into the module that owns it.
- Put ownership where behavior can be derived, validated, or enforced directly instead of coordinating it across callers.
- Different layers should provide different abstractions; pass-through layers are suspect.
- Layer names and APIs should use the vocabulary of their own layer, not leak concepts from callers or implementation details from callees.
- APIs should be ergonomic at call sites; avoid boolean parameters, long positional parameter lists, redundant props, and arguments the callee can derive.
- Define errors and special cases out of existence when clear semantics can remove them.
- Names should be precise, consistent, and hard to misinterpret.
- Comments should explain intent, constraints, and non-obvious decisions, not restate code.
- Consistency with established local patterns reduces cognitive load; diverge only when the improvement is material and intentional.
- Logs, defensive checks, and compatibility paths should earn their complexity with a plausible operational benefit.
- Review judgment matters: distinguish must-fix issues from broad cleanup that belongs in explicit follow-up work.

## Shared Red Flags

- Change amplification: a small behavior change requires edits in many places.
- Cognitive load: developers must absorb too much information to make a safe change.
- Unknown unknowns: it is unclear what code or knowledge is needed to make a safe change.
- Information leakage: the same design decision appears in multiple modules.
- Reinvented source of truth: code creates a new helper, constant, schema, policy, or primitive when an existing one should remain authoritative.
- Split ownership: one behavior is coordinated across multiple states, layers, or call sites when a single owner could expose the result.
- Special-general mixture: general-purpose and special-purpose logic are tangled together.
- Non-obvious code: behavior, intent, or constraints are difficult to infer from names, structure, and comments.
- Overexposure: common callers must understand rare features or low-level details.
- Awkward API shape: boolean parameters, positional argument lists, pass-through props, or redundant required inputs make call sites harder to read than necessary.
- Layer vocabulary leak: names or interfaces expose concepts from the wrong layer, such as caller-specific UI language in domain APIs or implementation details in higher-level code.
- Idiom drift: code is technically valid but inconsistent with the established patterns of its surrounding layer.
- Low-value noise: logging, defensive branching, or compatibility code adds paths to understand without reducing meaningful risk.
- Hard-to-describe behavior: if explaining the contract takes too long, the abstraction may be too complex.
