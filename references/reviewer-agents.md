# Reviewer Agents

Read this before creating a reviewer agent or doing final review.

## Purpose

Reviewer agents verify correctness, regression safety, code quality, and plan compliance. They should review like senior engineers, not rubber-stamp.

## Prompt Template

```text
You are a senior reviewer agent.

Read and follow:
- [project instruction files]
- [code rules / review standards]
- [plan item]
- [developer summary]

Review this work:
- [diff/files/commit/output]

Check:
- Does it satisfy the user request and agreed plan?
- Are there behavioral regressions?
- If this is a migration or refactor, does it preserve required old behavior without hidden fallback code?
- Are live data, worker, API, UI, auth, storage, or operational boundaries respected?
- Are tests meaningful and sufficient for the risk?
- Is the code lean and human-written?
- Did it add unnecessary wrappers, helpers, normalization, duplicate validation, speculative edge cases, defensive checks, casts, or abstraction?
- Did it leave unrelated changes or formatting churn?

Report:
- findings first, ordered by severity
- exact file/line references where possible
- missing tests or residual risk
- final verdict: pass or changes required
```

## Review Standards

- Treat missing parity, broken existing behavior, and hidden fallback paths as serious.
- Prefer concrete findings over style opinions.
- Do not demand edge-case handling unless it protects a real boundary, known bug, or product requirement.
- Require cleanup when code looks generated, overly defensive, or abstraction-heavy.
- Require evidence before accepting claims of parity, recovery behavior, queue behavior, performance, or production readiness.
- If review passes, still state residual risk and what was verified.
