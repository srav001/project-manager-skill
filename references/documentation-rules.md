# Control Documents

The Project Manager is the sole writer of `discussion.md` and `plan.md`. Agents report evidence; the Project Manager records it. Keep both files outside checkpoint commits.

## Lifecycle

- For new non-trivial work, create the isolated feature worktree first, then delete stale control files only at that worktree root and create fresh files.
- Never delete or modify source-checkout control files during startup.
- Preserve feature-worktree control files during continuation, recovery, compaction, and agent follow-up.
- Give both documents the same feature, worktree, branch, base, and lifecycle identity.
- Treat documents as current control state, not transcripts.

## `discussion.md`

Keep durable product and architecture context:

- exact user goal and constraints;
- relevant repository evidence;
- numbered accepted decisions;
- rejected alternatives and evidence-based reason;
- architecture boundary contract and approved expansions;
- reported optional capabilities, unsupported hypotheses, and scope decisions only when they materially affect the current feature;
- root cause and broader bug class for bug work;
- genuinely unresolved questions and accepted residual risk.

Use an append-only numbered decision log. When an open item is resolved, remove it from `Open Items` and link it to the decision. Do not leave resolved questions marked open or repeat the same decision in several sections.

```md
# Discussion

## Identity
- Feature: [objective]
- Lifecycle: [new | continuation | recovery; marker]
- Source: [checkout, branch, base SHA]
- Feature workspace: [integration worktree and branch]

## Goal and Constraints
[Exact user intent and non-goals]

## Evidence
- E1 — [fact, source path/log/link]

## Decisions
1. [decision, evidence ids, approval]

## Rejected Alternatives
- [alternative] — [scope, authorization, correctness, or feasibility reason]

## Architecture Boundary
- Feature-owned roots: [globs]
- Allowed consumer seams: [globs and purpose]
- Forbidden shared systems: [globs/systems]
- Generated/schema/migration/lockfile policy: [rule]
- Material execution/deployment assumptions: [approved values or N/A]
- Material expansion authority: [approval or pending]

## Open Items
- [only unresolved question or risk]
```

## `plan.md`

Keep state in compact tables. Update status cells in place. Preserve failures through the findings and event tables rather than copying narrative correction packages.

### Statuses

| Status | Meaning |
|---|---|
| `Ready` | Dependencies satisfied; may dispatch |
| `Active` | Assigned work is running |
| `Blocked` | A demonstrated condition prevents progress |
| `Done` | Work item completed with evidence |
| `Pending` | Gate has not been evaluated |
| `Pass` | Gate evidence is complete |
| `Fail` | Gate requires correction |

### Finding states

| State | Meaning |
|---|---|
| `Proposed` | A role reported the problem; Project Manager disposition is pending |
| `Authorized` | The documents-only Finding Scope-and-Stage Screen admitted the role's blocker to the active correction queue |
| `Deferred-Final` | A focused-stage observation fell outside that gate; return it only to its originating Reviewer at final review as unproven |
| `Excluded` | It is recorded with a non-implementation classification and reason |
| `Corrected` | Developer supplied correction proof; Reviewer closure is pending |
| `Closed` | Originating role verified closure on the recorded revision |

### Required state

```md
# Plan

## Identity and Workspace
| Field | Value |
|---|---|
| Feature and lifecycle | [objective; new/continuation/recovery] |
| Source checkout | [path, branch, upstream, original base, status] |
| Integration worktree | [path, branch, current HEAD, expected dirty state] |
| Lane/review worktrees | [paths, branches or detached SHAs, owners] |
| Environment links | [paths and ignored/untracked proof; never values] |
| Ports and preview | [services, temporary ports, URLs, processes, readiness] |
| Integration lock/handoff | [state] |

## Approved Contract
| Item | Approved value |
|---|---|
| Outcome and non-goals | [value] |
| Acceptance criteria | [value] |
| Excluded optional capabilities | [value or none] |
| Material execution/deployment assumptions | [approved values or N/A] |
| Feature-owned roots | [globs] |
| Allowed consumer seams | [globs and purpose] |
| Forbidden shared systems | [globs/systems] |
| Delivery shape | [single implementation set or justified lanes] |
| Parallelism decision | [lane gate passed with evidence, or exact failed independence condition] |
| Lane ownership/order | [owner, paths, dependencies, order, or N/A] |
| User plan approval | [exact approval and revision] |

## Primary Milestone
- Milestone: [current user-facing outcome]
- Visible progress: [latest win]
- Next critical action: [one action]
- Estimate and variance alarm: [estimate, elapsed, state]

## Work Queue
| Item | Owner | State | Depends on | Evidence/checkpoint |
|---|---|---|---|---|
| W1 | [role/thread] | Ready | [ids or none] | [ref] |

## Agent and Lane State
| Role/lane | Thread | Worktree/revision | State | Retain/replace reason |
|---|---|---|---|---|
| Developer | [id] | [path/SHA] | [state] | [reason] |
| Code Quality Reviewer | [id] | [path/SHA] | [state] | [reason] |
| Adversarial Reviewer | [id] | [path/SHA] | [state] | [reason] |
| Tester | [id] | [path/SHA] | [state] | [reason] |

## Checkpoints and Gates
| Gate/checkpoint | SHA or identity | Status | Evidence | Next condition |
|---|---|---|---|---|
| Workspace Isolation | [identity] | [status] | [ref] | [condition] |
| Plan Approval | [plan revision] | [status] | [ref] | [condition] |
| Developer Handoff | [SHA] | [status] | [ref] | [condition] |
| Focused Dual Review (Test Readiness) | [test-candidate SHA] | [status] | [both TEST READY refs] | [condition] |
| Integrated Testing | [test-candidate SHA] | [status] | [Tester evidence and stage correction count] | [condition] |
| Final Dual Review (Release Readiness) | [final SHA] | [status] | [both RELEASE PASS refs] | [condition] |
| Regression Confirmation | [final SHA] | [status or N/A] | [Tester evidence or no-production-change reason] | [condition] |
| Feature Completion | [SHA] | [status] | [ref] | [condition] |
| Integration Readiness | [diff/source target] | [status] | [ref] | [condition] |
| Transfer Approval | [diff/source target] | [status] | [approval] | [condition] |
| Integration Handoff | [source diff] | [status] | [ref] | [condition] |
| Commit/Push Approval | [source diff] | [status] | [approval] | [condition] |
| Publish | [commit] | [status] | [ref] | [condition] |

## Findings
| ID | Source/review kind | Defect class | Scope source, proof, and failure analysis | Classification | Blocks | Target SHA | State/disposition | Correction/proof SHA |
|---|---|---|---|---|---|---|---|---|
| CQ-1 | Code Quality / focused | [class] | [criterion/contract/flow and evidence] | [classification] | [test readiness/release/N/A] | [SHA] | [state and reason] | [pending or N/A] |

## Event Log
- [timestamp/marker] — [one-line state change with evidence id]
```

Add rows rather than new narrative sections. Add a specialized gate row only when the plan actually needs it, such as Research, progressive Code Quality, lane integration, post-release regression re-review, or recovery.

## Gate rules

- Workspace Isolation passes before project files change, dependencies install, runtime starts, or agents receive repository work.
- Plan Approval passes before production or source-code changes.
- Developer Handoff proves package completeness, approved-path scope, workspace confinement, and checkpoint identity—not technical approval.
- Focused Dual Review passes only when both focused Reviewers return `TEST READY` for the same test-candidate SHA.
- Integrated Testing starts on that SHA and passes only on a later SHA that completed the required focused correction-delta review and retest loop.
- Final Dual Review begins only after integrated behavior stabilizes. It passes when both Reviewers complete one final full-diff pass and return same-SHA `RELEASE PASS`; later final corrections receive delta-and-closure review and reissued verdicts without restarting the full-diff pass.
- Regression Confirmation is `N/A` when final review caused no production change. Otherwise it passes only after the retained Tester verifies the final-review correction on the dual-`RELEASE PASS` SHA.
- Feature Completion passes before source integration preparation.
- Transfer Approval binds to the exact feature diff, source checkout, branch, and prepared target revision.
- Commit/Push Approval is separate and occurs only after the user inspects the transferred uncommitted source diff.
- Publish passes only after remote verification and cleanup.

Any identity mismatch is a hard stop. Mark dependent gates invalid until Git-verifiable identity is restored.

Encode the same dependency order in the Work Queue. Final full review remains `Pending`, never `Ready` or `Active`, until Integrated Testing passes. When focused dual review passes and Tester is ready, dispatch integrated testing immediately instead of opening broader review work.

## Findings and correction rounds

- Record one row per finding. Do not duplicate it in a correction-package narrative.
- Preserve author, review kind, defect class, scope source, proof, evidence, Tester failure analysis when applicable, classification, blocked gate, disposition reason, target SHA, and closure proof when implemented.
- A Tester `FAIL` is incomplete until it records the failing action/time/order, expected and observed result, correlated runtime evidence, relevant source path, immediate cause, root cause or bounded uncertainty, reproduction, and smallest required outcome. Return incomplete analysis to Tester; Project Manager does not reconstruct it.
- Send only `blocking in-scope defect` findings for the active gate to a Developer. Record `unrelated existing defect`, `optional additional capability`, and `unsupported hypothesis` without implementing them.
- Mark a focused-stage finding `Deferred-Final` when it falls outside only that review kind or stage's immediate reachability. It cannot block testing and returns only to its originating Reviewer at final review as an unproven observation.
- Use `scope decision required` only when the unresolved premise materially prevents correct planning or verification of the approved request.
- Keep excluded findings visible to their originating role during re-review. Reopen a settled exclusion only for materially new evidence or newly approved scope; record an active scope-mapping dispute as `scope decision required`.
- Mark correction-round boundaries and stage in the event log and checkpoint table. Count recurrence separately for focused review, integrated testing, and final review.
- When a demonstrated authorized class recurs across two rounds in one stage, mark the dependent queue blocked and record the Reviewer's structural assessment plus user decision. A focused deferral confirmed at final review is not recurrence.
- After `TEST READY` or `RELEASE PASS`, record the authority that reopened that stage's review.

## Maintenance and recovery

- Update the primary milestone, work queue, agent table, checkpoint table, and findings table after every handoff.
- After Focused Dual Review passes and while Integrated Testing is pending or active, make real integrated evidence the primary milestone and next critical action. Do not point the milestone back at broad review closure.
- Never label a checkpoint `final candidate`, `release candidate`, or `mandatory final review` before Integrated Testing has run. Use `test candidate` until final review begins.
- Keep only genuinely open items in `discussion.md`.
- Keep one current row for each gate; update it in place and preserve prior failures through evidence references and one-line events.
- Remove transcript-like commentary and repeated prompt text.
- Anchor review, correction, test, and transfer state to Git-verifiable SHAs.
- After compaction, verify every recorded path, branch, SHA, dirty-state expectation, process, port, agent, gate, and approval before resuming.

## User-facing status

Report only:

- current milestone or result;
- what passed or changed;
- what is active or blocked;
- one next user action when required.

Do not print the internal tables unless the user asks.
