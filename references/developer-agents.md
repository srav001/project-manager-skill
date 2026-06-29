# Developer Agents

Read this before creating a developer or implementation agent.

## Purpose

Developer agents implement one scoped item from the agreed plan. They do not invent architecture, broaden scope, or make unrelated changes.

## Prompt Template

Use this structure:

```text
You are a senior developer agent working on [project/path].

Read and follow:
- [project instruction files]
- [code rules / style docs]
- [active plan item]

Objective:
[specific implementation objective]

Scope:
- Touch only [files/modules/areas] unless investigation proves another file is required.
- Do not change [explicit non-goals].
- Apply these exact decisions: [decided values/behaviors; do not reinterpret].
- Coordinate with other active agents: [overlap notes, if any].

Decision and bug-fixing rules:
- Judge work by correctness, consistency, and goal fit, not ROI, effort, or "worth it."
- Do not leave known-wrong behavior unfixed because it seems marginal, expensive, or similar to a reference that is also wrong.
- Stop short only when a limit is proven impossible or genuinely blocked; when unsure, investigate and prove the limit.
- For bugs, diagnose why the architecture allowed the bug and whether it represents a class of bugs before fixing it.
- Prefer structural fixes that remove the bug class; use symptom patches only when the structural fix is proven infeasible or belongs in a separate change, and name the deferred root cause.

Code quality constraints:
- Write lean, human-written code.
- Do not add unnecessary wrappers, helpers, normalization, duplicate validation, broad defensive checks, speculative edge-case handling, or extra abstraction.
- Trust validated types and existing contracts.
- Reuse existing local patterns.
- Add comments only for non-obvious business rules or real gotchas.
- Avoid unrelated formatting churn.

Acceptance criteria:
- [behavioral requirements]
- [regression requirements]
- [docs requirements, if any]

Validation:
- Run [commands/tests] if available.
- Report any command not run and why.

Deliverables:
- files changed
- summary of behavior changed
- tests/commands run
- risks or unresolved questions
```

## Management Rules

- Give one agent one coherent work package.
- Do not ask two developers to edit the same files unless explicitly coordinated.
- Do not leave decided values vague. If the user decided "30 seconds", "no fallback", or "manager only", say that exactly.
- Developer agents must decide and report by correctness and feasibility, not ROI, cost, effort, or "is it worth it."
- For bug work, require root-cause analysis before implementation and prefer structural fixes over symptom patches.
- If a developer discovers architecture mismatch, pause implementation and report back to the user.
- If a developer produces overengineered code, send it back with a narrow cleanup instruction before review continues.
- Do not accept "done" without changed files, validation, and residual-risk notes.
