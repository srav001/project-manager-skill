# Source Integration and Publication

## Contents

- [Handoff Readiness](#handoff-readiness)
- [Integration Lock](#integration-lock)
- [Transfer for Manual Review](#transfer-for-manual-review)
- [Manual Approval](#manual-approval)
- [Commit and Push](#commit-and-push)
- [Cleanup](#cleanup)
- [Recovery](#recovery)

## Handoff Readiness

<handoff_readiness>

Begin integration only after the approved plan is complete and the feature worktree passes Developer, both Reviewer, Tester, and Project Manager evidence gates.

Before touching the source checkout:

- stop every preview, server, worker, watcher, emulator, and test process started for the feature after proving ownership;
- revert every tracked port-only change and remove worktree-only port overrides that could enter the diff;
- prove environment symlinks, secrets, logs, caches, builds, and temporary artifacts are absent from the transferable feature diff;
- run required final validation in the feature worktree;
- capture an exact binary-capable feature diff, including additions, deletions, renames, modes, and intended new files;
- record the final feature-worktree revision and diff identity.

</handoff_readiness>

## Integration Lock

<integration_lock>

Serialize final handoffs across Project Managers with one repository-level integration lock stored in the Git common directory, outside tracked files.

- Acquire the lock before checking or changing the source checkout and hold it through manual review, final commit, and push.
- Record the owning task, feature branch, worktree, and acquisition time without secrets.
- If another valid lock exists, do not remove it or modify the source checkout. Report the owner and wait for that integration to finish.
- After a crash, verify the recorded owner, worktree, source status, and remote state before asking to clear a possibly stale lock. Never assume it is stale.

</integration_lock>

## Transfer for Manual Review

<source_transfer>

With the integration lock held:

1. Verify the exact source checkout and target branch. Require `git status --porcelain` to be empty; do not stash, discard, overwrite, or absorb user or another Project Manager's work.
2. Pull the target branch with fast-forward-only behavior. Do not force, rebase the source checkout, or create an implicit merge commit.
3. If the target advanced since the feature worktree's base, reconcile the feature against the new target in the isolated worktree or a separate isolated integration worktree, then repeat affected review and testing gates. Keep the source checkout clean during reconciliation.
4. Validate the complete feature patch against the updated source checkout before applying it. If it does not apply cleanly, leave the source unchanged and return to isolated reconciliation.
5. Apply the exact feature diff to the source working tree without committing.
6. Prove the resulting source diff matches the approved feature diff and contains no environment link, secret, temporary port change, log, cache, build artifact, or unrelated file.
7. Run any non-mutating handoff checks required to prove the transfer is intact.
8. Tell the user the uncommitted source diff is ready for manual code inspection. Do not commit or push yet.

If the source checkout changes at any point, stop. Never overwrite or combine concurrent work silently.

</source_transfer>

## Manual Approval

<manual_approval_gate>

- Wait for explicit user approval of the transferred source diff.
- Treat requested manual-review changes as a new correction. First verify whether the source diff still exactly matches the Project Manager's transfer. If unchanged, remove only that transferred diff with a validated reversible operation so the source becomes clean; then make corrections in the isolated worktree, rerun required review and testing gates, and transfer the new exact diff again.
- If the user or another process edited the transferred source diff, do not overwrite, reverse, or absorb those edits. Report the difference and ask how the user wants the edits incorporated into the isolated feature branch.
- Keep the integration lock while the transferred diff awaits review or correction.

</manual_approval_gate>

## Commit and Push

<publication_gate>

- Immediately before commit, verify the source branch, expected diff, integration lock, remote state, and required repository validation.
- If the remote target advanced, safely remove only the unchanged transferred diff, update and reconcile in isolation, rerun affected gates, transfer again, and obtain approval of the updated source diff. If the transferred diff changed during manual review, stop and ask before altering it.
- After explicit approval, create the final commit in the source checkout and push normally.
- Verify the remote contains the intended commit and that no unapproved file entered the commit.
- Never force-push unless the user separately and explicitly requests it.

</publication_gate>

## Cleanup

<cleanup_sequence>

After the final push succeeds:

1. verify the pushed commit and clean source checkout;
2. confirm all feature-owned processes are stopped;
3. remove environment symlinks and disposable runtime overrides from the exact validated feature worktree;
4. remove the Git worktree with Git's worktree mechanism;
5. delete only the validated temporary feature branch and temporary parent when no retained work remains;
6. release the integration lock;
7. close retained role agents and record cleanup evidence.

Do not remove the feature worktree before a successful push or an explicit user abandonment decision. Never delete a broad, unresolved, or user-owned path.

</cleanup_sequence>

## Recovery

<integration_recovery>

When integration has started, recover these fields from `plan.md` and verify them against live state before any Git mutation:

- source checkout, target branch, upstream, and remote state;
- feature worktree, branch, current base, final revision, and diff identity;
- integration-lock owner and acquisition time;
- stopped or still-running feature processes;
- source-transfer state and whether the source diff still exactly matches the transfer;
- manual-approval, commit, push, and cleanup state.

Do not clear a lock, reverse a source diff, retry a transfer, commit, push, or remove a worktree from remembered state alone.

</integration_recovery>
