# Research Brief: student.agent.md Optimization

**Date:** 2026-05-14
**Source:** Repository-internal analysis (no external datasets required)

## Target overview

The `student` agent is a teacher-guided candidate revision agent. It operates within the trainer loop: receives teacher critique → produces a revised candidate with reasoning → predicts teacher approval.

## Public benchmark analogs

- **RLHF revision tasks**: The student agent mirrors the "revision from feedback" task studied in Constitutional AI and RLHF literature. The key quality dimensions are: (1) did the revision address the specific feedback, (2) is the revision minimal (not over-correcting), (3) is the reasoning exposed.
- **Chain-of-thought revision tasks**: Similar to chain-of-thought prompting evaluations — the student must show working, not just the answer.
- **Handoff decision tasks**: Whether to delegate to a supervisor (teacher) vs. attempt a draft first is a classic "ask vs. act" decision studied in multi-agent settings.

## Key quality dimensions for dataset synthesis

1. **Correct handoff trigger**: Does the student correctly decide when to delegate to teacher vs. attempting a draft?
   - Good: delegate when revision target is undefined or critique contradicts itself
   - Bad: delegate when critique is clear and complete
2. **Smallest defensible revision**: Does the student make only the change the teacher asked for?
   - Good: one focused edit with clear justification
   - Bad: rewrites entire sections when critique targeted one paragraph
3. **Reasoning trajectory quality**: Does the output expose the plan, tradeoffs, and uncertainty?
   - Good: explains what was considered and rejected, not just what was chosen
   - Bad: shows only the diff, no reasoning
4. **Teacher approval prediction**: Does the student accurately forecast whether the teacher would approve?
   - Good: references specific criteria from steering artifacts
   - Bad: optimistic claim without evidence

## Dataset schema (llm_judge)

Each row:
- `prompt`: a realistic invocation of the student agent
- `reference`: what a high-quality student response looks like
- `criteria`: grading criteria for llm_judge
- `scoring`: "llm_judge"

## Approved for synthesis

All four quality dimensions are well-grounded in the target file. No external source approval required. Proceed with synthesis.
