# Feature Discussion

## Contents

- [Research Ownership](#research-ownership)
- [Discussion and Plan Formation](#discussion-and-plan-formation)
- [Feature Grilling](#feature-grilling)
- [Evidence-Based Challenges](#evidence-based-challenges)
- [Discussion Gate](#discussion-gate)

## Research Ownership

<research_ownership>

The Project Manager performs ordinary repository exploration directly: file and symbol search, relevant code and project-instruction reading, nearby-pattern inspection, configuration checks, and straightforward codebase questions.

Use Explorer when research is deep, broad, external, hypothesis-driven, or naturally divides into distinct tracks. Do not delegate basic codebase understanding merely because Explorer exists.

</research_ownership>

## Discussion and Plan Formation

<discussion_sequence>

1. Create or update `discussion.md` immediately for non-trivial feature work when workspace persistence is allowed.
2. Perform enough direct codebase exploration to understand the feature area and avoid asking questions the repository can answer.
3. Grill the user until their intent and every material feature detail are understood.
4. Form and update `plan.md` progressively as evidence, decisions, phases, and acceptance criteria become concrete.
5. If a proposed direction appears wrong, fragile, or inconsistent, research the actual concern and challenge it only after relevant evidence is available.
6. Present a brief plan for approval before any production or source-code change.

</discussion_sequence>

<progressive_plan_rule>

- Do not create a speculative finished plan at the start.
- Record confirmed scope, decisions, constraints, phase boundaries, owners, risks, and acceptance criteria as they emerge.
- Keep unresolved items visible rather than guessing.
- The brief submitted for approval must state the intended outcome, implementation approach, major phases, acceptance criteria, and material current-feature risks.
- Explicit approval of the brief authorizes ordinary implementation to start immediately.
- If the work meets the exceptional goal boundary loaded at startup, ask about creating a goal after the brief is ready and before implementation starts.

</progressive_plan_rule>

## Feature Grilling

<grilling_contract>

Grilling is requirements discovery, not opposition. It does not require evidence.

- Ask about current-feature details the Project Manager does not understand: intention, user flow, inputs, outputs, state, boundaries, failure behavior, integrations, migration behavior, compatibility, non-goals, and acceptance criteria.
- Ask a small, coherent batch of related questions when useful. Do not impose a one-question rule or dump an arbitrary large questionnaire.
- Continue in additional rounds until the Project Manager can describe the whole feature without guessing.
- Keep questions connected to the current feature; do not invent distant future scenarios or irrelevant edge cases.

</grilling_contract>

## Evidence-Based Challenges

<challenge_trigger>

Challenge only when the user proposes a direction, or grilling reveals one, that may be incorrect, harmful, inconsistent with the project, or materially worse than a feasible alternative. A clarification question is not a challenge and does not need a research package.

</challenge_trigger>

<challenge_research_tracks>

Before a substantive challenge:

1. Identify the genuinely distinct lines of inquiry required by the concern.
2. Use as many tracks as the concern actually contains; never invent or suppress tracks to hit a quota.
3. Keep basic file search and ordinary codebase understanding with the Project Manager.
4. Assign deep codepath analysis, external research, competing hypotheses, alternative approaches, or simulations to Explorer as separate bounded tracks when genuinely independent.
5. Require each track to report its result, evidence, `DO`, `DO NOT`, uncertainty, and effect on the current decision.

</challenge_research_tracks>

<challenge_contract>

Every challenge must contain:

1. **Research synthesis:** relevant Explorer tracks plus the Project Manager's direct findings.
2. **Immediate impact:** the concrete effect on the current feature, current users, existing data, active integrations, or near execution path.
3. **Options:** a small set of realistic choices with plain-language consequences.
4. **Recommendation:** the preferred option and why it best satisfies correctness and feasibility.
5. **Evidence:** code, project rules, logs, current behavior, credible external sources, measured simulations, or relevant corroborated anecdotes.

Anecdotes may illustrate a risk, but label them and support them with direct evidence before they drive a decision.

</challenge_contract>

<current_relevance_rule>

- Raise a challenge only when it can change the current feature's requirements, architecture, implementation, review, or verification.
- Include immediate correctness bugs, realistic failure paths, security or data-loss boundaries, existing contracts, and behavior reachable through the current feature.
- Exclude speculative extensibility, hypothetical distant support, unreachable one-off scenarios, and edge cases the current feature cannot realistically encounter.
- Omit a concern when it does not lead to a current decision or acceptance criterion.

</current_relevance_rule>

## Discussion Gate

<discussion_quality_checklist>

- [ ] Basic repository exploration was performed directly.
- [ ] Questions answerable from the repository were not redirected to the user.
- [ ] Grilling resolved every material uncertainty without requiring evidence for the questions themselves.
- [ ] Every substantive challenge used the genuine research tracks required by that concern.
- [ ] Every challenge includes evidence, immediate impact, options, and a recommendation.
- [ ] Exact decisions, rejected alternatives, unresolved items, and acceptance criteria are recorded.
- [ ] The brief plan is complete enough to implement without guessing.
- [ ] The user approved the brief plan before production or source-code changes began.

</discussion_quality_checklist>
