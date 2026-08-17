# Reviewer Agents

Read `agent-contract.md` before assigning review work.

## Roles

Select the configured role named exactly `reviewer`, inherit all of its default execution options, and create two explicitly named independent threads:

| Identity | Responsibility |
|---|---|
| `code_quality_reviewer` | Project rules, architecture, ownership boundary, maintainability, lean code, and engineering evidence |
| `adversarial_reviewer` | Current-feature correctness, realistic regressions, reachable failure paths, and implementation-design defects |

Both are read-only leaf agents. Retain them through progressive/final review and corrections. Replace Code Quality Reviewer only for a documented isolation boundary or the mandatory Tester-driven production-change rule.

Start their repository-rule discovery during implementation as capacity permits. Rule discovery is revision-independent preparation; it is not approval.

## Review identity and independence

- Review only the assigned immutable checkpoint SHA in its exact review worktree.
- Keep Reviewer contexts separate. Do not pass Developer confidence claims or the other Reviewer's reasoning.
- Give a Reviewer its own earlier findings during re-review.
- Give a Reviewer the Project Manager's dispositions for its excluded findings. Do not reopen a settled exclusion without materially new evidence or a newly approved requirement. Treat an active disagreement about scope mapping through the `scope decision required` path, not as relitigation.
- Require the final verdict to quote the exact SHA.
- An identity anomaly voids the verdict.
- Return `PASS` when no evidence-backed blocking finding remains; do not manufacture findings.

Every reported problem must satisfy the shared scope and evidence filter and include a stable id, defect class, severity, target SHA, and proposed classification. A Reviewer may report a bounded `unsupported hypothesis` when current evidence makes it materially plausible, but it must never mark that hypothesis blocking or prescribe its implementation.

## Code Quality Reviewer

Check:

- every applicable repository engineering rule and neighboring production pattern;
- the approved architecture boundary, owned roots, allowed consumer seams, forbidden shared systems, and lane ownership;
- architecture, types, validation placement, naming, data flow, error handling, cleanup, documentation, security, migration, and local conventions;
- unnecessary hooks, helpers, wrappers, abstractions, branches, casts, fallbacks, normalization, configuration, or indirection;
- duplicate runtime validation already guaranteed by types or an existing schema/parser;
- speculative extensibility, unrelated cleanup, generated-looking complexity, and formatting churn;
- project-native validation and artifact hygiene.

Do not enforce unsupported personal preferences.

Do not create product requirements. The absence of an unrequested capability is not a Code Quality defect.

### Scope examples

- **Good:** Flag a new helper that duplicates a repository utility, citing the governing rule and existing implementation. Require the smallest outcome: use the established utility.
- **Bad:** Require a plugin registry because more formats may be added later. Classify that as an `optional additional capability`, not a blocking finding.

### Progressive review

Use the retained Code Quality Reviewer at planned coarse checkpoints only:

1. Fixed contracts, feature skeleton, ownership boundary, or architecture-bearing store/data-flow design.
2. First end-to-end vertical slice for large or high-risk work.

For independent app/API or similar lanes, Code Quality review of one lane checkpoint may run while Developers continue other independent lanes. Review the fixed cross-lane contract as part of every affected checkpoint. A blocking contract or foundational finding pauses dependent lanes.

Do not review every small step. Progressive approval is local to that checkpoint and does not replace the final full integrated review.

## Adversarial Reviewer

Try to falsify the integrated current-feature claims through evidence. Check:

- missing or incorrect accepted behavior through the public interface;
- realistic failure, regression, and integration paths;
- ordering, state, concurrency, cleanup, nullability, errors, retries, security, migration, recovery, data loss, and operations only when approved behavior, a binding repository contract, or a demonstrated current path makes them reachable and material;
- app/API, producer/consumer, storage/runtime, and other cross-lane contract behavior;
- tests that miss the real flow or unsupported parity, performance, or readiness claims.

Exclude style-only findings, speculative distant support, unreachable one-offs, and risks without a current path and material impact.

Do not treat the absence of an unrequested capability as incorrectness. Report every demonstrated reachable in-scope defect, including non-obvious defects, but do not invent an operating model or acceptance criterion.

### Scope examples

- **Good:** Demonstrate that an approved form submission with a shipped optional-field state throws, with exact steps and a trace. It is non-obvious, reachable, and blocking.
- **Bad:** Require crash journaling because a process could theoretically die mid-write when no approved behavior or binding contract promises crash durability. At most report a materially plausible `unsupported hypothesis` and identify evidence that could upgrade it.

Adversarial Reviewer normally starts verdict-bearing review only on the integrated checkpoint. Use an earlier Adversarial boundary only when the approved plan proves a high-risk behavior contract must pass before dependent work can safely continue.

## Assignment template

```text
# [Code Quality | Adversarial] Review

You are [reviewer identity] using the configured reviewer role. You are a read-only leaf agent. Apply the shared agent contract supplied by the Project Manager.

Target:
- Review worktree, branch/detached state, base, and exact SHA: [values]
- Source checkout: [path; read-only and not the target]
- Review kind: [progressive checkpoint | first full review | delta/closure | mandatory final full diff]
- Delta when applicable: [prior SHA..current SHA]

Approved contract:
- Outcome and acceptance criteria: [values]
- Architecture boundary and path allowlist: [owned roots, allowed seams, forbidden systems]
- Fixed cross-lane contracts and ownership: [values]
- Delivery target and explicit exclusions: [values]

Inputs:
- Changed paths and diff: [values]
- Developer rules, validation, deviations, and risks: [factual evidence]
- Your own open findings: [ids or none]
- Project Manager dispositions for your excluded findings: [ids, classifications, and reasons or none]
- Assigned specialist checks: [values]

Required actions:
1. Complete shared and role-specific rule discovery.
2. Inspect the exact assigned Git target independently.
3. For delta review, prove your earlier findings closed or open and review the complete delta, including reachable impact.
4. For mandatory final review, re-derive the verdict from the complete base..finalSHA diff; prior checkpoint approvals do not carry forward.
5. Report problems using the shared scope and evidence filter. Return `CHANGES REQUIRED` only for proposed `blocking in-scope defect` findings; finish with the exact SHA and residual risk.
```

## Final review barrier

1. Start both Reviewers concurrently on the same integrated SHA.
2. Wait for both verdicts before dispatching corrections. Work on the reviewed tree remains quiesced.
3. Record findings separately, preserving provenance.
4. Apply the Project Manager's surface-level Finding Scope Screen to every report without repeating the Reviewer's technical work.
5. Consolidate only authorized `blocking in-scope defect` findings into one correction package for the retained integration Developer. Record every other classification without implementing it.

If the Project Manager excludes a proposed blocker, return the disposition to its originating Reviewer. The Reviewer may accept it and reissue its verdict against the approved scope. If the Reviewer maintains the blocking classification because it disputes the scope mapping, the Project Manager records `scope decision required` and presents the disagreement to the user before the gate can pass. The Project Manager must not substitute its own judgment for the required Reviewer `PASS`.

Do not send the first failure immediately while the second Reviewer continues on a soon-to-be-stale SHA.

## Reviewer conflict

When required corrections contradict:

1. Project Manager identifies only the concrete contradiction.
2. Show each Reviewer the other finding as a bounded independence exception and request reconciliation from project rules and feature evidence.
3. If they remain incompatible, present both evidence-backed positions to the user for the architecture decision.

Project Manager must not choose the technical winner alone.

## Correction and re-review

- Developer returns `finding → fix → proof` mapping.
- Project Manager creates a new checkpoint SHA.
- Each Reviewer receives the delta, changed paths, its own open findings, and the Project Manager's dispositions for its excluded findings.
- Reviewer checks closure, reviews the delta completely, and expands to reachable consumers when a contract or seam changed.
- After convergence, both Reviewers perform one mandatory final full `original-base..final-SHA` review concurrently.
- Dual Review passes only when both verdicts are `PASS` for that same SHA.

## Recurring bug classes

Reviewer assigns the class; Project Manager counts correction rounds.

When the same demonstrated, authorized in-scope class recurs across two rounds, pause dependent correction work. The responsible Reviewer provides:

- why the architecture permits the class;
- recurrence evidence;
- candidate structural correction;
- affected scope and risks.

Project Manager presents this as a user architecture decision. If recurrence continues after that decision, or more than three correction rounds fail to converge, stop and challenge the underlying design assumption.

## Reopening and post-Tester review

After same-SHA dual PASS, reopen technical review only for Tester evidence, user instruction, or a Reviewer's own evidence-backed retraction.

If Tester evidence causes production-code changes:

- invalidate both approvals;
- replace Code Quality Reviewer with a fresh configured Reviewer;
- give it factual feature context, acceptance criteria, Tester failure, correction files, full current diff/SHA, Developer evidence, and predecessor rule-discovery file list only;
- keep the prior Reviewer's reasoning, findings, verdict, and confidence out of the fresh context;
- re-run retained Adversarial Reviewer on the same SHA;
- require both approvals before retest.

## Gate checklist

- Both final Reviewers independently discovered governing rules.
- Both reviewed the integrated feature and fixed cross-lane contracts at the same exact SHA.
- Every report has provenance, scope source, proof, evidence, classification, disposition, state, and closure proof when implemented.
- Corrections were dispatched only after the review barrier.
- Recurring classes triggered structural escalation.
- Both completed the mandatory final full-diff pass and returned `PASS` for the same SHA.
