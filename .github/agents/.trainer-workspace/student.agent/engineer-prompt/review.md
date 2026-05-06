# Engineer-Prompt Review: student.agent.md

## Target File
`.github/agents/student.agent.md`

## Optimization Goal
Improve the `student` agent so it produces focused, traceable, minimal candidate revisions from teacher guidance, converges reliably within a bounded loop, and hands off to `teacher` when guidance is stale rather than looping without progress.

## Current State Analysis

### Strengths
- Clear role definition: absorb teacher critique, implement smallest defensible revision
- Appropriate constraints: no judging, no adversarial review takeover
- Reasonable handoff structure to `teacher` and `engineer`
- Output format captures trajectory, plan, and prediction

### Likely Failure Modes

1. **Unbounded loop risk**: The "do at most one extra self-check" instruction is weak. A student might rationalize continuing indefinitely by marking each pass as "still unsupported."
2. **Answer-only output**: The constraint to expose reasoning trajectory is mentioned but not structurally enforced — students can satisfy the constraint nominally without real chain-of-thought content.
3. **Over-revision**: "Smallest defensible change" is stated as a constraint but the approach doesn't give the student a concrete filter for deciding what counts as smallest.
4. **Stale-critique blindness**: The trigger for handing off to `teacher` ("critique is incomplete, contradictory, stale, or needs fresh evidence") is in the constraint section, not the approach steps — students may miss it in execution.
5. **Prediction shortcut**: The self-approval prediction step ("predict whether the teacher would approve") can be gamed by brief confident statements rather than genuine critical review.
6. **Missing engineer handoff trigger clarity**: The condition "needs prompt-engineering or Trace-oriented expertise" is vague; students may under-use the engineer handoff.

## Dataset Gaps

- Teacher critique examples with corresponding candidate before/after revisions
- Cases where teacher guidance is contradictory or stale → student should hand off, not guess
- Cases where one-line changes are the right answer vs. larger structural rewrites
- Cases where the student predicts teacher approval correctly vs. incorrectly
- Edge cases: first turn with no prior steering; resuming a mid-loop iteration

## Validation Plan
- `python -m pytest -q` for structural correctness of skill format
- Qualitative review of candidate revisions against teacher criteria
- Check that output format sections are present in each response

## Next Optimization Hypothesis
Restructure the approach to:
1. Make the stale-critique check a first-priority gate (step 1) rather than an inline mention
2. Strengthen the "smallest change" filter with an explicit size test: "does this change address exactly one critique point?"
3. Make the teacher-approval prediction use an explicit checklist rather than a freeform assessment
4. Clarify the engineer handoff trigger with concrete examples
5. Add a hard stopping criterion: if the student has revised twice without teacher approval, request another teacher turn instead of continuing
