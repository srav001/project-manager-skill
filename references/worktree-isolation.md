# Isolated Worktree Delivery

## Contents

- [Workspace Contract](#workspace-contract)
- [Create the Feature Worktree](#create-the-feature-worktree)
- [Link Environment Files](#link-environment-files)
- [Allocate Temporary Ports](#allocate-temporary-ports)
- [Run the Production-Like Preview](#run-the-production-like-preview)
- [Agent Workspace Boundaries](#agent-workspace-boundaries)
- [Subagent Repository-Rule Startup](#subagent-repository-rule-startup)
- [Recovery](#recovery)

## Workspace Contract

<isolation_invariant>

Every project-backed feature lifecycle handled by this skill runs in its own unique temporary Git worktree and feature branch. This permits multiple Project Managers to work on the same repository without sharing files, processes, ports, agent state, or unfinished diffs.

The original project checkout is the **source checkout**. Treat it as read-only until the feature passes every implementation, review, and testing gate and is ready for manual user review.

All of the following belong in the feature worktree:

- `discussion.md` and `plan.md`;
- repository exploration after initialization;
- dependency installation and generated local state;
- Developer changes and corrections;
- Reviewer inspection;
- Tester execution, simulations, builds, previews, and evidence;
- temporary port overrides and runtime logs.

Keep feature changes uncommitted in the temporary worktree. No subagent may commit or push. The final approved commit is created only in the source checkout under `references/integration-handoff.md`.

</isolation_invariant>

<no_fallback_rule>

- Do not perform feature work in the source checkout because worktree creation is inconvenient or fails once.
- If the folder is not a Git repository, the source revision or branch cannot be resolved, or a worktree cannot be created after reasoned attempts, report the demonstrated blocker and request the minimum decision required.
- Do not initialize Git, move user changes, stash, reset, clean, switch branches, or alter the source checkout without explicit authority.

</no_fallback_rule>

## Create the Feature Worktree

<source_discovery>

Before creating anything, record:

1. source checkout absolute path;
2. repository common Git directory;
3. checked-out source branch and upstream;
4. source branch commit SHA;
5. source checkout status, including untracked files;
6. existing worktrees and their branches.

Read applicable source-root `AGENTS.md` before worktree creation. After creation, re-run instruction discovery from inside the feature worktree so path-scoped rules govern every later action.

</source_discovery>

<creation_sequence>

1. Create a unique temporary parent with the platform's safe temporary-directory mechanism and validate its resolved absolute path.
2. Create a unique feature branch from the recorded source-branch commit. Use a collision-resistant name that identifies the Project Manager feature without reusing another worktree's branch.
3. Add a Git worktree beneath the validated temporary parent for that feature branch.
4. Verify `git worktree list --porcelain`, the worktree branch, `HEAD`, repository root, and Git common directory.
5. Record the source checkout, source branch, original base SHA, feature branch, worktree path, and temporary parent in `plan.md` as soon as the fresh control files exist.
6. Run all later repository commands with the feature worktree as the working directory.

Source-checkout changes do not automatically become part of the feature branch. If the source checkout is dirty and those changes may affect the requested feature, do not copy or infer them; ask which committed base should govern the work.

</creation_sequence>

## Link Environment Files

<environment_inventory>

Discover the environment files required by the repository's scripts, project instructions, packages, frontend, backend, workers, and preview command.

- Tracked environment files already exist in the Git worktree; do not replace them with symlinks.
- Treat example, sample, template, and documentation environment files as tracked project guidance, not runtime secrets.
- For required untracked or ignored runtime environment files in the source checkout, create symlinks at the same relative paths inside the feature worktree.
- Record environment filenames and relative paths only. Never print, copy into reports, or expose their values.
- Validate every symlink target and confirm every link remains untracked or ignored.

</environment_inventory>

<environment_safety>

- Never edit a symlinked environment file: writing through it would modify the source checkout's real file.
- Never stage, commit, transfer, or push environment symlinks or secret-bearing runtime files.
- Apply temporary port values through process environment variables or command-line flags whenever supported.
- If the project requires a worktree-local override file, create a separate ignored non-symlink override and record it for cleanup.
- If a required port can only be changed in a tracked file, isolate that as a temporary port-only diff and prove it is reverted before any Reviewer package or source handoff.

</environment_safety>

## Allocate Temporary Ports

<port_discovery>

Inspect project instructions, package scripts, preview/build/start configuration, environment-variable names, proxy and callback URLs, frontend and backend listeners, workers, emulators, and neighboring services. Identify every listener and cross-service URL required by the real feature flow; do not assume one port or a fixed framework default.

</port_discovery>

<port_allocation>

For each required listener:

1. Reserve every project-defined normal port for the source checkout. Probe nearby non-privileged ports and select a free temporary port that differs from the normal port.
2. Inspect listeners and owning processes. Never terminate, reconfigure, or reuse another process to claim a port.
3. Prevent collisions within the feature's own multi-service port map.
4. Recheck each candidate immediately before launch because another Project Manager may claim it after the first probe.
5. After launch, verify the owning PID, command, process group, working directory, and expected HTTP or protocol response.
6. If a candidate is claimed or serves the wrong worktree, stop only the process this feature started, allocate another free port, update the whole mapping, and retry.

Record a table in `plan.md` containing service, normal port, temporary port, override mechanism, URL, owning process, and verification evidence. All agents use this exact mapping.

</port_allocation>

<port_change_boundary>

- Prefer runtime overrides over tracked edits.
- Keep port-only changes separate from feature changes and out of review packages, commits, transfers, and pushes.
- Update dependent callback, proxy, API, WebSocket, asset, and browser URLs together; a free listener port alone does not prove the application works.
- Do not claim a port is usable until the correct feature-worktree process serves the expected response.

</port_change_boundary>

## Run the Production-Like Preview

<preview_selection>

Choose the repository-defined runtime that most closely represents production:

1. If a project preview command builds and serves every affected frontend and backend component together, always use it for integrated verification.
2. If `preview` covers only part of the affected system, combine it only with the project's established production-like commands for the missing components.
3. If no qualifying preview exists, use the repository's established build plus production-like start or serve workflow.
4. Use a development server only when the repository has no production-like local path; record the exact parity gap and resulting residual risk.

Do not invent a new preview system merely for this skill.

</preview_selection>

<preview_execution>

- Run builds and previews from the feature worktree with the recorded temporary port map.
- Use persistent process sessions that the Project Manager can monitor and stop.
- Verify every started process belongs to the feature worktree before relying on its output or stopping it.
- Probe every required endpoint and exercise the real application flow; do not treat a successful build or open listener as sufficient.
- Capture the preview command, environment override names without secret values, process ownership, URLs, logs, and health evidence in `plan.md`.
- Tester must use this same verified preview topology unless its assignment proves a project-native reason to restart it.

</preview_execution>

## Agent Workspace Boundaries

<agent_workspace_contract>

Every subagent prompt must include:

- exact feature-worktree path and feature branch;
- source checkout path marked read-only and out of scope;
- applicable phase or review revision;
- temporary port mapping and verified preview URLs when runtime work is relevant;
- explicit prohibition on editing, building, installing, testing, or starting processes in the source checkout;
- instruction to report any command that ran outside the feature worktree;
- explicit prohibition on committing or pushing from the feature branch.

Explorer reads feature-worktree code. Developer edits only there. Both Reviewers inspect only the submitted feature-worktree revision and feature diff. Tester runs only the assigned feature-worktree preview and ports. No subagent may create another worktree or change the port map; those remain Project Manager responsibilities.

</agent_workspace_contract>

## Subagent Repository-Rule Startup

<shared_rule_discovery_contract>

Before its first task action, every Explorer, Developer, Reviewer, and Tester must:

1. Resolve the assigned feature-worktree repository root and target paths.
2. Locate and read completely every root and path-scoped `AGENTS.md` governing those targets.
3. Follow every documentation route in those instructions and read the documents relevant to its role and assignment.
4. Search for additional role-relevant project documentation, scripts, configuration, CI, and neighboring conventions rather than assuming the prompt contains every rule.
5. Treat discovered project rules as binding. Stop and report conflicting instructions, missing required documents, or unclear authority before acting.
6. Report the files read, the material constraints applied, and the targets governed by them.

Repeat discovery when the assigned paths or subsystem change, when a new phase introduces different governed files, and after compaction or recovery before resuming work. Reading instructions once for an earlier scope is not sufficient evidence for a later scope.

</shared_rule_discovery_contract>

<role_specific_documentation>

| Role | Required project evidence in addition to applicable `AGENTS.md` |
|---|---|
| Explorer | Architecture, subsystem, data-flow, interface, contract, and research documentation relevant to the question |
| Developer | Architecture, engineering, coding, style, type, validation, security, migration, documentation, testing, build, and CI rules governing the assigned files |
| Code Quality Reviewer | The same governing engineering and code rules independently discovered, plus neighboring production patterns used for exact diff comparison |
| Adversarial Reviewer | Behavior contracts, public interfaces, testing, security, migration, operations, recovery, and integration documentation relevant to reachable current-feature paths |
| Tester | Testing and QA guidance, test configuration, CI, fixtures, runbooks, simulations, Computer Use guidance, and preview/build/start/serve documentation |

</role_specific_documentation>

## Recovery

<recovery_contract>

On compaction, restart, or interruption, recover from `plan.md` before running project commands:

- source checkout and source branch;
- original and current base revisions;
- feature worktree, branch, and temporary parent;
- environment symlink paths without values;
- port map, processes, and preview sessions;
- active phase, retained agents, and review revision.

Verify each recorded path, branch, process, and port against live state. Resume from the last evidence-backed checkpoint. Do not create a replacement worktree merely because the current task turn ended. If source integration has begun, load `references/integration-handoff.md` and follow its recovery contract.

</recovery_contract>
