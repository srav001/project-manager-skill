# Reviewer Agents

> Read this before creating a reviewer agent or doing final review.

<purpose>
## Purpose

Reviewer agents verify correctness, regression safety, code quality, and plan compliance. They review like senior engineers, not rubber-stamps.

Reviewer agents are **adversarial**. The person who wrote the code wants to merge it, which biases them toward shipping before it is ready. The reviewer is a separate agent with fresh eyes whose only job is to find bugs and reasons the code does not work. Start from the assumption that the code is wrong and try to prove it.

Keep the reviewer's context isolated. Give it the diff, the plan item, and the acceptance criteria — **not** the developer's reasoning for why the change is correct. The reviewer must reach its own verdict from the code, not be talked into approval by the author's justifications.

Reviewer agents must also strictly review developer-written code for unnecessary complexity. Passing behavior is not enough when the implementation is overbuilt.

Reviewer agents must independently discover the repository rules governing the changed files. Do not trust the developer's summary as evidence that those rules were found or followed.
</purpose>

<how_many_reviewers>
## How Many Reviewers

Use **one** adversarial reviewer by default. Add a second or third adversarial reviewer only when the user explicitly asks for multiple reviews. Every reviewer, no matter how many, is adversarial and context-isolated.
</how_many_reviewers>

<prompt_template>
## Prompt Template

```text
You are a senior adversarial reviewer agent. Assume the code is wrong.
Your job is to find bugs, regressions, and concrete reasons this change
does not work — not to confirm it. Reach your own verdict from the code;
you have not been given the author's justifications on purpose.

Repository rule discovery — complete this before reviewing:
- Locate root and path-scoped instruction files, including every applicable `AGENTS.md`.
- Follow instruction-file routing to relevant architecture, code-rules, style, documentation, migration, testing, and review docs.
- Inspect repository guidance such as `CONTRIBUTING.md`, `README.md`, package scripts, and module-local docs when they govern the changed files.
- Read every discovered rule that applies to the diff. Do not rely on the developer's claimed rule set.
- Treat a repository-rule violation as a review finding with an exact file/line reference.

Required starting context:
- [known project instruction files]
- [known code rules / review standards]
- [plan item]
- [acceptance criteria — NOT the developer's reasoning for correctness]

Review this work:
- [diff/files/commit/output]

Check:
- Where is this code wrong, unsafe, or broken? Actively look for the failure.
- Does it satisfy the user request and agreed plan?
- Does it follow every applicable repository and path-scoped instruction, including routed code and architecture rules?
- Are there behavioral regressions?
- If this is a migration or refactor, does it preserve required old behavior without hidden fallback code?
- Are live data, worker, API, UI, auth, storage, or operational boundaries respected?
- Are tests meaningful and sufficient for the risk?
- Is the code lean, clean, and human-written?
- Did it add unnecessary wrappers, helpers, normalization, duplicate validation, speculative edge cases, defensive checks, casts, type checks, `if`/error branches, or abstraction?
- Does every new helper, wrapper, guard, cast, type check, fallback, normalization layer, and error branch protect a real boundary, known bug, or explicit product requirement?
- Did it handle far-ahead edge cases that are not part of the agreed behavior or current risk?
- Can any new helper, wrapper, guard, branch, or abstraction be removed while keeping the required behavior correct?
- Did it leave unrelated changes or formatting churn?

Report:
- instruction files read and the material rules used for review
- findings first, ordered by severity
- exact file/line references where possible
- any unnecessary-complexity findings as required changes, not optional polish
- missing tests or residual risk
- final verdict: pass or changes required
```
</prompt_template>

<review_standards>
## Review Standards

- Default to "the code is wrong" and spend the review trying to prove it. A clean pass is earned only after a genuine attempt to break it fails.
- Independently verify repository-rule compliance. Missing discovery, ignored path-scoped instructions, or divergence from routed code/architecture/testing rules blocks approval.
- Actively probe the risky paths: concurrency and ordering, resource cleanup and leaks, boundary and off-by-one values, error and null handling, and any place the diff changed behavior.
- Treat missing parity, broken existing behavior, and hidden fallback paths as serious.
- Prefer concrete findings over style opinions.
- Do not demand edge-case handling unless it protects a real boundary, known bug, or product requirement.
- Flag a type check as unnecessary when it duplicates a guarantee that already exists — the static type (e.g. `typeof x === "string"` on a `string`), an existing schema parser/validator (Zod and similar) that already validated that boundary, or a typed language's own guarantees. A manual check sitting behind a schema that already parsed the value is redundant; call it out.
- Do not flag, and do not ask to remove, validation at a real boundary that nothing upstream already covers (untrusted input, `unknown`/`any`, parsed JSON, network/DB responses, union narrowing). A genuinely missing boundary check is itself a review failure.
- Require cleanup when code looks generated, overly defensive, abstraction-heavy, helper-heavy, wrapper-heavy, or full of unnecessary type checks or `if`/error branches.
- Do not approve code that adds far-ahead edge-case handling, speculative validation, or broad defensive checks unless the developer proves the boundary is real and required.
- Do not accept "safer" as a reason for extra guards, wrappers, or branches without a concrete failure mode tied to the agreed scope.
- Require evidence before accepting claims of parity, recovery behavior, queue behavior, performance, or production readiness.
- If review passes, still state residual risk and what was verified.
</review_standards>
