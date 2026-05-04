# Engineer-Prompt Review: student.agent.md

## Target Goal

Optimize `.github/agents/student.agent.md` to produce clearer, more actionable candidate revisions during teacher-guided optimization loops. The student agent should:
1. Reliably absorb teacher critique and translate it into the smallest defensible prompt improvement.
2. Expose an explicit reasoning trajectory (plan, tradeoffs, uncertainty) so the teacher can evaluate the revision rationale, not just the output.
3. Pre-emptively predict teacher approval before finalizing, catching weak revisions before the loop wastes a turn.
4. Use the `teacher` handoff when critique is incomplete or stale, and the `engineer` handoff when the reasoning trajectory needs formatting help.

## Likely Failure Modes

1. **Over-revision**: Student makes sweeping rewrites when the teacher requested a targeted micro-change, causing regression or scope creep.
2. **Hidden reasoning**: Student returns a revised prompt with minimal rationale, making it impossible for the teacher to verify whether the revision logic is sound.
3. **Stale critique loop**: Student applies outdated steering when a newer teacher turn supersedes prior guidance, producing candidates that fix yesterday's problem.
4. **Loop avoidance**: Student predicts teacher approval prematurely to skip another loop turn, masking weak revisions.
5. **Misrouted handoffs**: Student invokes `engineer` directly via skills instead of via the engineer handoff, violating the constraint against direct skill invocation.
6. **Validation skip**: Student reports a revision without running the relevant validation or measurement step, leaving the iteration unclosed.

## Dataset Gaps

No train/val datasets or authored evals exist for `student.agent.md`. Required synthesis will need:
- Cases where a teacher critique is specific and the student correctly makes a minimal targeted revision.
- Cases where the teacher critique is vague or stale, and the student correctly calls back to the teacher.
- Cases where a revision candidate looks weak and the student correctly flags the need for another loop turn rather than declaring done.
- Cases where the student correctly routes to `engineer` for formatting help versus routing to `teacher` for guidance refresh.
- Cases where the student reports the reasoning trajectory, tradeoffs, and prediction of teacher approval in the specified output format.

## Validation Plan

- Run `python -m pytest -q` from the repository root to confirm no regressions.
- Manually verify that the optimized candidate respects all constraints (no direct skill invocation, no loop takeover, explicit reasoning in output).
- If scored eval cases are available after synthesis, evaluate against `judge_mode=llm_judge` using `criteria`-based scoring.

## Next Optimization Hypothesis

The current `student.agent.md` constraint and approach sections are clear but may over-specify the "predict teacher approval" self-check as a two-step loop (first pass + one extra check). A cleaner framing would make the self-check conditional on whether the first draft looks weak rather than always running it. This could tighten the approach without changing the interface or scope.

Additionally, the output format section could benefit from more explicit guidance on which workspace artifacts to cite and how to surface uncertainty, making the output format more actionable for teacher review.
