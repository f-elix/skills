# Standards checklist

## Fowler smell baseline

Adapted from the baseline in Matt Pocock's code-review skill, drawing on Martin Fowler's *Refactoring*, chapter 3. Apply even without repository standards, but label smells as judgment calls. Documented standards take precedence over these heuristics. Verify the cost before prescribing a refactor.

| Signal | Investigation |
|---|---|
| Mysterious Name | Does the name obscure a relevant domain fact or responsibility? |
| Duplicated Code | Is shared policy repeated, or are similar expressions coincidental? |
| Feature Envy | Does behavior repeatedly reach into another owner's data? |
| Data Clumps | Do values travel together because they form one meaningful concept? |
| Primitive Obsession | Does a primitive hide constraints a domain type could enforce? |
| Repeated Switches | Are multiple branches reimplementing the same decision? |
| Shotgun Surgery | Does one behavior change require edits in distant places? |
| Divergent Change | Does a module combine unrelated reasons to change? |
| Speculative Generality | Are hooks or parameters justified by current requirements? |
| Message Chains | Does navigation expose implementation knowledge to callers? |
| Middle Man | Does delegation supply useful policy or isolation? |
| Refused Bequest | Does an implementation reject much of its inherited contract? |

## Local readability, consistency, and reuse

Compare the changed code with relevant siblings and actual consumers. Investigate unclear domain/layer vocabulary, hard-to-read caller arguments, scattered policy, tangled control flow, hidden failure semantics, and test setup that obscures the protected behavior. Raise only concrete costs; avoid introducing abstractions merely to eliminate a smell.

Search before asserting an existing helper, source of truth, component, or idiom should be reused. Confirm semantics, availability, and ownership fit. Treat logs, defensive checks, comments, and compatibility branches according to the operational information or constraints they preserve, not their mere presence. Apply repository comment guidance whenever reviewing a code comment.

Distinguish documented violations from heuristic improvements. Leave mechanical formatter/linter feedback to tooling. Existing debt is outside findings unless the PR materially amplifies it.
