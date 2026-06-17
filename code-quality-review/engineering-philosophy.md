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
- Prefer local reasoning; avoid designs that require callers to understand distant details.
- Pull unavoidable complexity downward into the module that owns it.
- Different layers should provide different abstractions; pass-through layers are suspect.
- Define errors and special cases out of existence when clear semantics can remove them.
- Names should be precise, consistent, and hard to misinterpret.
- Comments should explain intent, constraints, and non-obvious decisions, not restate code.

## Shared Red Flags

- Change amplification: a small behavior change requires edits in many places.
- Cognitive load: developers must absorb too much information to make a safe change.
- Unknown unknowns: it is unclear what code or knowledge is needed to make a safe change.
- Information leakage: the same design decision appears in multiple modules.
- Special-general mixture: general-purpose and special-purpose logic are tangled together.
- Non-obvious code: behavior, intent, or constraints are difficult to infer from names, structure, and comments.
- Overexposure: common callers must understand rare features or low-level details.
- Hard-to-describe behavior: if explaining the contract takes too long, the abstraction may be too complex.
