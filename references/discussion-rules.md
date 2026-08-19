# Feature Discussion

## Advisor choice

At discussion startup, inspect the current harness's available skills. If one or more read-only advisor skills are available and the user has not already approved or declined advisor use for this feature, ask once which may be used for material discussion or investigation. Name the available advisors and offer a compact choice such as `[advisor A]`, `[advisor B]`, `both`, or `none`.

- Do not invoke an advisor before the user approves it.
- Treat approval as permission for the current feature, not a requirement to consult on every question. Use an approved advisor when independent analysis can materially improve an architecture discussion, evidence investigation, challenge, or consequential tradeoff.
- Give the advisor the concrete question and relevant evidence. Project Manager still owns direct inspection, research synthesis, grilling, decisions, and recommendations.
- Do not use an advisor as a substitute for repository inspection, to reopen Reviewer/Tester finding intake, or merely because one is available.
- Record the available advisors, user choice, and authorized purpose in `discussion.md`. If none are available or the user declines, continue without delay.

**Good:** "Fable and Sol advisor skills are available. May I use Fable, Sol, both, or neither for material architecture investigation during this feature?"

**Bad:** Silently consult an advisor, ask again before every discussion, or use one to duplicate a Reviewer's technical judgment.

## Research ownership

Project Manager performs ordinary repository exploration directly: relevant file and symbol search, instructions, nearby code, configuration, logs, and one coherent codepath.

Before using Explorer, record:

1. The decision the evidence can change.
2. Direct inspection already completed.
3. The exact remaining evidence gap.
4. Why another bounded direct read is insufficient.

Use one Explorer for one qualifying gap. Use multiple tracks only when each needs different evidence or methods and can independently change the current decision. Multiple files, related checks, corroboration, available capacity, or general thoroughness do not create independent research lanes.

## Discussion sequence

1. Initialize the isolated worktree and fresh control documents.
2. Offer the one-time available-advisor choice when applicable.
3. Inspect enough repository evidence to avoid asking the user questions the code can answer.
4. Grill the user on every material current-feature uncertainty.
5. Record decisions and form the plan progressively.
6. Define the architecture boundary and assess safe Developer parallelism.
7. Gather evidence before challenging an incorrect, fragile, or inconsistent direction.
8. Present a brief plan and wait for approval before production changes.

## Feature grilling

Grilling discovers requirements; it does not oppose the request and needs no evidence.

- Ask about intent, user flow, inputs, outputs, state, boundaries, non-goals, and acceptance criteria. Ask about failure behavior, integrations, migration, compatibility, or operating models only when the current request materially affects them.
- Ask a small coherent batch of related questions.
- Continue until implementation would not require guessing.
- Stay on current reachable behavior; do not invent distant support or unlikely future cases.
- Resolve an execution, deployment, recovery, or concurrency premise only when the current approved behavior materially depends on it. Do not grill the user about speculative operating models.

## Architecture boundary discussion

Make removability and ownership explicit before planning implementation:

- feature-owned folders/modules;
- allowed external consumers, registrations, routes, or adapters;
- forbidden shared packages or systems;
- fixed API, event, schema, storage, and error contracts;
- generated, migration, and lockfile policy;
- material execution or deployment assumptions required by the current feature;
- what requires renewed user approval.

When the user requests an isolated experimental feature, translate that into path globs and narrow seam purposes that the Project Manager can check mechanically and Code Quality Reviewer can judge architecturally.

## Parallelism assessment

At plan formation, actively look for independently writable lanes. Fixed contracts may permit app/API, producer/consumer, migration/runtime, or other parallel Developer work.

Do not force lanes. Reject parallelism when contracts are unsettled, ownership overlaps, generated artifacts collide, one lane consumes unreviewed design from another, or integration order is undefined.

Record one of:

- `single Developer`: the exact independence condition that fails; or
- `parallel Developers`: frozen contracts, exclusive ownership, lane dependencies, integration order, lane criteria, and designated integration Developer.

## Evidence-based challenges

Challenge only when evidence shows a proposed direction may be incorrect, harmful, inconsistent, or materially less correct than a feasible alternative.

Every challenge contains:

1. Evidence synthesis from direct inspection and any bounded delegated research.
2. Immediate effect on the current feature or reachable behavior.
3. A small set of correct and feasible options.
4. A recommendation and reason.

Use primary project evidence first. Use authoritative external sources when material. Label anecdotes and inference.

When a challenge depends on a measurable claim, Project Manager defines the falsifiable hypothesis, control, variables, metrics, threshold, sample/repetitions, and decision effect before assigning a temporary Tester. Tester returns measurements and limitations; Project Manager synthesizes and decides whether to challenge.

## Brief plan approval

The brief plan must state:

- intended outcome, scope, and non-goals;
- the smallest complete implementation approach and optional capabilities deliberately excluded;
- architecture boundary and fixed contracts;
- one- or multi-Developer delivery shape with evidence;
- acceptance criteria and verification approach;
- material current-feature risks.

Explicit approval authorizes ordinary implementation, local-only checkpointing, review progression, testing, correction, and retesting inside isolated feature workspaces. It does not authorize source-checkout mutation, transfer, source-branch commit, push, publication, or destructive cleanup, even when phrased as full permission or autonomous completion. Reapprove any later material change to behavior, boundary, fixed contract, delivery shape, or criteria.

## Discussion gate

- Repository-answerable questions were inspected directly.
- Every Explorer lane passed the escalation gate.
- Available advisor skills were offered once; any consultation stayed within the user's recorded choice.
- Grilling resolved material uncertainty.
- Boundary and parallelism decisions are explicit.
- The plan is the smallest complete approach for the approved behavior and does not add unrequested capabilities.
- Bug work includes root-cause and bug-class reasoning.
- Every challenge is evidence-backed and currently relevant.
- Open items are genuinely unresolved.
- User approved the brief before production changes.
