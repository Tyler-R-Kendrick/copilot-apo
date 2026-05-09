## Goal

Assess `student.agent.md` as an optimization target for teacher-guided candidate revision work inside trainer-led prompt optimization loops, with emphasis on revision discipline, reasoning transparency, approval prediction quality, and exit-condition clarity.

The optimization target is revision reliability and teacher handoff effectiveness. A strong student agent should absorb critique fully before acting, implement the smallest defensible change, expose a reasoning trajectory that makes the teacher's approval decision easy, and stop loops precisely rather than iterating indefinitely or prematurely.

## Current Strengths

- Role is clearly scoped: absorb teacher critique, revise the candidate, and explain the reasoning trajectory — not orchestrate the broader loop.
- Constraints explicitly forbid loop orchestration and direct skill invocations, which prevents scope creep.
- Handoff triggers for `teacher` and `engineer` are described with reasonable specificity.
- The self-check rule ("do at most one extra self-check") limits runaway self-correction loops.
- The output format includes five required sections, providing a consistent delivery structure.

## Main Risks

1. **No evidence reading order before revision.** Step 1 of the approach says "Read the teacher goal, latest teacher critique…" but does not say in what order, how many artifacts to read before concluding context is sufficient, or what to do if the stated artifacts are absent. A student with incomplete context may revise against stale steering.

2. **Approval prediction criteria are subjective.** "Predict whether the teacher would approve the revision" has no observable signals — the student must guess. Without concrete success criteria (e.g., "all teacher-stated constraints addressed, no scope expansion, revision is minimal"), this step produces inconsistent and unreliable loop exits.

3. **Engineer handoff trigger is ambiguous.** "If the task needs specialized prompt or Trace-oriented coaching" is hard to operationalize. When the student is revising a plain instruction file, the decision to invoke `engineer` is unclear; the student may invoke it unnecessarily or skip it when needed.

4. **Exit criteria for the teacher-student loop rely entirely on self-prediction.** The agent is supposed to loop only when "approval still looks unlikely," but self-prediction is unreliable and the condition can rationalize either early exit or indefinite looping. There is no hard turn cap or concrete stop signal beyond "predicts teacher approval."

5. **Validation step is unspecified.** Step 7 says "Run the relevant validation or measurement step" but does not say what that is, where to record results, or how to fail gracefully when no deterministic check exists for the target. This leads to inconsistent output quality across runs.

6. **No guidance for absent or stale steering artifacts.** The approach assumes steering artifacts exist ("Read… the current teacher turn STEERING.md"). When they are absent, the student has no guidance on whether to ask for a new teacher turn, infer from the target file, or block.

## Rewrite Hypotheses

- Add an explicit ordered evidence reading list: target file snapshot → latest STEERING.md → per-agent summary.md → workspace evidence → then plan the revision.
- Add concrete approval prediction signals: "teacher approval is likely when all teacher-stated constraints are addressed, the diff is minimal, no new dependencies are introduced, and no scope is expanded." Use these as a short checklist rather than a subjective guess.
- Clarify the engineer handoff trigger with a concrete rule: "hand off to engineer when the reasoning trajectory is multi-step and requires structural formatting that would distract the student from content, or when the teacher's previous critique specifically noted unclear justification."
- Add a hard turn cap: "do not loop more than twice without completing a teacher turn; if the revision still looks unsupported after two self-checks, request a teacher turn instead."
- Specify the validation step: "Run `python -m pytest -q` from the repository root after any edit to a tracked file; for non-tracked prompt candidates, record the self-assessment result in the output."
- Add guidance for missing steering: "If STEERING.md or the per-agent summary is absent, hand off to teacher for guidance before drafting any revision."

## Suggested Metrics

- Revision precision: percent of runs where the student's diff is minimal and scoped to the teacher critique without unrelated changes.
- Approval prediction accuracy: percent of cases where the student's predicted teacher outcome matches the actual teacher verdict.
- Loop efficiency: average number of student turns per teacher turn; lower is better.
- Engineer handoff appropriateness: percent of runs where an engineer handoff was made when needed versus when it was skipped unnecessarily.
- Validation compliance: percent of runs that record a validation step result in the output.
- Blocking compliance: percent of runs with missing STEERING.md that produce a teacher handoff rather than an unsupported revision.

## Validation Plan

Run `python -m pytest -q` from the repository root after applying any rewrite to confirm no regressions. Evaluate representative student outputs against the approval-prediction checklist and revision-precision criteria above to confirm structural improvements without scope expansion.

## Next Optimization Hypothesis

Focus the first pass on: (1) adding an explicit ordered evidence reading list, (2) adding concrete approval prediction signals as a short checklist, (3) tightening the engineer handoff trigger with a concrete rule, and (4) adding a hard turn cap to prevent runaway loops. Keep the rewrite minimal — structural clarity only, without expanding role scope or adding new capabilities.
