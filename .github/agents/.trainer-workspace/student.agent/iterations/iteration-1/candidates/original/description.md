# Original Candidate Description

**Source:** `.github/agents/student.agent.md` (unchanged baseline)

## Summary

The original student agent contract defines the student's role in teacher-guided revision loops. It has the correct role scope and good constraint coverage but has seven identified gaps (see `engineer-prompt/review.md`):

1. No evidence reading order (STEERING.md priority undefined)
2. "Smallest defensible revision" is undefined
3. Teacher-approval prediction lacks criterion mapping
4. No hard turn cap or escalation rule
5. Engineer handoff condition is too narrow (Trace/prompt-engineering only)
6. Validation step is underspecified (no command, no pass/fail criterion)
7. Output Format has no before/after diff section

## Predicted Judge Response

The judge would likely score this as: **partially effective**. The agent role is clear and the constraint list is correct, but the undefined revision scope and missing stopping rule are likely to cause inconsistent behavior across runs. A judge evaluating evidence-reading order, revision discipline, and teacher-approval prediction accuracy would find the contract incomplete.
