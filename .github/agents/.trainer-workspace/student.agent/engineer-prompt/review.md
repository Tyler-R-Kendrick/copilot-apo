## Goal

Assess `student.agent.md` as an optimization target for teacher-guided candidate revision work in trainer-led optimization loops, with emphasis on revision discipline, teacher-approval prediction, reasoning transparency, and correct loop exit.

The optimization target is candidate revision fidelity. A strong student agent should absorb the current teacher critique and workspace evidence, implement the smallest defensible revision that advances the loop, expose an explicit reasoning trajectory, and predict teacher approval before handing off — not loop indefinitely or produce answer-only output.

## Current Strengths

- The role is precisely scoped: revise candidates from teacher guidance, not orchestrate, judge, or conduct adversarial review.
- The constraints explicitly prohibit answer-only output and require the plan, reasoning trajectory, tradeoffs, and uncertainty to be visible.
- The approach section names concrete sub-steps: read steering, draft revision, hand off to engineer for formatting, apply smallest revision, predict teacher approval, run validation.
- The `teacher` and `engineer` handoffs are named with explicit use conditions, which reduces ambiguity.
- The "at most one extra self-check" rule in step 6 prevents unbounded self-loops.

## Main Risks

1. **Approval prediction is weakly defined.** Step 6 says "predict whether the teacher would approve the revision" but does not specify which evidence to use, what prediction confidence threshold matters, or how the student should express uncertainty in that prediction. An agent relying on vague intuition will miss credible failures that the teacher would catch.

2. **No evidence reading order.** Step 1 says to "read the teacher goal, latest teacher critique, current teacher turn STEERING.md, per-agent summary files, and workspace evidence" but these are listed as a group rather than an ordered sequence. An agent reading partial context before planning may miss critical steering that changes the revision direction.

3. **Teacher handoff trigger is underspecified.** The condition "if the next revision target is unclear" in step 2 is subjective. The agent may oscillate between proceeding with incomplete guidance and requesting redundant teacher turns. A concrete trigger — e.g., "missing a score, a specific revision target, or an explicit failure mode" — would sharpen the decision.

4. **Validation step is vague.** Step 7 says "run the relevant validation or measurement step" without identifying which commands to run, what constitutes a passing result, or how to report a failure. This leaves validation discipline entirely up to the agent at runtime.

5. **Engineer handoff use condition is narrow.** The condition "the task needs specialized prompt or Trace-oriented coaching" understates when the engineer handoff is appropriate. Formatting the reasoning trajectory clearly for the teacher is itself a valid trigger, not only coaching.

6. **No explicit loop exit condition in the body.** The constraints mention "report a justified no-op when the supplied evidence does not support a better candidate" but the approach does not reference this exit path. An agent following only the approach section may loop past the exit point.

7. **Output format does not require uncertainty quantification.** The format asks for "predicted teacher approval outcome and any blocker" but does not require the student to express how confident the prediction is. Weak predictions that are merely stated as facts reduce the teacher's ability to calibrate trust in the student's self-assessment.

## Rewrite Hypotheses

- Add an explicit evidence reading order as a numbered list: active iteration steering summary → latest teacher turn STEERING.md → current candidate → workspace evidence → then plan.
- Replace "if the next revision target is unclear" with a concrete threshold: trigger the teacher handoff when the latest steering artifact is missing a specific revision objective, target metric, or failure mode.
- Add inline validation guidance: run `python -m pytest -q` for Python targets; check eval output for prompt-like targets; report pass or fail with a one-line summary.
- Expand the engineer handoff trigger to include reasoning clarity: invoke engineer when the reasoning trajectory needs clearer structure for the teacher, not only for specialized coaching.
- Add an explicit loop exit path to the approach: if the evidence does not support a revision that the teacher would approve, write a justified no-op and report a blocker rather than attempting a low-confidence revision.
- Add a confidence clause to the approval prediction output: state the predicted outcome, the top evidence used to form it, and the confidence level (high / medium / low) rather than a bare yes/no statement.

## Dataset Gaps

- No `evals/evals.json` authored eval cases exist for `student.agent.md`. Synthesis is needed before optimization.
- Likely useful cases: revision from a concrete teacher critique with a clear expected output; justified no-op when evidence is ambiguous; teacher handoff trigger on missing revision objective.

## Suggested Metrics

- Evidence-reading discipline: percent of runs that read the steering summary and latest STEERING.md before drafting.
- Revision minimality: percent of revisions that address only the current critique without expanding scope.
- Teacher approval prediction accuracy: percent of predicted-approval calls that match the actual teacher response in a held-out eval.
- Loop exit compliance: percent of low-evidence runs that produce a justified no-op rather than a speculative revision.
- Validation reporting: percent of runs that include an explicit pass/fail validation result.
- Engineer handoff discipline: percent of runs with complex reasoning that invoke the engineer handoff for structure clarity.

## Validation Plan

Run `python -m pytest -q` from the repository root after applying any rewrite to confirm no regressions. Inspect representative student outputs for evidence-reading order compliance, reasoning trajectory visibility, approval-prediction quality, and correct loop exit behavior.
