# Design philosophy

Review rubric derived from John Ousterhout's *A Philosophy of Software Design*. These are reasoning tools, not a checklist requiring findings.

| Principle | What to establish in the changed code |
|---|---|
| Complexity has observable costs | Show change amplification, knowledge a caller must carry, or a dependency a maintainer cannot discover locally. Size alone proves little. |
| Deep modules | A small useful contract hides substantial behavior and decisions. A wrapper can earn its place through policy, isolation, or compatibility. |
| Information hiding | Keep a design decision with one owner; show where knowledge is duplicated or callers depend on internal representation. |
| Ownership and locality | Prefer deriving, validating, and enforcing a behavior where its facts live, reducing coordination across states and callers. |
| Different abstractions per layer | Each layer should offer value beyond forwarding the same contract. Names should use the concepts of that layer. |
| Pull complexity downward | Let the owning module absorb unavoidable decisions rather than requiring every caller to repeat them. Compare internal complexity with total caller burden. |
| Clear error semantics | Remove unnecessary special cases through an explicit contract, such as idempotent removal. Preserve errors that carry required information or enforce security. |
| Useful generality | Support current needs with a coherent interface. Avoid hooks for imagined consumers and avoid mixing general mechanisms with one consumer's policy. |
| Design it twice | Compare a concrete alternative against the requirement, interface, migration effort, compatibility, performance, and operational cost. |
| Precise concepts | Difficult names or long contract explanations can reveal unclear ownership. Demonstrate the ambiguity rather than prescribing a preferred word. |
| Intentional consistency | Local precedent reduces cognitive load. Divergence can be justified by a material benefit; precedent alone does not prove correctness. |

Distinguish essential domain complexity from coordination introduced by the implementation. Important facts should have one authoritative source, but caches and historical snapshots may warrant additional representations with explicit consistency semantics. Choose columns, tables, services, and component state according to ownership and lifecycle, not a universal preference.

Use shallow modules, temporal decomposition, pass-through layers, overexposed interfaces, and split ownership as investigation signals. Report only when tracing the affected code establishes a material consequence and an alternative improves the tradeoff. Small diffs can contain major decisions; large diffs can be mechanical.
