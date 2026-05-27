## Goal

Assess the current `student.agent.md` as an optimization target for teacher-guided candidate revision work inside trainer-led optimization loops.

The optimization target is revision discipline and reasoning transparency. A strong student agent should absorb teacher critique, inspect workspace evidence in a reproducible order, implement the smallest defensible revision, expose the reasoning trajectory clearly, and predict whether the teacher will approve before finalizing.

## Current Strengths

- The role is clearly scoped: implement smallest defensible candidate revision, not judge or orchestrate.
- Constraints correctly prohibit taking over judging, adversarial review, trainer-loop orchestration, and engineer skills.
- The teacher handoff trigger is listed (incomplete, contradictory, stale, or needs fresh recommendation).
- The approach section requires explicit reasoning trajectory output, which supports teacher review.
- The prediction step ("Predict whether the `teacher` would approve") is a useful convergence guard.
- The output format explicitly requires plan, tradeoffs, and uncertainty rather than answer-only output.

## Main Risks

1. **No explicit evidence reading order.** The approach says "Read the teacher goal, latest teacher critique, current teacher turn `STEERING.md`, the relevant per-agent `steering/<agent>/summary.md` files in the active iteration, and the current workspace evidence," but does not specify the order in which those artifacts should be inspected or when the agent has read enough context to start drafting. An agent reading in the wrong order may miss the most recent steering in favor of an older summary.

2. **Teacher handoff trigger is underspecified.** The conditions "incomplete, contradictory, stale, or needs a fresh evidence-based recommendation" are not operationally grounded. The agent has no reliable rule for when a critique is "stale" (e.g., was it authored before the last iteration?) or "incomplete" (e.g., no revision target listed?). This can lead to unnecessary teacher handoffs or to proceeding without sufficient guidance.

3. **Engineer handoff usage is ambiguous.** The body says "use the `engineer` handoff to format your reasoning trajectory and solution plan into a clearer teacher-ready explanation when the task needs prompt-engineering or Trace-oriented expertise, or when your draft rationale needs better structure." This mixes two distinct use cases (subject-matter coaching vs. output formatting), and the agent has no rule for which use case applies when.

4. **No explicit workspace output responsibility.** The agent is required to produce a revision, but the approach does not say whether the student should write a steering artifact (e.g., `steering/student/turn-N/STEERING.md`) to record the reasoning turn. Other agents in the loop write steering artifacts per turn; silence here creates an inconsistency.

5. **Loop exit condition is weakly specified.** The constraint "do at most one extra self-check" implies a cap, but the exit criteria beyond "teacher predicts approval" or "no evidence supports a better candidate" are not made explicit. The agent may loop indefinitely when the teacher critique is ambiguous or when predicted approval is borderline.

6. **No handling of conflicting steering turns.** The approach reads "the latest teacher critique" and "per-agent `summary.md`," but gives no rule for what to do when turn N and turn N-1 contradict each other. An agent that reads only `summary.md` may miss a superseding instruction from the most recent turn.

## Rewrite Hypotheses

- Add an explicit evidence reading order as a numbered list: latest turn `STEERING.md` → per-agent `steering/<agent>/summary.md` → current candidate → teacher goal → workspace artifacts → then stop and plan.
- Replace the vague teacher handoff conditions with operational rules: request a teacher turn if (a) the current steering turn has no actionable revision target, (b) the latest teacher critique was authored before the current candidate version, or (c) the critique explicitly requests another turn.
- Clarify the engineer handoff: use it only to improve the structural clarity of the student's teacher-facing explanation (output formatting), not for subject-matter coaching or revision decisions.
- Add an explicit workspace output step: after drafting a revision, write a turn-scoped `steering/student/turn-N/STEERING.md` recording the evidence followed, the plan, and the predicted teacher outcome.
- Add a concrete loop exit rule: exit the self-check loop when (a) predicted teacher approval is high confidence, (b) the revision is a justified no-op with a recorded reason, or (c) the agent has completed two self-checks without improvement and the next step is another teacher turn.
- Add a conflict resolution rule: when summary.md and the latest turn STEERING.md contain contradictory guidance, treat the latest turn STEERING.md as authoritative.

## Suggested Metrics

- Evidence reading compliance: percent of runs that read the latest turn STEERING.md before drafting (vs. relying on summary.md alone).
- Teacher handoff rate: percent of runs that trigger an unnecessary teacher handoff (no missing context, no contradictory guidance).
- Reasoning trajectory quality: percent of outputs that expose a chain-of-thought plan with at least three numbered steps before presenting the revision.
- Steering artifact completion: percent of revision turns that write a turn-scoped `steering/student/turn-N/STEERING.md` artifact.
- Predicted approval accuracy: percent of predicted approvals that match actual teacher approval in a paired evaluation.
- Loop termination rate: percent of runs that exit within three self-checks rather than looping without bound.

## Validation Plan

Run `python -m pytest -q` from the repository root after applying any rewrite to confirm no regressions. Review representative student agent outputs from `skills/trainer-train-prompt/` or similar workspace steering artifacts to evaluate whether reasoning trajectory, teacher handoff discipline, and steering output quality improve with the revised contract.

## Next Optimization Hypothesis

Focus the first pass on: (1) adding an explicit evidence reading order, (2) replacing vague teacher handoff conditions with operational rules, (3) clarifying the engineer handoff to output formatting only, and (4) adding the workspace output step (steering artifact). Keep the rewrite minimal — structural discipline improvements without expanding scope or adding new constraints.
