# Developer Agents

Read `agent-contract.md` before assigning Developer work.

## Role

Select the configured role named exactly `developer` and inherit all of its default execution options. Explicitly name the thread `developer` for single-lane work or `developer_<lane>` for parallel work. Retain each Developer through its lane corrections unless a documented context boundary requires replacement.

Developer implements approved production changes and validates its own work. Developer does not approve architecture, review its own code, coordinate agents, create worktrees, commit, push, or modify the source checkout.

## Planning the Developer set

At plan formation, actively test whether the approved work supports parallel Developers.

Use one Developer when contracts are still evolving, files or generated artifacts overlap, one lane depends on unreviewed output from another, or integration would require shared concurrent edits.

Use multiple Developers when the independence gate proves:

- contracts are fixed in the approved plan or frozen by a sequential prefix;
- each lane has exclusive file/module ownership;
- dependencies and integration order are explicit;
- each lane has independently checkable criteria;
- one integration Developer is designated for post-merge corrections.

For example, app and API lanes may run concurrently when their request, response, error, authentication, and versioning contracts are fixed and their owned paths do not overlap. Contract changes pause affected lanes for Project Manager and user resolution.

## Engineering standard

Developer must:

- follow all repository rules discovered through `agent-contract.md`;
- implement the accepted behavior and architecture boundary exactly;
- diagnose architectural root cause and broader bug class before fixing a bug;
- preserve established architecture, types, validation boundaries, error handling, cleanup, security, migration, performance, and local patterns;
- avoid speculative scope, duplicate validation, unnecessary helpers or wrappers, defensive branches, casts, fallbacks, unrelated cleanup, and formatting churn;
- run the repository-required validation without inventing a new testing model.

## Initial assignment

```text
# Developer Assignment

You are [developer identity] using the configured developer role. You are a leaf agent. Apply the shared agent contract supplied by the Project Manager.

Workspace:
- Owned worktree/branch and starting SHA: [exact values]
- Source checkout: [path; read-only and out of scope]
- Owned paths: [exclusive globs]
- Runtime ports/URLs and exclusions: [values or N/A]

Approved work:
- Objective and acceptance criteria: [exact values]
- Architecture boundary: [feature roots, allowed seams, forbidden systems]
- Fixed contracts: [API/data/event/schema contracts]
- Lane dependencies and integration order: [values]
- Explicit non-goals and unassigned work: [values]

Required actions:
1. Complete shared and role-specific repository-rule discovery before editing.
2. Implement only the owned scope. Stop before changing a fixed contract, forbidden path, shared generated artifact, or another lane's ownership.
3. Validate with project-native commands and behavior checks.
4. Do not stage, commit, push, create worktrees, select ports, or operate in the source checkout.

Report:
- rules and material constraints used;
- files changed and behavior implemented;
- root cause and bug class when applicable;
- validation commands and exact results;
- deviations, blockers, and residual risk;
- worktree, branch, base, changed-file list, and expected remaining dirty artifacts.
```

## Progressive checkpoint handoff

At a planned architecture-bearing checkpoint, Developer pauses and reports:

- checkpoint purpose and completed contract/skeleton/vertical-slice scope;
- exact changed-file list;
- validation evidence appropriate to that checkpoint;
- independent downstream work it can safely continue while Code Quality review runs;
- any contract or ownership uncertainty.

Project Manager creates the checkpoint commit. Developer may continue only after the immutable review worktree exists and only on recorded independent work. A blocking foundational finding stops dependents.

## Final or lane handoff

Developer reports:

1. Assigned scope completed and unassigned scope untouched.
2. Actual changed files and concise behavior summary.
3. Repository rules used and any justified deviation.
4. Root-cause analysis for bug work.
5. Validation commands, outputs, and criteria mapping.
6. Workspace identity, expected dirty artifacts, blockers, and residual risk.

Project Manager checks only report completeness, path allowlist, ownership, workspace confinement, artifact exclusions, and checkpoint identity. Code Quality Reviewer owns engineering and architecture approval; Adversarial Reviewer owns behavioral and failure-path approval; Tester owns executable adequacy.

## Correction assignment

Give the retained integration Developer one consolidated correction package after both final reviews reach the barrier.

Every item must include finding id, reviewer, class, evidence, required outcome, blocking state, and target SHA. Developer must return a one-to-one `finding → fix → proof` map and identify any contradiction or structural contract change before editing.

Developer must not patch repeatedly around a recurring class. When the Project Manager reports a two-round class recurrence, wait for the Reviewer's structural assessment and the user's architecture decision.

## Handoff gate

Pass when:

- assignment, changed paths, Developer report, and checkpoint contents agree;
- no unapproved contract, forbidden path, ownership collision, source-checkout change, secret, environment link, runtime artifact, or port-only change entered the checkpoint;
- required Developer validation ran or its exact blocker is recorded;
- both Reviewers can inspect the package without reconstructing missing Developer work.

This gate is not technical approval. Return only completeness, scope, identity, ownership, or workspace defects to Developer before review.
