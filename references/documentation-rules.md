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
| `plan.md` | Progressively formed phases, owners, retained agent threads, gates, evidence, and next actions | For active and future work; compress completed steps into evidence-backed summaries |

</artifact_table>

<persistence_boundary>

- Create or update `discussion.md` immediately for non-trivial feature work when workspace persistence is allowed.
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

</required_content>

<discussion_template>

```md
# Discussion

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
| `Pass` | Required evidence satisfies the gate, or the phase is proven not required |
| `Fail` | Evidence shows required correction or rework |
| `Blocked` | A demonstrated external, policy, authorization, or capability condition prevents progress |

</gate_statuses>

<gate_order>

| Gate | Must pass before |
|---|---|
| Work Classification | A non-trivial plan is finalized |
| Research / Investigation | A research-dependent brief is approved |
| Plan Approval | Production or source-code changes start |
| Delegation | Non-trivial implementation starts |
| Developer | The local pull-request review phase begins when implementation occurred |
| Dual Review | The approved release candidate reaches Tester or signoff begins |
| Testing | Completion is claimed when verification is required |
| Completion | Readiness or completion is reported |

</gate_order>

<plan_template>

```md
# Plan

## Active Phase

<phase_state>
- Phase: [phase]
- Active reference: [reference]
- Next action: [action]
- Goal decision: `not applicable | awaiting user approval | approved | declined`
</phase_state>

## Retained Agents

<agent_lifecycle>
| Role | Thread identity | State | Reuse, replace, or close decision | Reason |
|---|---|---|---|---|
| Developer | [id or not started] | [state] | [decision] | [reason] |
| Code Quality Reviewer | [id or not started] | [state] | [decision] | [reason] |
| Adversarial Reviewer | [id or not started] | [state] | [decision] | [reason] |
| Tester | [id or not started] | [state] | [decision] | [reason] |
</agent_lifecycle>

## Phases

<execution_phases>
1. [Context-sized phase, owner, scope, allowed files/modules, dependencies, acceptance criteria, validation, review-boundary id, status]
2. [Context-sized phase, owner, scope, allowed files/modules, dependencies, acceptance criteria, validation, review-boundary id, status]
</execution_phases>

<review_cadence>
- Feature shape and risk: [small feature | large subsystem or portal | rewrite or high-risk migration; evidence]
- Planned review boundaries: [after all phases | coherent phase sets | every phase; exact grouping]
- Boundary rationale: [architecture, integration, risk, and recovery evidence]
- Cadence changes: [new evidence and updated boundary, or none]
</review_cadence>

## Phase Checkpoint

<phase_checkpoint>
- Active phase and review boundary: [phase; boundary id and included phase set]
- Developer assignment boundary: [included work and explicit later-phase exclusions]
- Actual diff and rule evidence: [revision, files, material rules]
- Validation and acceptance evidence: [commands, results, criteria]
- Retained agent states: [ids and states]
- Recovery cursor: [last completed phase and exact next action]
</phase_checkpoint>

## Gate Ledger

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
- Implementation approach and major phases: [summary]
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
- Evidence: [evidence]
</delegation_gate>

### Research / Investigation Gate

<research_gate>
- Status and requirement: `Pending | Pass | Fail | Blocked`; [required or not required]
- Agents: [Explorer tracks and identities; temporary simulation agents and closure state]
- Questions and track synthesis: [questions, `DO`, and `DO NOT`]
- Evidence and remaining uncertainty: [codebase, sources, simulations, uncertainty]
</research_gate>

### Developer Gate

<developer_gate>
- Status, requirement, and Developer: [status; required or not required; retained id]
- Active bounded phase and review group: [phase; independent or connected group id]
- Project instructions and engineering practices: [files and rules]
- Changed files and acceptance evidence: [files and evidence]
- Project-native validation, deviations, and unresolved risk: [commands, results, details]
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
- Project testing model and evidence: [model and evidence]
- Checks, simulations, user flows, and resolved failures: [evidence]
- Artifact hygiene, verdict, and residual risk: [git evidence, verdict, risk]
</testing_gate>

### Completion Gate

<completion_gate>
- Status: `Pending | Pass | Fail | Blocked`
- Final diff, required gates, and project-rule evidence: [evidence]
- Hygiene and retained-agent closure: [temporary or unrelated changes absent; fresh Code Quality Reviewer replacements after Tester-driven changes; closure state]
- Documentation state and remaining risk: [state and risk]
</completion_gate>
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

- [ ] Update the active phase, next action, and retained agent states after every handoff.
- [ ] Record exact findings and evidence instead of “done” or “looks good.”
- [ ] Compress completed phase detail without deleting decisions, failures, or risk.
- [ ] Remove transcript-like commentary that no longer controls execution.
- [ ] After compaction, re-read the thin `SKILL.md`, both control documents, and only the active phase reference.

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
