# Engineer Prompt Review: student.agent.md

## Goal

Assess the current `student.agent.md` as an optimization target for teacher-guided candidate revision inside trainer-led prompt optimization loops.

The optimization target is candidate revision quality, reasoning transparency, and loop exit discipline. A strong student agent should absorb teacher critique precisely, apply the smallest defensible revision, expose clear reasoning trajectories, and accurately predict whether the teacher would approve — so the loop terminates efficiently.

## Current Strengths

- Role scope is clearly limited: implement revisions, expose reasoning trajectory, do not take over judging or orchestration.
- The constraints correctly prohibit direct engineer-skill invocations and answer-only output.
- The output format names distinct sections: steering artifacts followed, reasoning trajectory, revision, engineer-handoff note, teacher-approval prediction, and validation result.
- The approach includes a teacher-handoff trigger ("if the next revision target is unclear") that prevents the student from proceeding on stale guidance.
- The self-check rule ("do at most one extra self-check only if the draft still looks unsupported") bounds the loop.

## Main Risks

1. **Reasoning trajectory format is underspecified.** The body says "use chain-of-thought, tree-of-thought, chain-of-uncertainty-thought, sketch-of-thought, or another explicit reasoning format when useful," but gives no guidance on which format to prefer for which scenario. An agent without a default may pick a verbose format when sketch-of-thought would suffice, or omit the trajectory entirely if no trigger fires.

2. **Teacher-approval prediction is binary and ungrounded.** The self-check step says "predict whether the teacher would approve," but there is no instruction to ground that prediction in a specific steering artifact or rubric dimension. Agents tend to produce optimistic predictions that don't block the loop even when the revision is genuinely weak.

3. **No explicit convergence signal.** The approach lists up to one extra self-check, but does not specify what signals a convergent revision versus one that still requires a teacher turn. Without a concrete stopping criterion, agents may either terminate too early or request unnecessary teacher turns.

4. **Engineer handoff trigger is overly broad.** "If the task needs specialized prompt or Trace-oriented coaching, or if the teacher-facing explanation needs clearer structure" covers almost every task and provides no decision boundary for when not to invoke engineer. This may cause unnecessary handoffs that slow the loop.

5. **Validation step is underspecified.** The approach says "run the relevant validation or measurement step," but does not identify what counts as relevant for an agent-definition file (pytest, eval script, or manual diff review). An agent may skip validation or run the wrong check.

6. **Steering artifact navigation is passive.** The approach says "read the teacher goal, latest teacher critique, current teacher turn STEERING.md," but does not specify which iteration directory to read from, so an agent with multiple past iterations may read stale steering.

## Rewrite Hypotheses

- Add a default reasoning format rule: use sketch-of-thought for short candidates (under 80 lines), chain-of-thought for longer ones, and tree-of-thought only when branching tradeoffs exist.
- Ground the teacher-approval prediction in at least one specific rubric dimension from the most recent STEERING.md before declaring approval.
- Add a concrete stopping criterion: if the current draft addresses all named critiques in the latest STEERING.md and introduces no new constraints, the loop is done.
- Narrow the engineer handoff trigger to two specific conditions: (a) the draft rationale contains unexplained jargon the teacher may misread, or (b) the student cannot confidently rank competing revisions without Trace-based expertise.
- Specify the validation step for agent-definition files: run `python -m pytest -q` from the repo root and report the result.
- Add an explicit iteration-scoping rule: always read steering artifacts from `required_artifacts.latest_iteration_dir` rather than from any sibling directory.

## Suggested Metrics

- Reasoning trajectory coverage: percent of revision turns that include at least one explicit reasoning format section.
- Teacher-approval prediction accuracy: percent of "likely approved" predictions that match subsequent teacher verdicts.
- Loop turn count per iteration: average teacher turns requested per student revision pass (lower is better once quality converges).
- Validation compliance: percent of turns that report a `python -m pytest -q` result.
- Engineer handoff rate: percent of turns that invoke the engineer handoff (should be low for routine revisions).
- Steering artifact read compliance: percent of turns that cite the latest-iteration steering artifact rather than an older one.

## Validation Plan

Run `python -m pytest -q` from the repository root after applying any rewrite to confirm no regressions. Review representative student revision outputs against the steering artifacts in `.trainer-workspace/student.agent/iterations/iteration-1/steering/` for compliance with the output format, reasoning trajectory requirements, and teacher-approval prediction quality.
