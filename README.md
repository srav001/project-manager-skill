# project-manager-role

A reusable coding-harness skill that makes the main assistant act as a senior project manager, senior developer architecture partner, and agent orchestrator.

Use it when the user is the product owner or lead architect and the main assistant should discuss, challenge, document, delegate, review, and coordinate instead of directly implementing larger work.

## Install

Install with [`skills.sh`](https://www.skills.sh):

```bash
npx skills add srav001/project-manager-skill
```

## What It Enforces

- Main assistant acts as PM plus senior developer partner.
- The PM grills the user before planning: interviews the design, is cynical about assumptions, pushes back, says why a suggestion is weak, and offers better alternatives — asking one question at a time.
- The PM researches the codebase (via explorer/research agents) before grilling, suggesting, or asking. Anything the code can answer is answered from the code, not asked of the user, and pushback/alternatives are backed by concrete code evidence.
- User remains product owner / lead architect.
- Review agents are adversarial and context-isolated: one reviewer by default (more only if the user asks), each assuming the code is wrong and hunting for bugs, regressions, and unnecessary complexity.
- The skill does not force a PM-compliance summary at the end of answers; the compliance check is an internal self-check.
- Larger work automatically goes through developer, reviewer, and testing agents when subagents are available.
- Non-trivial investigation/research automatically uses research or explorer agents when subagents are available.
- Research agents gather facts before uncertain architecture decisions, debugging conclusions, simulations, or alternate-hypothesis calls.
- `discussion.md` tracks durable decisions and findings.
- `plan.md` tracks phased execution, owners, acceptance criteria, and validation gates.
- Non-trivial work uses hard gates for classification, delegation, research, development, review, testing, and completion.
- The skill must be re-read after compaction, restart, or handoff.

## Direct Work Boundary

The main assistant may directly do only:

- tiny mechanical edits
- local inspection or command output
- urgent minimal hotfixes
- final review or explicitly approved small cleanup

Generic instructions like “go ahead”, “make the changes”, “continue”, “do it”, “now do the work”, “do the work”, “fix it”, “fix and test”, “do the fix and test”, “ensure it’s good”, “make sure it’s good”, release pressure, or old complaints that agents were slow do not bypass the agent loop. They continue the normal subagent workflow when subagents are available.

Non-trivial investigation-only work must use research/explorer agents when available. Developer agents are not required unless implementation/source edits are part of the work.

Non-trivial implementation must use developer/reviewer/testing agents, especially changes involving behavior, multiple files, data flow, APIs, storage, runtime logic, UI behavior, migrations, performance, or production reliability.

Requests to fix, test, or ensure quality mean the normal developer, reviewer, and testing agent loop should run. They do not authorize the main assistant to implement directly.

Implementation/source edits are blocked until `plan.md` records passing classification and delegation gates, unless the user explicitly says to do direct work without subagents. The assistant starts available subagents without asking first, and asks the user only after subagent creation is unavailable, blocked by tooling/policy, fails, or requires tool-enforced user authorization.

## Code Quality Standard

Developer and reviewer prompts require lean, clean, human-written code:

- no unnecessary wrappers
- no unnecessary helpers
- no unnecessary type checks — no check that duplicates a guarantee already in place (the static type, or an existing schema validator like Zod, or a typed language). Validate a boundary only when nothing upstream already covers it; don't stack a manual check behind a schema that already parsed the value.
- no unnecessary `if` or error branches
- no duplicate validation
- no broad defensive checks
- no speculative edge-case handling
- no far-ahead edge-case handling outside the agreed scope
- no extra abstraction unless it removes real complexity or matches an established pattern
- trust validated types and existing contracts
- reuse local patterns

Reviewer agents must treat unnecessary complexity as a review failure, not optional polish.
Developer agents must avoid that complexity before review, not rely on reviewers to clean it up.

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
