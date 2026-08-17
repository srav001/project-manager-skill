# Control Documents

## Contents

- [Artifact Responsibilities](#artifact-responsibilities)
- [`discussion.md` Contract](#discussionmd-contract)
- [`plan.md` Contract](#planmd-contract)
- [Maintenance and Recovery](#maintenance-and-recovery)
- [User-Facing Reporting](#user-facing-reporting)

## Artifact Responsibilities

<artifact_table>

| Artifact | Purpose | Keep detailed |
|---|---|---|
| `discussion.md` | Durable product, architecture, constraint, risk, and exception decisions | Until decisions are resolved, then compress to durable conclusions |
| `plan.md` | Progressively formed delivery shape, owners, retained agent threads, gates, evidence, and next actions | For active and future work; compress completed steps into evidence-backed summaries |

</artifact_table>

<persistence_boundary>

- At a new-feature lifecycle boundary, create the isolated feature worktree first, then delete stale `discussion.md` and `plan.md` only at that worktree root; never change source-checkout control files or carry state from an earlier feature.
- Preserve both files during continuation and recovery, including compaction, app restart, tool failure, and retained-agent follow-up.
- Create a fresh `discussion.md` at the feature-worktree root immediately for non-trivial feature work when workspace persistence is allowed.
- Create `plan.md` when evidence and decisions begin to form a real implementation approach; expand it progressively instead of inventing a complete plan upfront.
- Require both artifacts before non-trivial implementation when workspace persistence is allowed.
- Treat them as control state, not transcript storage.
- Preserve exact user decisions, failed-gate evidence, accepted exceptions, and residual risk.

</persistence_boundary>

## `discussion.md` Contract

<required_content>

- [ ] User goal and exact constraints
- [ ] Repository evidence that shaped the discussion
- [ ] Architecture decisions and rejected alternatives
- [ ] Exact user-decided values and behaviors
- [ ] Work classification and affected systems
- [ ] Required agent roles and delegation state
- [ ] Architectural root cause for bug work
- [ ] Direct-work override or delegation exception, when applicable
- [ ] Unresolved questions, risks, and accepted residual risk
- [ ] Source checkout, feature worktree, branch, environment-link, port, preview, integration, and publication constraints

</required_content>

<discussion_template>

```md
# Discussion

## Feature Identity

<feature_identity>
- Feature: [current user objective]
- Lifecycle started: [timestamp or task marker]
- Lifecycle classification: `new feature`
- Feature worktree and branch: [absolute path and branch]
- Source checkout and branch: [absolute path and read-only base branch]
</feature_identity>

## Goal

<user_goal>
[Goal and exact constraints]
</user_goal>

## Evidence

<repository_findings>
- [Finding with path, behavior, log, or source]
</repository_findings>

## Decisions

<accepted_decisions>
1. [Decision]
</accepted_decisions>

<rejected_alternatives>
- [Alternative]: [reason grounded in correctness or feasibility]
</rejected_alternatives>

## Classification

<work_classification>
- Classification: `trivial | non-trivial`
- Work type: `research | implementation | review | testing | mixed`
- Affected systems: [systems]
- Root-cause analysis required: `yes | no`
</work_classification>

## Delegation

<delegation_state>
- Required roles: [roles]
- Subagents available: `yes | no | blocked | authorization required`
- Generic execution wording received: [exact text or none]
- Direct-work override: [exact text or none]
- Exception and substitute controls: [details or none]
</delegation_state>

## Open Items

<risks_and_questions>
- [Open question or residual risk]
</risks_and_questions>
```

</discussion_template>

## `plan.md` Contract

<gate_statuses>

Use only these statuses:

| Status | Meaning |
|---|---|
| `Pending` | Required work has not completed |
| `Pass` | Required evidence satisfies the gate, or the gate is proven not required |
| `Fail` | Evidence shows required correction or rework |
| `Blocked` | A demonstrated external, policy, authorization, or capability condition prevents progress |

</gate_statuses>

<gate_order>

| Gate | Must pass before |
|---|---|
| Workspace Isolation | Feature project files change, dependencies install, runtime processes start, or subagents receive repository work |
| Work Classification | A non-trivial plan is finalized |
| Research / Investigation | A research-dependent brief is approved |
| Plan Approval | Production or source-code changes start |
| Delegation | Non-trivial implementation starts |
| Developer | The local pull-request review stage begins when implementation occurred |
| Dual Review | The approved release candidate reaches Tester or signoff begins |
| Testing | Feature Completion is claimed when verification is required |
| Feature Completion | Source-checkout integration preparation begins |
| Integration Readiness | The user is asked to approve transferring the exact feature diff into the clean source checkout |
| Transfer Approval | The feature diff is applied to the source checkout |
| Integration Handoff | The user is asked to inspect the transferred diff and approve commit and push |
| Manual Approval | Final commit or push begins |
| Publish | Feature worktree and integration-lock cleanup begins |

</gate_order>

<plan_template>

```md
# Plan

## Feature Identity

<feature_identity>
- Feature: [same current user objective as discussion.md]
- Lifecycle started: [same timestamp or task marker]
- Lifecycle classification: `new feature | continuation | recovery`
</feature_identity>

## Isolated Workspace

<workspace_state>
- Source checkout, branch, upstream, and original base: [absolute path, branch, upstream, SHA]
- Source checkout initial status: [clean, or exact unresolved state and decision]
- Feature worktree, branch, temporary parent, and current base: [absolute paths, branch, SHAs]
- Environment links: [relative paths and verified ignored state; never values]
- Temporary port map: [service, normal port, temporary port, override mechanism, URL, owner]
- Preview topology: [qualifying command, component coverage, process sessions, health evidence, parity gap]
- Integration lock: [path, owner, state, acquired time]
- Source handoff: [not started | preparing transfer | awaiting transfer approval | transfer approved | transferred for review | commit approved | committed | pushed]
</workspace_state>

## Active Workflow Stage

<workflow_stage>
- Stage: [stage]
- Active reference: [reference]
- Next action: [action]
- Goal decision: `not applicable | awaiting user approval | approved | declined`
</workflow_stage>

## Retained Agents

<agent_lifecycle>
| Role | Thread identity | State | Reuse, replace, or close decision | Reason |
|---|---|---|---|---|
| Developer | [id or not started] | [state] | [decision] | [reason] |
| Code Quality Reviewer | [id or not started] | [state] | [decision] | [reason] |
| Adversarial Reviewer | [id or not started] | [state] | [decision] | [reason] |
| Tester | [id or not started] | [state] | [decision] | [reason] |
</agent_lifecycle>

## Delivery Shape

<delivery_shape>
- Shape: `single implementation set | justified multi-unit delivery`
- Single-set feasibility: [why one bounded Developer assignment is reliable, or the concrete failure it would cause]
- Complexity-gate evidence: [rewrite, migration, large independent subsystem, materially different contracts, necessary risk checkpoint, demonstrated context boundary, or not applicable]
- Artificial splits rejected: [multiple files, frontend and backend, several acceptance criteria, ordinary sequential steps, visibility or convenience, or none]
</delivery_shape>

<implementation_units>
1. [Default complete implementation set, or justified unit with owner, scope, allowed files/modules, dependencies, acceptance criteria, validation, review-boundary id, and status]
<!-- Add another unit only when the implementation complexity gate passes. -->
</implementation_units>

<review_cadence>
- Feature shape and risk: [ordinary feature or fix | qualifying large subsystem or portal | rewrite or high-risk migration; evidence]
- Planned review boundaries: [one review after the complete implementation set | coherent unit groups | every justified unit; exact grouping]
- Boundary rationale: [architecture, integration, risk, and recovery evidence]
- Cadence changes: [new evidence and updated boundary, or none]
</review_cadence>

## Delivery Checkpoint

<delivery_checkpoint>
- Active delivery unit and review boundary: [complete implementation set or justified unit; boundary id and included units]
- Developer assignment boundary: [included work and explicit later-unit exclusions, or not applicable]
- Actual diff and rule evidence: [revision, files, material rules]
- Validation and acceptance evidence: [commands, results, criteria]
- Retained agent states: [ids and states]
- Recovery cursor: [last completed implementation set or unit and exact next action]
</delivery_checkpoint>

## Gate Ledger

### Workspace Isolation Gate

<workspace_gate>
- Status: `Pending | Pass | Fail | Blocked`
- Source checkout and committed base: [path, branch, upstream, SHA, status]
- Feature worktree: [path, branch, temporary parent, Git verification]
- Environment links: [paths, target validation, ignored proof, source untouched]
- Port and preview discovery: [required listeners, temporary map, qualifying preview or fallback]
- Agent workspace contract ready: [yes/no and evidence]
</workspace_gate>

### Work Classification Gate

<classification_gate>
- Status: `Pending | Pass | Fail | Blocked`
- Classification and work type: [value]
- Affected systems: [systems]
- Required project references read: [references]
- Architecture and root-cause analysis required: [yes/no and evidence]
- Evidence: [evidence]
</classification_gate>

### Plan Approval Gate

<plan_approval_gate>
- Status: `Pending | Pass | Fail | Blocked`
- Brief outcome and scope: [summary]
- Implementation approach and delivery shape: [single implementation set by default, or justified multi-unit summary]
- Acceptance criteria: [criteria]
- Material current-feature risks: [risks]
- User approval: [exact approval or pending]
- Material changes since approval: [changes and renewed approval, or none]
- Exceptional goal decision: [not applicable, exact approval, declined, or pending]
</plan_approval_gate>

### Delegation Gate

<delegation_gate>
- Status: `Pending | Pass | Fail | Blocked`
- Required roles: [roles]
- Availability or authorization state: [state]
- Agents started or queued: [identities]
- Direct-work override or exception: [exact text and controls, or none]
- Prompt acceptance criteria complete: [yes/no]
- Role-specific repository-rule startup contract included: [applicable AGENTS.md discovery, routed and independently discovered docs, binding constraints, evidence, and rediscovery triggers]
- Evidence: [evidence]
</delegation_gate>

### Research / Investigation Gate

<research_gate>
- Status and requirement: `Pending | Pass | Fail | Blocked`; [required or not required]
- Direct Project Manager inspection: [files, docs, configuration, logs, codepaths, findings]
- Escalation decision: [no Explorer required | one Explorer required | multiple independent tracks required]
- Unresolved decision-relevant gap: [exact question and why another bounded direct read is insufficient, or none]
- Track independence: [for each additional track, distinct question, evidence or method, and independent decision effect; or not applicable]
- Agents: [Explorer tracks and identities; temporary simulation agents and closure state]
- Questions and track synthesis: [questions, `DO`, and `DO NOT`]
- Challenge hypotheses: [claim, baseline or control, variables, metrics, predeclared thresholds, repetitions or sample boundary, or not applicable]
- Quantitative simulation evidence: [temporary Tester's disposable artifacts and raw measurements; exceptional Developer escalation, demonstrated capability boundary or program requirement, and independent Tester execution when applicable; threshold comparison, uncertainty, limitations, and cleanup; or not applicable]
- Project Manager synthesis and remaining uncertainty: [direct findings, Explorer conclusions, Tester measurements, decision, uncertainty]
</research_gate>

### Developer Gate

<developer_gate>
- Status, requirement, and Developer: [status; required or not required; retained id]
- Active bounded delivery unit and review group: [complete implementation set or justified unit; independent or connected group id]
- Project instructions and engineering practices: [files and rules]
- Changed files and acceptance evidence: [files and evidence]
- Project-native validation, deviations, and unresolved risk: [commands, results, details]
- Worktree confinement: [working directories, port-only state, environment-link exclusion, source-checkout proof]
</developer_gate>

### Dual Review Gate

<review_gate>
- Status and local pull-request revision: [status; revision]
- Code Quality Reviewer: [retained id, rule discovery, review comments, queued or resolved state, approval, residual risk]
- Adversarial Reviewer: [retained id, evidence, review comments, queued or resolved state, approval, residual risk]
- Correction queue: [ordered packages, active Developer package, completed packages]
- Same-revision proof: [evidence that both reviewers approved the same latest diff before Tester received the release candidate]
- Post-testing change review: [Tester failure and prior revision; exact correction files; fresh Code Quality Reviewer id and clean-context evidence; Adversarial re-review; same-revision approvals before retest]
</review_gate>

### Testing Gate

<testing_gate>
- Status, requirement, and Tester: [status; required or not required; retained id]
- Project testing model, production-like preview, ports, and process ownership: [model, commands, mapping, URLs, PIDs, cwd, health evidence, parity gap]
- Checks, simulations, user flows, and resolved failures: [evidence]
- Artifact hygiene, verdict, and residual risk: [git evidence, verdict, risk]
</testing_gate>

### Feature Completion Gate

<completion_gate>
- Status: `Pending | Pass | Fail | Blocked`
- Final diff, required gates, and project-rule evidence: [evidence]
- Hygiene and retained-agent readiness: [temporary or unrelated changes absent; fresh Code Quality Reviewer replacements after Tester-driven changes; retained for possible manual-review correction]
- Documentation state and remaining risk: [state and risk]
</completion_gate>

### Integration Readiness Gate

<integration_readiness_gate>
- Status: `Pending | Pass | Fail | Blocked`
- Feature-worktree readiness: [final revision, diff identity, all gates, stopped processes, reverted port-only state]
- Integration lock: [owner and state]
- Source target: [path, branch, clean proof, fast-forward pull result]
- Updated-base reconciliation: [not required, or isolated reconciliation plus repeated review/testing evidence]
- Proposed transfer: [exact feature-diff identity, prepared target revision, apply-check evidence, and artifact exclusions]
- Transfer request: [exact request shown to user; source remains clean and feature diff unapplied]
</integration_readiness_gate>

### Transfer Approval Gate

<transfer_approval_gate>
- Status: `Pending | Pass | Fail | Blocked`
- Exact proposed transfer: [feature-diff identity, source checkout, source branch, and prepared target revision]
- Explicit user transfer approval: [exact approval or pending]
- Approval freshness: [still applies to every identified input, or invalidated and renewed]
</transfer_approval_gate>

### Integration Handoff Gate

<integration_gate>
- Status: `Pending | Pass | Fail | Blocked`
- Pre-transfer freshness: [clean proof and second fast-forward-only pull immediately before application]
- Concurrent-work protection: [integration-lock verification and proof no other transferred or user-edited source diff exists]
- Updated-target handling: [unchanged, or transfer stopped for reconciliation, repeated gates, and renewed approval]
- Uncommitted transfer: [approved diff identity, resulting source diff identity, artifact and secret exclusions]
- User review state: [ready for manual code inspection and separate commit-and-push approval; not committed or pushed]
</integration_gate>

### Manual Approval Gate

<manual_approval_gate>
- Status: `Pending | Pass | Fail | Blocked`
- Transferred diff shown to user: [revision and evidence]
- User-requested corrections: [none, or correction/re-review/retest/transfer evidence]
- Prior transfer approval: [exact approval proving placement only; not publication authority]
- Explicit commit-and-push approval: [exact approval or pending]
- Pre-commit branch, diff, validation, lock, and remote checks: [evidence]
</manual_approval_gate>

### Publish Gate

<publish_gate>
- Status: `Pending | Pass | Fail | Blocked`
- Final source commit and push: [commit, branch, remote, result]
- Source checkout state: [clean proof]
- Feature process and runtime cleanup: [processes, overrides, environment links]
- Worktree, feature branch, temporary parent, and integration-lock cleanup: [evidence]
- Retained-agent closure: [ids and closure reasons]
</publish_gate>
```

</plan_template>

<failed_gate_rule>

When a gate fails:

1. Preserve the failure and its evidence.
2. Return review corrections through the ordered queue to the retained Developer; return other failures to the retained role agent.
3. Re-run the failed gate.
4. Add passing evidence without deleting the earlier failure history.

</failed_gate_rule>

## Maintenance and Recovery

<maintenance_checklist>

- [ ] Update the active workflow stage, next action, and retained agent states after every handoff.
- [ ] Confirm `discussion.md` and `plan.md` carry the same current-feature identity; never merge state from different features.
- [ ] Keep source checkout, feature worktree, branches, base revisions, environment-link paths, port map, preview sessions, integration lock, and handoff state current.
- [ ] Record exact findings and evidence instead of “done” or “looks good.”
- [ ] Compress completed stage and delivery-unit detail without deleting decisions, failures, or risk.
- [ ] Remove transcript-like commentary that no longer controls execution.
- [ ] After compaction, re-read the thin `SKILL.md`, `references/worktree-isolation.md`, both control documents, and only the active workflow-stage reference; verify every recorded path, branch, port, and process. If integration began, also read `references/integration-handoff.md` and verify its lock and handoff state.

</maintenance_checklist>

## User-Facing Reporting

<reporting_contract>

Apply the mandatory ADHD User Communication rules in `references/operating-model.md`. This clause defines report content only.

Report:

- what changed;
- what was verified;
- what remains;
- what risk remains.

Do not print the internal gate ledger or compliance checklist unless the user asks.

</reporting_contract>
