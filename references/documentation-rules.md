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
