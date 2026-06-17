---
title: Review Finding Format
---

# Review Finding Format

Subagents must return either `No findings worth raising` or a concise list of findings with these fields.

```md
- title: Short imperative or risk-focused title.
  severity: Blocking | High | Medium | Low
  confidence: High | Medium | Low
  files: `path/to/file.ts:line`, `path/to/other-file.ts`
  evidence: Short quote, diff reference, or concrete observation.
  why_it_matters: One sentence tied to the engineering philosophy.
  suggested_human_review: What the user should inspect or decide, optionally with a brief fix direction.
```

Rules:

- Keep every field short.
- Do not include preamble or broad summary.
- Include nits only when they are evidence of a larger issue.
- Do not include full patches or implementation instructions.
- Allow brief suggested directions.
- Do not imply tests pass or fail. Only evaluate what the test diff does or does not cover.
- Report only high-signal issues that materially affect review quality.
- Propose severity from the persona's viewpoint; the coordinator assigns final severity.

Severity:

- `Blocking`: the PR should not merge without addressing the issue or consciously accepting the risk.
- `High`: likely bug, regression, or major maintainability/design risk.
- `Medium`: real issue that is bounded or can reasonably be fixed later.
- `Low`: minor but worth mentioning because it signals a broader pattern.
