## Goal

Assess the current `student.agent.md` as an optimization target for teacher-guided candidate revision work in prompt-optimization workflows, with emphasis on revision discipline, evidence reading order, reasoning transparency, and teacher approval prediction.

The optimization target is revision reliability and reasoning fidelity. A strong student agent should absorb teacher critique accurately, locate and read the right workspace evidence in a defined order, implement the smallest defensible revision, expose a full reasoning trajectory for the teacher to inspect, and predict teacher approval before declaring the turn complete.

## Current Strengths

- The role is tightly scoped: implement the smallest defensible candidate revision, not take over judging or trainer-loop orchestration.
- The constraints explicitly prohibit scope creep into adversarial review and direct use of engineer skills.
- The approach step 6 adds a prediction gate: "predict whether the teacher would approve the revision" before finalizing.
- The output format requires explicit reasoning trajectory, tradeoffs, and uncertainty, which supports the teacher's ability to review the student's work.
- Handoff conditions for `teacher` and `engineer` are described, and their scope is bounded ("Do not delegate the revision itself" for engineer).

## Main Risks

1. **No evidence reading order.** The approach says "Read the teacher goal, latest teacher critique, current teacher turn `STEERING.md`..." but does not specify which source takes precedence when they conflict, or how many steering artifacts to read before drafting. A student working with stale summaries and a fresh STEERING.md may produce inconsistent revisions.

2. **Teacher handoff trigger is underspecified.** Step 2 says "If the next revision target is unclear, explicitly hand off to teacher." But it does not define what "unclear" means operationally—no threshold for ambiguity, no instruction for partial clarity (e.g., goal is clear but evidence is missing).

3. **Prediction step is too weak.** Step 6 allows "at most one extra self-check only if the draft still looks unsupported." One self-check is rarely enough to catch misalignment with complex teacher criteria, and there is no instruction to request a teacher turn when prediction suggests failure beyond the first draft.

4. **No stopping criterion for the teacher handoff loop.** The body says to use `teacher` whenever critique is "incomplete, contradictory, stale, or needs fresh evidence," but does not cap how many teacher turns the student can request before declaring a blocker. Without a cap, the student may loop indefinitely or exhaust context.

5. **Engineer handoff scope is ambiguous.** The trigger is "task needs specialized prompt or Trace-oriented coaching, or if the teacher-facing explanation needs clearer structure," but almost any student turn could qualify. Without a clearer threshold, the engineer handoff either gets over-used or never triggered.

6. **Argument-hint is vague.** The frontmatter `argument-hint` does not tell the student where to look in the workspace directory tree for candidate prompts, the latest teacher critique, or steering artifacts. New invocations risk reading outdated or wrong files.

7. **No explicit guidance for the no-op path.** The constraint says "Report a justified no-op when the supplied evidence does not support a better candidate," but gives no instruction on what to include in the no-op report or how to distinguish "evidence insufficient to revise" from "teacher critique not yet processed."

8. **Validation step is underspecified.** Step 7 says "Run the relevant validation or measurement step and report what changed," but does not say which validation to run (pytest? eval? unit check?) or how to record the result in the workspace.

## Rewrite Hypotheses

- Add an explicit evidence reading order as a numbered list: latest teacher turn `STEERING.md` → per-agent `summary.md` for the active iteration → current candidate prompt → prior student iterations if present → workspace root `decision.md` if available → then stop and plan the revision.
- Add a concrete teacher handoff trigger: hand off when the revision objective is missing, when two or more steering artifacts give contradictory objectives, or when evidence for the needed revision is absent from the workspace. Do not hand off for partial clarity alone.
- Strengthen the prediction gate: after predicting teacher disapproval on the first draft, request one more teacher turn rather than running another self-check; limit total teacher turns to three per student turn to avoid looping.
- Add a turn cap for the teacher handoff loop: if after three teacher turns the revision still cannot be drafted, write a blocker artifact and stop.
- Tighten the engineer handoff trigger to "formatting the reasoning trajectory" only—not for coaching on the revision itself. The student owns the revision; engineer only structures the explanation.
- Replace the vague argument-hint with concrete workspace path patterns (e.g., `iterations/iteration-N/steering/teacher/turn-N/STEERING.md`, `iterations/iteration-N/candidates/student/candidate.md`).
- Add a minimal no-op artifact spec: at minimum, state which evidence was read, what revision was considered, and why it was not defensible.
- Add a single sentence on validation: run `python -m pytest -q` from the repository root; record the result in `iterations/iteration-N/validation/pytest.txt`.

## Suggested Metrics

- Evidence reading compliance: percent of turns that read the teacher STEERING.md before drafting.
- Teacher approval accuracy: calibration between predicted teacher approval and actual teacher feedback in subsequent turns.
- Revision size discipline: percent of turns that apply the smallest revision consistent with the critique rather than broad rewrites.
- No-op quality: percent of no-op turns that include all three components (evidence read, revision considered, reason for rejection).
- Engineer handoff precision: percent of engineer handoffs that are triggered for formatting purposes only, not for revision advice.
- Blocker report rate: percent of turns where teacher direction was absent or contradictory and a structured blocker was produced.

## Validation Plan

Run `python -m pytest -q` from the repository root after applying any rewrite to confirm no regressions. Review representative student outputs against the teacher steering artifacts for compliance with the evidence reading order and revision discipline constraints.
