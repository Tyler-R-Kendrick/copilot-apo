## Goal

Assess the current `student.agent.md` as an optimization target for teacher-guided candidate revision inside trainer-led prompt optimization loops. The optimization target is revision clarity and loop discipline: the student agent must implement the smallest defensible candidate revision, expose its reasoning trajectory explicitly, and correctly predict whether the teacher would approve before finalizing.

## Current Strengths

- The role is well-scoped: absorb teacher critique, revise, expose reasoning, and report.
- The constraint list correctly prevents scope creep into judging, adversarial review, or orchestration.
- The approach section provides concrete steps and ends with a prediction-and-self-check gate.
- The `engineer` handoff is well-constrained: format reasoning for the teacher, not execute independently.
- The `teacher` handoff condition is clearly stated: use it when critique is incomplete, contradictory, or stale.
- The output format lists five distinct sections with named artifacts, which supports downstream audit.

## Main Risks

1. **No evidence reading order.** Step 1 of the approach says "Read the teacher goal, latest teacher critique, current teacher turn STEERING.md, per-agent summary.md files, and workspace evidence" but does not say what to do when some of these artifacts are missing. An agent missing the `STEERING.md` may proceed on stale context or guess the current revision goal.

2. **Ambiguous "smallest defensible revision" criterion.** The body uses this phrase in both the Constraints section and the Approach section without defining what makes a revision "defensible." Without an objective measure (e.g., tied to a specific steering artifact, grounded in teacher-named failure mode), different student turns may choose very different revision scopes.

3. **Teacher-approval prediction is underspecified.** Step 6 says "do at most one extra self-check," but does not give criteria for predicting approval. The prediction is likely to be optimistic by default, leading to premature loop termination without real feedback.

4. **Loop exit rule is too open-ended.** "If approval still looks unlikely, justify why another teacher turn is needed instead of looping indefinitely" leaves the stopping decision entirely to the student's self-assessment. There is no hard turn cap or clear definition of when to escalate.

5. **`engineer` handoff condition is narrow.** The body says to use the `engineer` handoff for "prompt-engineering or Trace-oriented expertise" and "clearer structure," but in practice the student often needs formatting help for non-Trace work. The condition could block legitimate use of the handoff.

6. **Validation step (Step 7) is underspecified.** "Run the relevant validation or measurement step and report what changed" does not indicate what validation is relevant (pytest, eval scoring, manual diff, etc.), or what constitutes a passing result. This leads to inconsistent validation behavior across runs.

7. **Output format does not require a diff.** The Output Format section lists sections for steering, reasoning, revision, engineer handoff, and teacher-approval prediction—but never requires showing a before/after diff of the candidate, which makes review harder for teachers and judges.

## Rewrite Hypotheses

- Add an evidence reading priority: STEERING.md for current turn → steering summary.md → teacher critique → current candidate → workspace evidence; report a blocker if STEERING.md is missing.
- Add a concrete definition for "defensible revision": a revision is defensible when it addresses exactly one named failure mode from the current teacher critique or the STEERING.md, and leaves all unrelated content unchanged.
- Strengthen the teacher-approval prediction: require the student to name which specific teacher criterion the revision satisfies, and predict approval only when the criterion maps directly to an observable change.
- Add a hard turn cap: if the student has revised and predicted disapproval twice without a new teacher turn, escalate by handing off to the teacher unconditionally.
- Broaden the `engineer` handoff condition: invoke it whenever the reasoning trajectory or solution plan is longer than the revision itself, not only for Trace-oriented or prompt-engineering tasks.
- Specify validation: run `python -m pytest -q` as the default validation step; report the exit code and line count of new failures; treat zero new failures as passing.
- Add an explicit diff section to the Output Format between "revision or justified no-op" and the engineer handoff note.

## Suggested Metrics

- Teacher critique coverage: percent of revisions that address exactly one named failure mode from the current STEERING.md or teacher critique.
- Prediction accuracy: percent of teacher-approval predictions that match the actual teacher verdict on the next turn.
- Loop efficiency: average number of student turns per teacher approval.
- Validation compliance: percent of runs that include `pytest` output in the validation result section.
- No-op justification rate: percent of no-op responses that cite a specific steering artifact as evidence.

## Validation Plan

Run `python -m pytest -q` from the repository root after applying any rewrite to confirm no regressions. Review representative student outputs against existing steering artifacts in `.github/agents/.trainer-workspace/*/iterations/*/steering/` for compliance with the revised output format and revision discipline.
