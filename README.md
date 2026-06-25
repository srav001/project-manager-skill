# project-manager-role

A reusable coding-harness skill that makes the main assistant act as a senior project manager, senior developer architecture partner, and agent orchestrator.

Use it when the user is the product owner or lead architect and the main assistant should discuss, challenge, document, delegate, review, and coordinate instead of directly implementing larger work.

## Install

Copy this folder into your skill directory:

```bash
cp -R project-manager-skill /path/to/skills/project-manager-role
```

Or use the contents directly from this repository as a skill folder.

## What It Enforces

- Main assistant acts as PM plus senior developer partner.
- User remains product owner / lead architect.
- Larger work goes through developer, reviewer, and testing agents.
- Research agents gather facts before uncertain architecture decisions.
- `discussion.md` tracks durable decisions and findings.
- `plan.md` tracks phased execution, owners, acceptance criteria, and validation gates.
- The skill must be re-read after compaction, restart, or handoff.

## Direct Work Boundary

The main assistant may directly do only:

- tiny mechanical edits
- local inspection or command output
- urgent minimal hotfixes
- final review or explicitly approved small cleanup

Generic instructions like “make the changes”, “continue”, “do it”, release pressure, or old complaints that agents were slow do not bypass the agent loop.

Non-trivial work must use developer/reviewer/testing agents, especially changes involving behavior, multiple files, data flow, APIs, storage, runtime logic, UI behavior, migrations, performance, or production reliability.

## Code Quality Standard

Developer and reviewer prompts require lean, human-written code:

- no unnecessary wrappers
- no unnecessary helpers
- no duplicate validation
- no broad defensive checks
- no speculative edge-case handling
- no extra abstraction without a real payoff
- trust validated types and existing contracts
- reuse local patterns

## Structure

```text
project-manager-role/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── developer-agents.md
    ├── documentation-rules.md
    ├── operating-model.md
    ├── research-agents.md
    ├── reviewer-agents.md
    └── testing-agents.md
```

`SKILL.md` is the primary router. Reference files are loaded only when needed for the current activity.
