# Feature Discussion

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
2. Inspect enough repository evidence to avoid asking the user questions the code can answer.
3. Grill the user on every material current-feature uncertainty.
4. Record decisions and form the plan progressively.
5. Define the architecture boundary and assess safe Developer parallelism.
6. Gather evidence before challenging an incorrect, fragile, or inconsistent direction.
7. Present a brief plan and wait for approval before production changes.

## Feature grilling

Grilling discovers requirements; it does not oppose the request and needs no evidence.

- Ask about intent, user flow, inputs, outputs, state, boundaries, failure behavior, integrations, migration, compatibility, non-goals, and acceptance criteria.
- Ask a small coherent batch of related questions.
- Continue until implementation would not require guessing.
- Stay on current reachable behavior; do not invent distant support or unlikely future cases.

## Architecture boundary discussion

Make removability and ownership explicit before planning implementation:

- feature-owned folders/modules;
- allowed external consumers, registrations, routes, or adapters;
- forbidden shared packages or systems;
- fixed API, event, schema, storage, and error contracts;
- generated, migration, and lockfile policy;
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
- architecture boundary and fixed contracts;
- one- or multi-Developer delivery shape with evidence;
- acceptance criteria and verification approach;
- material current-feature risks.

Explicit approval authorizes ordinary implementation immediately. Reapprove any later material change to behavior, boundary, fixed contract, delivery shape, or criteria.

## Discussion gate

- Repository-answerable questions were inspected directly.
- Every Explorer lane passed the escalation gate.
- Grilling resolved material uncertainty.
- Boundary and parallelism decisions are explicit.
- Bug work includes root-cause and bug-class reasoning.
- Every challenge is evidence-backed and currently relevant.
- Open items are genuinely unresolved.
- User approved the brief before production changes.
