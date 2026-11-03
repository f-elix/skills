# Evidence and finding format

## Falsify before reporting

For each candidate finding, check:

1. **Surrounding behavior:** inspect the owning module, actual callers, and relevant siblings. Establish whether a documented rule, local idiom, or compensating behavior changes the claim. Established practice can explain a choice; it does not excuse a demonstrated bug.
2. **Feasible remedy:** verify suggested symbols, helpers, contracts, and semantics exist at the reviewed target. Identify compatibility or migration costs. Label compilation as unchecked unless actually checked.
3. **Already addressed:** read relevant tests and the captured commits. Author explanations are context; the code determines whether a fix is present.
4. **Introduced consequence:** compare against the pinned baseline. A move alone does not introduce a problem; a new caller or reachable path may expose an inherited one.
5. **Actual consequence:** before calling a concern a confirmed bug, trace the required inputs/state to real producers and the failing path, or reproduce it with established reachability. A test with manually invented inputs proves behavior under those inputs, not application reachability. For other concerns, verify the cited code, rule, requirement, or dependencies and explain the concrete failure, confusion, or maintenance cost they support. State inferred consequences and missing verification explicitly.

For refactors, compare actual semantics before asserting a regression. Verify library behavior from matching source/docs when the distinction matters; visual similarity and remembered API behavior are insufficient.

## Proof labels

| Label | Required evidence |
|---|---|
| `ran` | Name the command or live interaction, snapshot/environment, observed result, and reachability limits. A synthetic reproduction needs a separate trace showing its input can occur. |
| `traced` | A complete reachable path with producer, constraints, affected branch, and consequence anchored to the pinned code. |
| `structural` | Concrete owners, callers, or duplicate policies with anchors, showing existing change amplification, cognitive load, or a hidden dependency. Supports a design/maintainability claim, not a runtime bug claim. |
| `code reading only` | Verified source anchors and a reasoned, concrete concern. State what inspection establishes, which consequences are inferred, and what execution or reachability remains unchecked. These findings can stay in the report, clearly marked. |
| `unverified` | A necessary factual premise could not be checked, such as an unavailable caller, requirement, or configuration. Name the missing evidence and keep the claim as a question in verification/limits. |

Choose the label for what was actually established. Supported standards violations, requirement gaps, and reasoned improvements can be reported from code reading without demonstrating a runtime failure. A suspected bug with verified code premises but incomplete failure proof may remain as a clearly qualified `code reading only` concern; describe the condition and missing proof rather than asserting that it occurs. Separate evidence from severity and review judgment. A proven structural cost may be an acceptable tradeoff. A code smell or preference alone establishes neither a cost nor a blocker.

## Finding shape

Each reviewer returns supported findings using this shape and their actual proof labels, or says `No supported findings`. Include `code reading only` findings under their owning axis. Keep questions with unchecked factual premises separate. Return coverage limits and up to a few human review targets as well.

```md
### [P2] Consolidate eligibility ownership
- Kind: Design tradeoff. Judgment: Improvement.
- Location: [changed source line](pinned-or-absolute-file-link).
- Evidence: short quoted hunk, plus supporting owner/caller anchors.
- Consequence: the specific failure, confusion, or structural cost introduced by the reviewed change; distinguish observed from inferred consequences.
- Remedy: a concrete change or alternative, including relevant tradeoffs.
- Proof: structural; traced policy duplication across the named callers.
- Validation: what was executed or inspected; what remains unchecked.
```

Standards cites the relevant rule or labels a heuristic. Spec quotes the requirement and identifies whether the implementation or requirement premise is wrong. Design also states the chosen decision, principle, alternative, what it removes, and what it costs or makes harder.

Use short excerpts with enough context to assess the claim. Proposed snippets are optional and must be feasible at the reviewed target. If a conceptual design alternative would require a prototype, state that limitation rather than promising it compiles.

## Severity and judgment

- **P0:** demonstrated immediate critical impact requiring urgent action.
- **P1:** a serious introduced defect or demonstrated structural risk that should be addressed before merge.
- **P2:** a material bounded issue or worthwhile design improvement.
- **P3:** a minor issue with a concrete consequence worth raising.

Also label **Must fix**, **Improvement**, or **Tradeoff**. The coordinator assigns final severity and judgment after verification. Reserve a blocking design recommendation for a demonstrated conflict with a required constraint, serious consistency hazard, or substantial evidenced cost; an Ousterhout red flag alone is insufficient. Drop taste-only nits, formatter/linter feedback, and unrelated cleanup.

When several remedies satisfy a required outcome, separate that must-fix outcome from the preferred architecture. A proven bug does not make one particular refactor mandatory; cost out the alternatives before recommending one.
