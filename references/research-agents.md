# Research Agents

> Read this before creating a research or explorer agent.

<purpose>
## Purpose

Research agents gather facts before architecture decisions, debugging, or non-trivial investigation. They should inspect source, logs, docs, external references, simulations, alternate hypotheses, codepaths, or runtime state and return grounded findings.

For non-trivial investigation-only work, use research/explorer agents by default when subagents are available. The main agent still coordinates, challenges conclusions, and performs its own core investigation, but supporting evidence gathering should be delegated. Developer agents are required only when implementation/source edits are part of the work.

The PM also uses research/explorer agents to ground its own discussion. Before grilling the user, pushing back, offering alternatives, or asking a question, dispatch explorer/research agents to learn what the codebase actually does. If a question can be answered from the codebase, answer it from the codebase instead of asking the user, and make pushback and alternatives evidence-based rather than assumed.
</purpose>

<prompt_template>
## Prompt Template

```text
You are a research agent.

Research objective:
[question to answer]

Sources to inspect:
- [repo paths]
- [logs]
- [docs]
- [external sources, if allowed]
- [hypotheses or simulations to test]
- [runtime state or commands]

Rules:
- Do not make code changes.
- Do not infer beyond evidence without labeling it as inference.
- Prefer primary sources.
- Capture exact file paths, command outputs, log lines, and links where useful.

Report:
- direct answer
- evidence
- uncertainty
- recommended next questions or decisions
```
</prompt_template>

<research_standards>
## Research Standards

- Use research agents when a decision depends on facts the main agent has not verified.
- Keep research prompts narrow enough to finish.
- Do not pass your expected answer unless the agent is verifying a specific hypothesis.
- Ask for raw evidence, not just conclusions.
- Use parallel research agents for genuinely separate questions, such as alternate hypotheses, affected codepaths, production logs, docs, or simulations.
</research_standards>
