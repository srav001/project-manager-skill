# Shared Agent Contract

Read this reference before assigning Explorer, Developer, Reviewer, or Tester work. Role references add specialist duties without repeating these rules.

## Leaf-agent boundary

- Treat `explorer`, `developer`, `reviewer`, and `tester` as harness-level logical role names. Never assume where or how their configuration is stored.
- Explicitly select the configured role and set a meaningful runtime thread identity; do not rely on a harness-generated anonymous name.
- Inherit the role configuration's default model, reasoning, permissions, service tier, tools, sandbox, and other execution options. Set only the role, thread identity, task/context, and fields the harness requires to create the assignment. Do not override execution defaults unless the user explicitly instructs it.
- Give every thread a distinct identity and retain it for its stated lifecycle.
- The agent must not spawn or coordinate subagents, contact another role directly, or make user-owned architecture decisions.
- All findings, questions, corrections, and verdicts return through the Project Manager.

## Workspace boundary

Every assignment must provide:

- exact assigned worktree, branch, base, and checkpoint SHA when applicable;
- source checkout path marked read-only and out of scope;
- allowed paths or read-only scope;
- temporary ports, preview URLs, and runtime exclusions when relevant;
- explicit prohibition on creating worktrees, selecting replacement ports, editing symlinked environment files, committing, or pushing;
- instruction to report any command that ran outside the assigned worktree.

An agent must stop when the assigned revision, worktree, ownership, or source boundary cannot be verified.

## Repository-rule discovery

Before its first task action, each agent must:

1. Resolve the repository root and assigned targets.
2. Find and read completely every applicable root and path-scoped `AGENTS.md`.
3. Follow their documentation routes and read role-relevant project documentation.
4. Inspect additional relevant scripts, configuration, CI, and neighboring patterns.
5. Treat discovered rules as binding and report conflicts or missing authority before acting.
6. Report files read, material constraints applied, and targets governed.

Repeat discovery when assigned paths or subsystems change and after compaction or recovery.

## Role-specific discovery emphasis

| Role | Additional evidence to inspect |
|---|---|
| Explorer | Architecture, subsystem, data flow, interfaces, contracts, and research sources |
| Developer | Architecture, engineering, coding, types, validation, security, migration, documentation, build, testing, and CI |
| Code Quality Reviewer | Governing engineering rules and neighboring production patterns for every changed path |
| Adversarial Reviewer | Behavior contracts, public interfaces, security, migration, operations, recovery, integration, and testing rules |
| Tester | Testing and QA guidance, CI, fixtures, runbooks, simulations, Computer Use, preview/build/start configuration, local SDKs/CLIs, browser/network diagnostics, server/application logs, traces, workers/queues, analytics/event telemetry, persisted test data, and connected source paths |

## Evidence standard

- Separate observed facts from inference.
- Cite exact paths, lines, commands, outputs, logs, screenshots, or links where useful.
- Report uncertainty, blocked checks, deviations, and residual risk.
- Do not claim approval outside the assigned role.

## Scope and evidence filter

Every problem report from any role must state:

1. **Scope source:** the exact approved acceptance criterion, binding repository contract, or current supported behavior affected by this change.
2. **Proof:** for behavior, concrete steps from a supported entry point through current code to the failure; for engineering quality, the exact binding rule or established pattern and how the diff violates it.
3. **Evidence and impact:** paths, lines, commands, outputs, traces, or other directly inspected evidence showing the current effect.
4. **Smallest required outcome:** the condition that must become true, not a redesign or implementation prescription.
5. **Classification:** `blocking in-scope defect` | `unrelated existing defect` | `optional additional capability` | `unsupported hypothesis` | `scope decision required`.
6. **Blocked gate:** every proposed `blocking in-scope defect` must state `Blocks: test readiness | release` and prove why it blocks that gate.

Use `test readiness` only for focused-review defects that must be corrected before the first meaningful integrated test. Use `release` for Tester failures and final-review defects that must be corrected before delivery.

The reporting role carries the proof burden. Absence of disproof is not proof. A report missing a scope source, required proof, or concrete evidence is an `unsupported hypothesis`; record it, but never implement it. It may be upgraded only through materially new evidence gathered through the Project Manager or a requirement the user newly approves.

Use `scope decision required` only when an unresolved premise materially prevents correct planning or verification of the current approved request. Do not turn an optional or speculative premise into a user question.

A binding repository contract is an explicit invariant in applicable instructions, documentation, schemas, types, tracked tests, or established public interfaces. Code or package structure does not by itself prove a deployment model, and silence does not authorize either a permissive or hostile assumption.

Only the Project Manager may disposition a report into the active gate's Developer correction package. A broader focused-stage observation may be marked `Deferred-Final`, but it is not a presumed defect and cannot block testing. No subagent may expand the approved product scope.

### Shared boundary example

- **Good:** Report a regression in an existing supported caller affected by the change, with the exact flow and evidence. Restoring that behavior is required correctness.
- **Bad:** Require support for an additional operating model merely because it is possible. Without approved scope or demonstrated reachability, classify it as optional or unsupported; do not implement it or fail the current feature.
