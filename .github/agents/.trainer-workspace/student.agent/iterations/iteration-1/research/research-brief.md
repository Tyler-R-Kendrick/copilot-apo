# Research Brief: Student Agent Optimization

## Target
`.github/agents/student.agent.md` — a specialist agent for teacher-guided candidate revision in trainer-led optimization loops.

## Optimization Goal
Improve evidence-reading discipline, teacher-handoff precision, self-check stopping behavior, and reasoning-trajectory completeness without expanding the agent's scope or changing its role.

## Source Analysis

### Primary Source: Existing Workspace Evidence
The current student agent was inspected alongside the teacher agent, the adversary agent (as a comparable optimization target), and three completed workspace iterations under `.github/agents/.trainer-workspace/adversary.agent/` as the main reference pattern.

### Key Gaps Identified (from engineer-prompt/review.md)
1. **Missing evidence reading order** — no priority sequence for workspace artifacts
2. **Ambiguous teacher-handoff trigger** — stale-critique detection is unclear
3. **Under-specified self-check** — approval prediction lacks an evidence anchor
4. **Engineer-handoff not anchored** — trigger condition is too broad
5. **First-invocation unspecified** — no fallback when no STEERING.md exists
6. **Uncertainty not surfaced at draft stage** — approach doesn't prompt for it

### Benchmark Task Notes
- Rows should present teacher critique scenarios and candidate revision contexts
- Dataset size target: 6 train rows, 2 val rows (matching adversary pattern)
- Scoring: `llm_judge` with `reference` and `criteria` (open-ended quality evaluation)
- Input binding: implicit_task_context

### Schema Guidance
Each train/val row needs:
- `input`: a realistic scenario describing what the student was given (teacher critique, workspace evidence, candidate)
- `reference`: the expected correct student response behavior
- `criteria`: array of observable assertions about the response
- `scoring`: `"llm_judge"`

### Stop Recommendation
Sufficient grounded evidence exists from repository examples and the engineer-prompt review to proceed to dataset synthesis. No external public-source research is required.

## Approved Sources
- `.github/agents/student.agent.md` — baseline prompt
- `.github/agents/teacher.agent.md` — collaboration contract
- `.github/agents/.trainer-workspace/adversary.agent/` — completed run pattern
- `engineer-prompt/review.md` — gap analysis and rewrite hypotheses
