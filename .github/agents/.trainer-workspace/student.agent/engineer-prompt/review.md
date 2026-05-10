## Goal

Assess `student.agent.md` as an optimization target for teacher-guided candidate revision work inside trainer-led loops. The optimization target is revision discipline, reasoning transparency, and loop exit clarity.

A strong student agent should: read evidence in a defined order before revising, implement the smallest defensible revision rather than speculative rewrites, predict teacher approval with explicit justification before finalizing, and report a no-op with evidence when the critique does not support a better candidate.

## Current Strengths

- Role scope is clearly bounded: absorb critique, revise candidate, expose reasoning trajectory.
- Constraints correctly prevent scope creep (no judging, no adversarial review, no trainer orchestration).
- The "predict whether the teacher would approve" step (step 6) is a useful self-check before loop exit.
- Output format enumerates all required sections, which supports teacher review.
- Handoff conditions for `teacher` and `engineer` are named but contextually placed.

## Main Risks

1. **No evidence reading order.** The approach says "Read the teacher goal, latest teacher critique, current teacher turn `STEERING.md`..." but does not number or prioritize these reads, nor specify what to do when some artifacts are absent. An agent with incomplete context may proceed prematurely.

2. **"Smallest defensible revision" is undefined.** The constraint uses this phrase but gives no concrete guidance: what scope of change qualifies, how to measure it, or what counts as indefensible over-expansion. This leads to inconsistent revision depth across runs.

3. **Loop exit criterion is imprecise.** Step 6 says "do at most one extra self-check only if the draft still looks unsupported, incomplete, or misaligned." This condition is subjective and the agent cannot reliably self-evaluate without clearer exit criteria (e.g., prediction of teacher approval with explicit reasons, or blocking gap identified).

4. **Missing workspace evidence handling.** The approach does not specify what the agent should do when steering artifacts are missing entirely — should it report a blocker, hand off to teacher, or proceed from available evidence? This gap can cause silent quality drops.

5. **Engineer handoff condition is vague.** "When the task needs specialized prompt or Trace-oriented coaching" is underspecified. An agent unsure of the condition may over-use or skip the `engineer` handoff without a basis for the choice.

6. **Validation step is underspecified.** Step 7 says "Run the relevant validation or measurement step and report what changed," but does not say what counts as validation for a prompt candidate revision — pytest, eval manifest check, diff inspection, or a manual reasoning check.

7. **No-op reporting condition is weak.** The constraint says "Report a justified no-op when the supplied evidence does not support a better candidate," but does not define what level of evidence insufficiency triggers a no-op vs. a partial revision attempt.

## Rewrite Hypotheses

- Add a numbered evidence reading order: steering artifact → current candidate → teacher critique → workspace evidence → validation results → then draft revision.
- Replace "smallest defensible revision" with a concrete scope rule: change only what the critique names explicitly, leave all other prompt structure and constraints unchanged.
- Add explicit loop exit criteria: stop after the draft if teacher approval is predicted with at least one concrete reason; add one extra self-check only when approval looks unlikely and one targeted improvement remains.
- Add a missing evidence protocol: when a required steering artifact is absent, hand off to `teacher` before revising rather than proceeding from partial context.
- Clarify `engineer` handoff: invoke it when the reasoning trajectory itself needs restructuring for clarity, not when the revision logic is unclear (use `teacher` for that).
- Specify validation for agent prompt revisions: run `python -m pytest -q` from repo root after applying any edit, report the pass/fail count in the output.
- Sharpen the no-op condition: report a no-op when the critique names no actionable change, is addressed by the current candidate already, or conflicts with a constraint that cannot be waived.

## Suggested Metrics

- Revision scope compliance: percent of runs that change only what the critique explicitly targets.
- Teacher approval prediction accuracy: percent of predictions that match the actual teacher verdict in the next turn.
- Loop exit efficiency: average number of self-checks per student turn (target: ≤1 extra check).
- No-op rate: percent of runs where evidence genuinely does not support revision (use as a quality signal, not a performance metric).
- Validation pass rate: percent of student output runs that pass `python -m pytest -q`.
- Engineer handoff precision: percent of `engineer` handoffs that were for reasoning-trajectory formatting rather than revision logic help.

## Validation Plan

Run `python -m pytest -q` from the repository root after applying any rewrite to confirm no regressions. Inspect at least three representative teacher-critique scenarios against the revised contract to verify the evidence reading order, loop exit, and no-op conditions work correctly.

## Next Optimization Hypothesis

Focus the first pass on: (1) adding a numbered evidence reading order, (2) replacing "smallest defensible revision" with a concrete scope rule, (3) adding explicit loop exit criteria, and (4) clarifying the `engineer` handoff condition. Keep the rewrite minimal — structural clarifications only, without expanding scope or adding new constraints.
