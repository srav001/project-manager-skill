# Project Manager Role

## Before using this skill

Create these four logical subagent roles in your harness of choice—Codex, Claude Code, OpenCode, or another multi-agent harness:

| Configured role name | Runtime thread identity | Required capability |
|---|---|---|
| `explorer` | `explorer_<scope>` | Read-only codebase and external evidence investigation |
| `developer` | `developer` or `developer_<lane>` | Scoped implementation and Developer-owned validation |
| `reviewer` | `code_quality_reviewer` and `adversarial_reviewer` | Read-only technical review; must support two independent instances |
| `tester` | `tester` | Developer-capable testing, simulations, UI/API/runtime verification, disposable diagnostics, and root-cause analysis without production-code edits |

The skill instantiates `reviewer` twice as `code_quality_reviewer` and `adversarial_reviewer`. It may instantiate multiple `developer` threads when fixed contracts and exclusive ownership permit parallel lanes.

Agent configuration filenames, directories, schemas, models, and permission syntax belong to the selected harness. This skill does not prescribe or assume any agent-file path.

When creating an agent, explicitly select the configured role name and assign the runtime thread identity shown above. Inherit the role configuration's default model, reasoning, permissions, service tier, tools, and other execution options. Do not restate or override those defaults in the skill's task assignment.

## Required harness behavior

- The primary agent invokes this skill and acts as the Project Manager.
- The Project Manager can create, retain, message, and close named agent threads.
- Every subagent is a leaf and cannot spawn or coordinate other agents.
- Repeated corrections, re-reviews, and retests can continue in the same retained thread.
- The harness can run independent ready assignments concurrently; fewer available slots reduce parallelism without changing the gates.

After configuring the four roles, invoke `$project-manager-role` from the primary agent.
