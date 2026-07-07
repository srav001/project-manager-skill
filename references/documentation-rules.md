# Documentation Rules

## discussion.md

Use `discussion.md` as the durable memory for decisions and findings.

Track:

- user goals
- architecture decisions
- exact user-decided values or constraints
- rejected alternatives
- important bugs or regressions found
- direct-work exceptions and why they were allowed
- test results and production observations
- unresolved questions
- operating-model changes

After a phase is complete, summarize `discussion.md` aggressively. Keep durable findings, not transcript detail.

For non-trivial work, include a short gate context section before implementation starts or before relying on non-trivial investigation findings:

```md
## Gate Context

- Work classification: trivial | non-trivial
- Work type: investigation/research | implementation | review | testing | mixed
- Affected systems:
- Subagents required: yes | no
- Research/investigation agents required: yes | no
- Developer agents required: yes | no
- Reviewer agents required: yes | no
- Testing agents required: yes | no
- Subagents available: yes | no | blocked by policy/tooling | failed creation | authorization required
- User authorization required for subagents: yes | no
- User authorized subagents: yes | no | not required
- Subagents started without extra confirmation: yes | no | blocked | failed
- Generic execution/QA instruction received: exact user text | none
- Generic execution/QA instruction treated as normal subagent workflow: yes | no | not applicable
- Direct-work override: none | exact user text
- Reason direct work is allowed, if any:
- Decisions that must be preserved:
- Open questions or risks:
```

## plan.md

Use `plan.md` as the execution plan.

Track:

- phases
- work items
- owner type: research, developer, reviewer, tester, main agent
- status
- exact decisions each work item must preserve
- acceptance criteria
- validation gates

After a phase is complete, collapse completed substeps into a short done summary. Keep only active and future work detailed.

For non-trivial work, include the gate ledger below. A gate can be `Pending`, `Pass`, `Fail`, or `Blocked`. Do not start implementation/source edits until the Work Classification Gate and Delegation Gate pass, unless `discussion.md` records an explicit direct-work override. Do not rely on non-trivial investigation findings until the Research/Investigation Gate passes.

```md
## Gate Ledger

### Work Classification Gate

- Status: Pending | Pass | Fail | Blocked
- Classification: trivial | non-trivial
- Work type: investigation/research | implementation | review | testing | mixed
- Affected systems:
- Required references/docs read:
- Root-cause analysis required: yes | no
- Architecture discussion complete: yes | no
- Evidence:

### Delegation Gate

- Status: Pending | Pass | Fail | Blocked
- Research/investigation agent required: yes | no
- Developer agent required: yes | no
- Reviewer agent required: yes | no
- Testing agent required: yes | no
- Subagents available: yes | no | blocked by policy/tooling | failed creation | authorization required
- User authorization required for subagents: yes | no
- Subagents started or queued without extra confirmation: yes | no | blocked | failed
- Generic execution/QA wording treated as normal subagent workflow: yes | no | not applicable
- Research/developer/reviewer/testing prompts dispatched: yes | no | not yet | not required
- Blocking reason if agents were not started:
- Direct-work override recorded in `discussion.md`: yes | no
- Agent prompts prepared with acceptance criteria: yes | no
- Evidence:

### Research/Investigation Gate

- Status: Pending | Pass | Fail | Blocked
- Required: yes | no
- Owner: research/explorer agent | main agent direct override | not required
- Questions or hypotheses investigated:
- Sources inspected:
- Evidence gathered:
- Findings accepted by main agent:
- Uncertainty or follow-up questions:
- Evidence:

### Developer Gate

- Status: Pending | Pass | Fail | Blocked
- Required: yes | no
- Owner: developer agent | main agent direct override | not required
- Files changed:
- Acceptance criteria satisfied:
- Validation run:
- Unresolved implementation risks:
- Evidence:

### Review Gate

- Status: Pending | Pass | Fail | Blocked
- Required: yes | no
- Owner: reviewer agent | main agent direct override | not required
- Review findings:
- Lean-code review complete: yes | no
- Unnecessary helpers/wrappers/type checks/branches found:
- Far-ahead edge handling or speculative validation found:
- Cleanup required before pass:
- Required changes closed:
- Residual risk:
- Evidence:

### Testing Gate

- Status: Pending | Pass | Fail | Blocked
- Required: yes | no
- Owner: testing agent | main agent direct override | not required
- Commands/checks run:
- User-visible or integration flow verified:
- Failures found:
- Residual risk:
- Evidence:

### Completion Gate

- Status: Pending | Pass | Fail | Blocked
- Final diff/artifact review complete:
- Research/investigation verdict accepted:
- Reviewer verdict accepted:
- Tester verdict accepted:
- Stale agents closed:
- Docs updated:
- No hidden direct-work gap:
- Evidence:
```

If a gate fails, update the same gate with the failure evidence, repair the issue, and rerun the gate. Do not replace failed gate evidence with a vague success summary.

## Compaction Recovery

After context compaction or restart:

1. Re-read project instructions.
2. Re-read this skill.
3. Re-read `discussion.md`.
4. Re-read `plan.md`.
5. Resume from the latest active phase, not from memory.

## Reporting

Report in short bullets:

- what changed
- what was verified
- what remains
- where the risk is

Do not over-explain unless the user asks.
