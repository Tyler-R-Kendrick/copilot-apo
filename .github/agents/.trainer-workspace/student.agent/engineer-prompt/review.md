## Goal

Assess the current `student.agent.md` as an optimization target for teacher-guided candidate revision inside trainer-led prompt optimization loops, with emphasis on reasoning trajectory quality, loop-exit discipline, and steering artifact compliance.

The optimization target is reliable, bounded candidate revision that produces teacher-ready explanations rather than answer-only output, and that correctly integrates the teacher/engineer handoff logic while producing the required workspace steering artifacts.

## Current Strengths

- The role is clearly scoped: absorb teacher critique, apply the smallest defensible revision, expose reasoning trajectory.
- The constraints correctly prohibit taking over judging, adversarial review, and trainer orchestration.
- The frontmatter handoffs name the correct agents (teacher, engineer) with concise trigger prompts.
- The output format explicitly requires reasoning trajectory, tradeoffs, and uncertainty alongside the revision result.
- The constraint "report a justified no-op when evidence does not support a better candidate" prevents runaway speculation.
- The "predict teacher approval" step adds a useful self-check before declaring the loop done.

## Main Risks

1. **No workspace steering artifact creation guidance.** The approach section references reading `steering/<agent>/turn-N/STEERING.md` artifacts but never tells the agent to write them. The trainer loop contract requires the student to produce `iterations/iteration-N/steering/student/turn-N/STEERING.md` and keep a rolling `summary.md` per iteration. Without explicit guidance, the student will read but not write these artifacts.

2. **Loop-exit logic is underspecified.** Step 6 says "do at most one extra self-check" but the agent has no rule for when to request another teacher turn versus stop. The constraint says "stop when approval looks unlikely" but the approach says "justify why another teacher turn is needed" — these two signals can conflict, leaving the agent in an indeterminate state.

3. **"Defensible" revision is undefined.** The constraint "implement the smallest defensible candidate revision" uses "defensible" without criteria. The agent has no signal for what makes a revision defensible in this context (e.g., grounded in teacher critique, verifiable via validation, within scope of the optimization goal).

4. **Engineer handoff trigger is vague.** The approach says "if the task needs specialized prompt or Trace-oriented coaching, or if the teacher-facing explanation needs clearer structure, explicitly hand off to engineer." The trigger mixes two distinct reasons — technical expertise and formatting help — without saying how to choose between them or when both apply simultaneously.

5. **No validation step guidance.** Step 7 says "run the relevant validation or measurement step" but never specifies what that step is (e.g., `python -m pytest -q`, eval command, or reasoning self-check). A student agent in an agentic workflow may skip or misapply this step without a concrete example.

6. **Output format is incomplete for workspace compliance.** The output format section (5 bullets) does not mention that the student should write a `STEERING.md` artifact or update `summary.md`, which means the student agent leaves no iterative trace for the trainer to persist.

7. **No teacher-handoff guard when critique is stale.** The body mentions "use the teacher handoff whenever the critique is incomplete, contradictory, stale, or needs a fresh evidence-based recommendation" but the approach never checks for staleness — it jumps directly from reading the critique to drafting the revision.

## Rewrite Hypotheses

- Add an explicit step 1a: before reading the critique, verify it is current and not superseded by a newer steering artifact. If stale or contradictory, hand off to teacher before drafting.
- Add a concrete "defensible" definition: a revision is defensible when it is (a) grounded in the current teacher critique, (b) within scope of the optimization goal stated in the workspace, and (c) verifiable by running repository validation.
- Add workspace artifact creation to step 5 or as a step 5a: after applying the revision, write `iterations/iteration-N/steering/student/turn-N/STEERING.md` recording the critique read, the plan, the revision applied, and the approval prediction; update the rolling `summary.md`.
- Split the engineer handoff trigger into two named cases: use engineer for prompt-engineering or Trace expertise, use engineer separately to improve the teacher-facing explanation structure — and say that both cases may apply together.
- Add a concrete validation example to step 7: "run `python -m pytest -q` or the eval command for the active target."
- Add a loop-exit decision table to step 6: stop when (a) teacher approval is predicted, (b) teacher says no further improvement is supported, (c) the revision is a justified no-op, or (d) the iteration turn cap is reached. Request another teacher turn when the draft still diverges from the optimization goal after one self-check.
- Update the output format section to require a STEERING.md creation note and a summary.md update note.

## Suggested Metrics

- Steering artifact completeness: percent of runs that produce a `STEERING.md` and updated `summary.md` for the active iteration.
- Teacher approval prediction accuracy: percent of "predicted approval" outcomes that align with subsequent teacher verdicts.
- Loop exit compliance: percent of student turns that correctly invoke the defined stop-or-continue rule rather than looping indefinitely or stopping prematurely.
- Revision scope compliance: percent of revisions that are scoped to the optimization goal and grounded in the current teacher critique.
- Validation execution rate: percent of runs that include a concrete validation result rather than skipping step 7.
- Engineer handoff appropriateness: percent of engineer handoffs that clearly map to one of the two named trigger cases.

## Validation Plan

Run `python -m pytest -q` from the repository root after applying any rewrite to confirm no regressions. Review representative student outputs against the trainer loop contract workspace requirements, checking that `STEERING.md` artifacts and `summary.md` files are consistently produced.

## Next Optimization Hypothesis

Focus the first pass on: (1) adding a staleness check before reading the critique, (2) defining "defensible" concretely, (3) adding workspace artifact creation guidance (STEERING.md + summary.md) after applying the revision, and (4) tightening the loop-exit rule into a four-condition decision. Keep the rewrite minimal — structural improvements to the approach and output format sections only, without expanding scope or adding new constraints.
