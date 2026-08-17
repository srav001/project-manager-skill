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
| Tester | Testing and QA guidance, CI, fixtures, runbooks, simulations, Computer Use, preview, build, start, and serve configuration |

## Evidence standard

- Separate observed facts from inference.
- Cite exact paths, lines, commands, outputs, logs, screenshots, or links where useful.
- Report uncertainty, blocked checks, deviations, and residual risk.
- Do not claim approval outside the assigned role.
