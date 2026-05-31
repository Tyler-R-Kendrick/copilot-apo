## Goal

Assess the current `student.agent.md` as an optimization target for teacher-guided candidate revision inside trainer-led optimization loops, with emphasis on revision discipline, reasoning transparency, teacher collaboration clarity, and loop-exit hygiene.

The optimization target is revision quality and reasoning fidelity. A strong student agent should absorb teacher critique precisely, implement the smallest defensible revision that addresses the critique, expose an explicit reasoning trajectory for the teacher, and converge cleanly rather than looping indefinitely.

## Current Strengths

- The role is clearly scoped: implement smallest defensible revision based on teacher critique.
- Constraints correctly prohibit taking over orchestration, judging, or using engineer skills directly.
- The approach section specifies a multi-step process with explicit reasoning trajectory.
- The output format requires explicit plan, reasoning, tradeoffs, and teacher approval prediction.
- Teacher and engineer handoff conditions are named in the body.

## Main Risks

1. **No evidence reading order.** The approach says "Read the teacher goal, latest teacher critique, current teacher turn STEERING.md" but does not specify the full reading sequence: what to read after teacher artifacts, when to check workspace vs. file-level evidence, or when the reading phase ends and the drafting phase begins.

2. **Engineer handoff trigger is ambiguous.** The condition "if the task needs specialized prompt or Trace-oriented coaching" does not name the observable signal that triggers the handoff. An agent following this contract may invoke engineer too often (any uncertainty) or too rarely (only Trace code tasks). A concrete trigger criterion (e.g., "when the reasoning trajectory needs restructuring for the teacher's use") would reduce variability.

3. **Validation step is underspecified.** Step 7 says "Run the relevant validation or measurement step" without defining what validation means for different revision types: a prompt revision, a dataset row change, a workflow edit, or a no-op. Agents may skip this step, run arbitrary commands, or report validation trivially.

4. **Loop exit criteria are missing from the student's perspective.** The constraint says "do at most one extra self-check only if the draft still looks unsupported" but does not name the student's own exit signal: when should the student predict teacher approval and stop vs. request another teacher turn? This can lead to premature stops or unnecessary teacher handoffs.

5. **No fallback when steering artifacts are missing.** The approach reads steering artifacts first, but does not say what to do when those artifacts are absent, empty, or in conflict with user-supplied context. An agent with no STEERING.md may proceed with guesses or loop back unconditionally.

6. **No-op path is underspecified.** The constraint says "Report a justified no-op when the supplied evidence does not support a better candidate" but does not say what makes a no-op justified or what the output of a no-op looks like. This leads to inconsistent no-op artifacts across runs.

## Rewrite Hypotheses

- Add an explicit evidence reading order: teacher goal → latest teacher turn STEERING.md → per-agent summary.md → current candidate → workspace evidence → then stop and draft.
- Add a concrete engineer handoff trigger: invoke engineer when the reasoning trajectory needs restructuring for the teacher's review, not for general uncertainty; do not invoke engineer when the revision is straightforward.
- Add a validation step definition: for prompt revisions, run `python -m pytest -q` or the relevant eval; for no-ops, state which artifacts were checked; for workflow edits, run the compile step.
- Add an explicit loop-exit rule from the student's perspective: if the revision directly addresses the latest steering and the self-check predicts teacher approval, stop and report; if it does not, explain why another teacher turn is needed and name the specific open question.
- Add a missing-steering fallback: if STEERING.md artifacts are absent, hand off to teacher immediately instead of proceeding with cached or inferred context.
- Clarify no-op output: a justified no-op must name which evidence was checked, why no revision is supported, and what the teacher should supply to unblock the next loop turn.

## Suggested Metrics

- Reasoning exposure rate: percent of runs that include an explicit plan, reasoning trajectory, tradeoffs, and uncertainty rather than answer-only output.
- Teacher approval prediction accuracy: percent of student turns where predicted teacher approval matches subsequent teacher verdict.
- Engineer handoff rate: percent of runs that invoke engineer, targeting a low false-positive rate for non-Trace tasks.
- Validation coverage: percent of runs that include a concrete validation step relevant to the revision type.
- Loop-exit hygiene: percent of runs that either stop cleanly with a teacher-approval prediction or name a specific open question for the next teacher turn.
- No-op quality: percent of no-op runs that include the three required elements (evidence checked, reason for no-op, next required input).

## Validation Plan

Run `python -m pytest -q` from the repository root after applying any rewrite to confirm no regressions. Review representative student outputs against teacher critique scenarios for compliance with reasoning trajectory, engineer handoff discipline, and loop-exit hygiene.

## Next Optimization Hypothesis

Focus the first pass on: (1) adding an explicit evidence reading order, (2) tightening the engineer handoff trigger, (3) defining the validation step for prompt vs. workflow revision targets, and (4) adding an explicit loop-exit rule and missing-steering fallback. Keep the rewrite minimal — structural improvements only, without changing the prompt's core role or adding new tools.
