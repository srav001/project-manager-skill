# Isolated Worktrees and Checkpoints

## Workspace invariant

Run every non-trivial project-backed feature in a unique temporary Git worktree. The original checkout is the source checkout and remains unchanged until approved transfer.

Keep discussion, planning, implementation, review, builds, previews, tests, logs, environment links, and temporary runtime state inside the feature workspace or validated temporary locations. Subagents must not create worktrees, commit, push, select ports, or operate in the source checkout.

Project Manager may create local-only checkpoint commits under this protocol. These commits are review identities, never publication commits, and must never be pushed.

## Create the feature workspace

Before creation, record the source checkout, common Git directory, source branch and upstream, source SHA and status, and existing worktrees.

1. Create and validate a unique temporary parent.
2. Create a unique feature integration branch from the recorded source SHA with no upstream.
3. Add the feature integration worktree beneath the temporary parent.
4. Verify the worktree list, branch, `HEAD`, repository root, and common Git directory.
5. Record source checkout, original base SHA, feature branch, integration worktree, and temporary parent in `plan.md`.
6. Run later project commands from the assigned worktree.

Do not initialize Git, stash, reset, clean, switch, copy uncommitted changes from, or otherwise alter the source checkout without explicit authority. If its uncommitted state may affect the feature base, ask the user which committed base governs the work.

## Environment files

- Discover runtime environment files required by repository instructions and scripts.
- Leave tracked files unchanged. Link required ignored or untracked source-checkout environment files at the same relative paths inside each feature worktree.
- Record filenames and paths only; never print secret values.
- Verify every link target and ignored or untracked state.
- Never edit through a symlink, stage it, checkpoint it, transfer it, or push it.
- Prefer process environment variables or CLI flags for temporary ports. Keep any required worktree-only override ignored and recorded for cleanup.

## Temporary ports and preview

Inspect scripts, configuration, proxies, callbacks, workers, emulators, and service URLs to identify the full required port map.

For each listener:

1. Keep repository-default ports reserved for the source checkout.
2. Select a free non-privileged temporary port and recheck it immediately before launch.
3. Never stop or reuse another process to claim a port.
4. Update dependent URLs together.
5. Verify PID, command, process group, working directory, listener, and expected response after launch.

Use the repository preview that most closely represents production. If it covers only part of the system, combine it with established production-like commands for the remainder. Use development mode only when no closer path exists and record the parity gap.

Tester owns the readiness smoke and final runtime judgment. Project Manager owns port allocation and process/worktree identity.

## Checkpoint commits

### Purpose

Use a Git commit SHA as the exact identity for progressive review, final review, correction rounds, testing, recovery, and final-diff transfer. Do not create or compare bespoke content-manifest hashes.

### Creation

Only the Project Manager creates checkpoint commits:

1. Pause the assigned Developer at a reported handoff boundary.
2. Compare the Developer's changed-file list with the approved path allowlist and lane ownership.
3. Stage production paths by explicit pathspec. Never use `git add -A`, `git add .`, or a broad unresolved glob.
4. Inspect the staged diff, staged path list, and excluded runtime/control artifacts.
5. Create an append-only local commit on the temporary branch. Never amend, rebase, configure an upstream, or push.
6. Verify the SHA with Git and compare the committed path list with the Developer report.
7. Verify remaining dirty state is exactly the recorded control documents, disposable artifacts, or temporary runtime differences. Unexpected production state is a hard stop.
8. Record base SHA, checkpoint SHA, purpose, path set, and remaining expected dirty state in `plan.md`.

Keep `discussion.md`, `plan.md`, secrets, environment links, logs, caches, builds, disposable smokes, and temporary port-only changes outside checkpoint commits. Prefer disposable artifacts outside the repository or in a proven ignored location.

An unresolved identity mismatch invalidates every dependent verdict. Recover through Git object verification before work resumes.

### Exact review worktree

When Developer will continue beyond a progressive checkpoint, create a detached review worktree at that checkpoint SHA and give it to the Reviewer. Never ask a Reviewer to inspect a moving Developer working tree while claiming it represents an older SHA.

Both final Reviewers may share one quiesced read-only review worktree when it points exactly to the assigned SHA. Do not move or remove a review worktree while an assigned review is active.

### Corrections

The integration Developer edits above the latest checkpoint in the integration worktree. Project Manager creates a new append-only checkpoint after each consolidated correction round. Reviewers receive the prior and current SHAs, changed paths, their open findings, and the full final target when required.

## Qualified Developer lanes

After the independence gate passes:

1. Create one branch and worktree per lane from the approved base or frozen contract checkpoint.
2. Record exclusive ownership, dependencies, and integration order.
3. Give each lane exactly one Developer writer.
4. Create lane checkpoint commits using the same explicit-path protocol.
5. Allow Code Quality review of a completed lane checkpoint while another genuinely independent lane continues.
6. Freeze completed lanes before integration.

Project Manager integrates lane branches into the integration worktree in the declared order. Any merge conflict falsifies the independence gate: abort the merge, preserve evidence, and assign resolution to the designated integration Developer in single-writer mode. After merge, run project-native build/type/smoke validation before creating the integrated review checkpoint.

Reviewers and Tester approve the integrated diff, not a collection of lane approvals. After integration, lane branches do not advance.

## Agent assignment boundary

Every agent assignment must follow `agent-contract.md` and identify the exact worktree and revision. Developer edits only its owned worktree. Reviewers inspect only the assigned checkpoint. Tester runs only the assigned integration worktree and verified port map. Explorer remains read-only.

## Recovery

Recover from Git-verifiable state in `plan.md`:

- source checkout, original base, integration branch/worktree, lane and review worktrees;
- latest checkpoint and its purpose;
- expected dirty files above each checkpoint;
- environment links, port map, preview processes, and owners;
- active agents, work items, review findings, and next action.

Verify each path, branch, SHA, status, process, and port before mutation. A dirty tree above a checkpoint is in-progress work, not a new checkpoint.

## Cleanup safety

Do not remove a worktree or local branch until its work is either published or explicitly abandoned. Before removing checkpoint branches, prove the approved transferred source diff equals the intended `original-base..final-checkpoint` diff. Use Git worktree removal and delete only validated task-owned paths and branches.
