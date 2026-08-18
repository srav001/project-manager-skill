# Reviewer Agents

Read `agent-contract.md` before assigning review work.

## Roles

Select the configured role named exactly `reviewer`, inherit all of its default execution options, and create two explicitly named independent threads:

| Identity | Responsibility |
|---|---|
| `code_quality_reviewer` | Focused local code quality before testing; full engineering and architecture quality at final review |
| `adversarial_reviewer` | Focused immediate correctness before testing; full reachable feature and interaction correctness at final review |

Both are read-only leaf agents. Retain them through progressive, focused, final, and correction reviews. Replace Code Quality Reviewer only for a documented isolation boundary or the post-`RELEASE PASS` Tester-driven production-change rule.

Start their repository-rule discovery during implementation as capacity permits. Rule discovery is revision-independent preparation; it is not approval.

## Review identity and independence

- Review only the assigned immutable checkpoint SHA in its exact review worktree.
- Keep Reviewer contexts separate. Do not pass Developer confidence claims or the other Reviewer's reasoning.
- Give a Reviewer its own earlier findings during re-review.
- Give a Reviewer the Project Manager's dispositions for its excluded findings. At final review, also return only that Reviewer's own `Deferred-Final` observations, labeled unproven. Never show one Reviewer the other's deferrals or reasoning.
- Do not reopen a settled exclusion without materially new evidence or a newly approved requirement. Treat an active disagreement about scope mapping through the `scope decision required` path, not as relitigation.
- Require the final verdict to quote the exact SHA.
- An identity anomaly voids the verdict.
- Return the assigned stage verdict when no evidence-backed blocker for that stage remains; do not manufacture findings.

Every reported problem must satisfy the shared scope and evidence filter and include a stable id, defect class, severity, target SHA, proposed classification, and `Blocks: test readiness | release` for a proposed blocker. A Reviewer may report a bounded `unsupported hypothesis` when current evidence makes it materially plausible, but it must never mark that hypothesis blocking or prescribe its implementation.

## Code Quality Reviewer

During final full review, check:

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

### Focused Code Quality review

Before first integrated testing, inspect only:

- newly written or changed files and directly modified call sites;
- binding repository rules governing those lines;
- leanness, readability, unnecessary abstractions, branches, fallbacks, wrappers, validation, or indirection;
- obvious approved-boundary violations and nearby established patterns.

Do not perform whole-feature architecture, broad system-interaction, speculative maintainability, or distant-consumer analysis. Return `NOT TEST READY` only for a demonstrated violation inside these focused checks; return `TEST READY` otherwise.

- **Good:** Flag an unnecessary wrapper or duplicated validation in the changed handler using the binding local rule and nearby pattern.
- **Bad:** Reconstruct the whole workflow recovery architecture to assess whether the changed handler needs another lifecycle mechanism; leave that to final review.

### Progressive review

Use the retained Code Quality Reviewer at planned coarse checkpoints only:

1. Fixed contracts, feature skeleton, ownership boundary, or architecture-bearing store/data-flow design.
2. First end-to-end vertical slice for large or high-risk work.

For independent app/API or similar lanes, Code Quality review of one lane checkpoint may run while Developers continue other independent lanes. Review the fixed cross-lane contract as part of every affected checkpoint. A blocking contract or foundational finding pauses dependent lanes.

Do not review every small step. Adversarial Reviewer does not run progressive review. Progressive approval is local to that checkpoint and does not replace focused or final review.

## Adversarial Reviewer

During final full review, try to falsify the integrated current-feature claims through evidence. Check:

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

### Focused Adversarial review

Before first integrated testing, inspect only:

- immediate execution paths of the approved acceptance criteria;
- obvious correctness failures on those paths;
- direct regressions in existing supported callers of changed code;
- concrete defects that would make the first integrated test crash, produce false evidence, corrupt its environment/data, or otherwise become unsafe, impossible, or misleading.

Do not explore distant races, speculative recovery, unrequested operating models, broad lifecycle redesign, or system-wide architecture. Return `NOT TEST READY` only for a demonstrated focused defect with `Blocks: test readiness`; return `TEST READY` otherwise.

- **Good:** Demonstrate that the approved submit action immediately throws or corrupts the test fixture before the real flow can proceed.
- **Bad:** Block testing on a hypothetical restart race that requires broader lifecycle analysis; return it as a broader observation for Project Manager disposition.

### Final review scope

After integrated behavior stabilizes, each Reviewer independently reviews the complete `original-base..final-SHA` diff. Code Quality then applies its complete engineering and architecture scope; Adversarial applies its complete reachable feature, regression, interaction, failure-path, lifecycle, recovery, security, migration, cleanup, and data-loss scope. Approved scope, binding contracts, and demonstrated supported paths still bound both roles.

## Assignment template

```text
# [Code Quality | Adversarial] Review

You are [reviewer identity] using the configured reviewer role. You are a read-only leaf agent. Apply the shared agent contract supplied by the Project Manager.

Target:
- Review worktree, branch/detached state, base, and exact SHA: [values]
- Source checkout: [path; read-only and not the target]
- Review kind: [progressive checkpoint | focused code quality | focused adversarial | correction delta | final full diff]
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
- For final full diff only, your own Deferred-Final observations labeled unproven: [ids or none]
- Assigned specialist checks: [values]

Required actions:
1. Complete shared and role-specific rule discovery.
2. Inspect the exact assigned Git target independently.
3. For focused review, stay inside the assigned focused boundary and return `TEST READY` or `NOT TEST READY` for the exact SHA.
4. For correction delta, prove assigned findings closed or open and review the complete delta within the active stage, expanding only when a changed contract or seam makes a consumer directly reachable.
5. For final full diff, re-derive the verdict from the complete base..finalSHA diff; prior approvals do not carry forward. Return `RELEASE PASS` or `CHANGES REQUIRED`.
6. Report problems using the shared scope and evidence filter, including the named blocked gate for every blocker; finish with the exact SHA and residual risk.
```

## Focused review barrier

1. Start both focused Reviewers concurrently on the same test-candidate SHA.
2. Wait for both verdicts before dispatching corrections. Work on the reviewed tree remains quiesced.
3. Record findings separately, preserving provenance.
4. Apply the Project Manager's documents-only Finding Scope-and-Stage Screen without repeating technical work.
5. Consolidate only authorized `blocking in-scope defect` findings with `Blocks: test readiness`. Record broader focused-stage concerns as `Deferred-Final` without implementing them.

If the Project Manager excludes or defers a proposed blocker, return the disposition to its originating Reviewer. The Reviewer may accept it and reissue its verdict against the assigned focused scope. If the Reviewer maintains the blocking classification because it disputes scope mapping, the Project Manager records `scope decision required` and presents the disagreement to the user before the gate can pass. The Project Manager must not substitute its own judgment for required same-SHA `TEST READY`.

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
- During focused review and the integrated testing loop, each retained Reviewer receives the delta, its own open findings, and relevant dispositions; both reissue `TEST READY` or `NOT TEST READY` on the new SHA before testing or retesting.
- After integrated behavior stabilizes, both Reviewers perform one mandatory final full `original-base..final-SHA` review concurrently and return `RELEASE PASS` or `CHANGES REQUIRED`.
- If final findings cause production changes, both Reviewers review the complete delta, verify closure, expand to reachable consumers only when a contract or seam changed, and reissue the final verdict on the new SHA. Do not repeat the full-diff pass.

## Recurring bug classes

Reviewer assigns the class; Project Manager counts correction rounds.

Count rounds separately within focused review, integrated testing, and final review. When the same demonstrated, authorized in-scope class recurs across two rounds in one stage, pause dependent correction work. A focused-stage deferral confirmed at final review is not recurrence. The responsible Reviewer provides:

- why the architecture permits the class;
- recurrence evidence;
- candidate structural correction;
- affected scope and risks.

Project Manager presents this as a user architecture decision. If recurrence continues after that decision, or more than three correction rounds fail to converge, stop and challenge the underlying design assumption.

## Reopening and post-Tester review

After same-SHA dual `TEST READY` or `RELEASE PASS`, reopen that stage's review only for Tester evidence, user instruction, or a Reviewer's own evidence-backed retraction.

Before final review, Tester-driven production changes use both retained Reviewers for focused correction-delta review before retest. Do not replace either Reviewer.

If regression confirmation after `RELEASE PASS` causes production-code changes:

- invalidate both approvals;
- replace Code Quality Reviewer with a fresh configured Reviewer;
- give it factual feature context, acceptance criteria, Tester failure, correction files, full current diff/SHA, Developer evidence, and predecessor rule-discovery file list only;
- keep the prior Reviewer's reasoning, findings, verdict, and confidence out of the fresh context;
- re-run retained Adversarial Reviewer on the same SHA;
- require both final approvals before retest.

## Gate checklist

- Both focused Reviewers independently discovered governing rules, stayed within their assigned boundary, and returned same-SHA `TEST READY` before integrated testing.
- Focused-stage broader concerns were deferred rather than allowed to block testing.
- Every report has provenance, scope source, proof, evidence, classification, disposition, state, and closure proof when implemented.
- Recurring classes were counted within their stage and triggered structural escalation only there.
- Both completed one mandatory final full-diff pass after testing and returned same-SHA `RELEASE PASS`; later corrections received delta-and-closure review.
