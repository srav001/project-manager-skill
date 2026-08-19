# Source Integration and Publication

## Readiness

Begin only after the exact feature SHA passes Developer Handoff, focused same-SHA dual `TEST READY`, integrated Tester verification, final same-SHA dual `RELEASE PASS`, Regression Confirmation when required, and Feature Completion.

Before touching the source checkout:

- stop feature-owned processes after verifying ownership;
- remove or revert temporary port/runtime differences;
- prove secrets, environment links, logs, caches, builds, and disposable artifacts are outside the transferable diff;
- record original base, final checkpoint SHA, and exact binary-capable `base..final-SHA` diff;
- for multi-lane work, prove all intended lane changes are present in that one integrated SHA.

Project Manager reconciles evidence and identity only; it does not perform another code review or test pass.

## Approval firewall

No earlier authority crosses into source integration. Plan approval, full work or testing permission, autonomous-execution permission, durable-goal authority, and instructions such as "finish it" or "do not ask routine permission" are invalid as Transfer Approval or Commit/Push Approval.

A valid Transfer Approval:

- is requested only after Integration Readiness records the exact final diff, source checkout, branch, prepared target SHA, clean status, and apply-check evidence;
- explicitly authorizes applying that exact diff to that exact source checkout without committing; and
- applies only to the current gate and bound identities.

A valid Commit/Push Approval:

- is requested only after the approved diff is present uncommitted in the source checkout and available for user inspection;
- explicitly authorizes committing and pushing that exact inspected source diff; and
- cannot be bundled with, inferred from, or preapproved alongside Transfer Approval.

Examples:

- Invalid for delivery: "You have full permission; finish the feature and test everything." Continue isolated work and testing, then stop at Transfer Approval.
- Valid transfer: "Transfer the prepared diff for C21 into `feat/workflow` without committing." Transfer only after rechecking the bound identities.
- Invalid for commit/push: "Transfer it, commit it, and push when done." Treat only the transfer portion as a request at the transfer gate; ask separately after uncommitted inspection.
- Valid commit/push: "I inspected the transferred uncommitted diff. Commit and push it." Reverify the bound diff and remote before proceeding.

## Integration lock

Use one repository-level lock in the common Git directory to serialize source handoffs.

- Acquire it before source-checkout inspection or mutation and hold it through manual review, commit, and push.
- Record task, feature branch, integration worktree, final SHA, and acquisition time.
- If another valid owner exists, do not alter the source checkout or lock.
- After interruption, verify owner, worktree, source status, and remote state before treating a lock as stale.

## Prepare and request transfer approval

With the lock held:

1. Verify exact source checkout and target branch; require a clean status.
2. Fast-forward-only pull the target. Do not stash, discard, rebase, force, or absorb concurrent work.
3. If the target advanced from the feature base, reconcile against it in isolation and repeat every review or test gate affected by the changed base.
4. Validate that the complete final-checkpoint diff applies to the prepared source target.
5. Record final SHA, diff identity, source branch, prepared target SHA, clean status, and apply-check evidence.
6. Ask the user to approve transferring that exact diff into that exact source checkout without committing. Do not treat any earlier broad or advance permission as this approval.

Keep the source checkout unchanged while approval is pending. Any change to diff, checkout, branch, or target SHA invalidates approval.

## Transfer for manual inspection

After explicit transfer approval:

1. Recheck the integration lock, clean source status, and fast-forward-only pull immediately before application.
2. If source state or target SHA changed, stop, reconcile in isolation, repeat affected gates, and request renewed transfer approval.
3. Apply the exact approved binary-capable diff without committing.
4. Prove the source working diff equals the approved `base..final-SHA` feature diff and contains no excluded artifact.
5. Run only required non-mutating transfer-integrity checks.
6. Tell the user the uncommitted diff is ready for inspection and ask separately for commit-and-push approval.

Never combine or overwrite user or another task's source changes.

## Manual-review corrections

If the user requests changes:

- first verify whether the source diff still exactly equals the transferred feature diff;
- if unchanged, reversibly remove only that transferred diff, return to the isolated integration worktree, assign corrections to the retained integration Developer, and repeat affected checkpoint/review/test/transfer gates;
- if the source diff was edited, do not overwrite or reverse it; ask how the user wants those edits incorporated.

Keep the lock while an unchanged transferred diff awaits inspection or correction.

## Commit and push

Only after explicit commit-and-push approval requested after uncommitted source inspection:

1. Reverify branch, exact diff, lock, validation, and remote freshness.
2. If remote advanced, remove only an unchanged transferred diff, reconcile and regate in isolation, then obtain renewed transfer and commit approvals.
3. Commit from the source checkout and push normally.
4. Verify the remote contains the intended commit and no unapproved path entered it.

Never force-push without a separate explicit request.

## Cleanup

After successful push:

1. Verify remote commit and clean source checkout.
2. Stop all feature-owned processes.
3. Remove environment links and disposable overrides from validated feature worktrees.
4. Prove the published source diff equals the intended original-base-to-final-checkpoint diff.
5. Remove review, lane, and integration worktrees; delete only validated local task branches and temporary parents; release the lock; close retained agents.

Local checkpoint commits are recoverable work until publication equality is proven. Do not delete them earlier.

## Recovery

Before any Git mutation after interruption, verify from `plan.md`:

- source checkout, target branch, upstream, status, and remote state;
- original base, final checkpoint SHA, and exact diff identity;
- integration/lane/review worktrees and branches;
- lock owner and acquisition time;
- transfer approval, second freshness check, source-diff equality, commit approval, commit, push, and cleanup state.

Never clear a lock, reverse a source diff, retry transfer, commit, push, or remove a worktree from remembered state alone.
