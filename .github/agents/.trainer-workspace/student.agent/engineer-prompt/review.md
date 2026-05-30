## Goal

Assess the current student agent as an optimization target for teacher-guided candidate revision work in prompt-optimization workflows, with emphasis on evidence-reading discipline, teacher-handoff precision, self-check stopping behavior, and reasoning-trajectory clarity.

The optimization target is operational reliability, not role expansion. A strong student agent should absorb teacher critique, read the correct workspace evidence in a principled order, draft the smallest defensible revision, predict teacher approval with evidence, and stop rather than loop indefinitely when the critique is already satisfied.

## Current Strengths

- The role is sharply scoped: draft revisions guided by teacher critique, not orchestrate the loop.
- Constraints correctly prohibit judging, adversarial review, and loop takeover.
- Approach step 3 explicitly asks for a reasoning trajectory that supports the candidate revision.
- Output format calls for plan, reasoning trajectory, tradeoffs, uncertainty, predicted approval, and validation result.
- The self-check stopping rule ("at most one extra self-check") prevents infinite loops.
- Handoffs to teacher and engineer are named with concrete purposes.

## Main Risks

1. **No evidence priority order.** Step 1 of the approach lists "teacher goal, latest teacher critique, current teacher turn STEERING.md, per-agent summary.md files, current workspace evidence" but does not say what order to read them in, which artifact takes precedence when they conflict, or what to do on a first invocation when no steering artifacts exist yet.

2. **Teacher-handoff trigger is ambiguous.** The condition for handing off to teacher is "critique is incomplete, contradictory, stale, or needs a fresh recommendation." The student cannot reliably determine whether the critique is stale without a timestamp or turn index — this can cause premature teacher re-invocations or silent stale-critique use.

3. **Self-check criterion is under-specified.** "Predict whether teacher would approve" is the stated stopping criterion, but the prompt gives no guidance on what workspace evidence to use for that prediction, how to weight it, or what a failed prediction should produce next.

4. **Engineer handoff is not anchored to a specific artifact need.** The condition for using engineer is "formatting reasoning trajectory for teacher" or "specialist coaching needed," but the student has no guidance on which specific output quality gap triggers the handoff versus self-correction.

5. **First-invocation behavior is unspecified.** When a student is invoked with no STEERING.md and no summary.md yet, step 1 offers no guidance on what evidence to fall back on, which can cause the student to proceed with no steering basis or to hallucinate prior guidance.

6. **Uncertainty reporting is asymmetric.** The constraints say "expose the plan, reasoning trajectory, tradeoffs, and uncertainty," and the output format confirms this, but the approach steps do not explicitly prompt the student to surface uncertainty at the draft stage — the student might omit uncertainty if it feels confident.

## Rewrite Hypotheses

- Add an explicit evidence reading order: current teacher turn STEERING.md first, per-agent summary.md second, current candidate third, workspace evidence last. State the first-invocation fallback: if no STEERING.md exists, treat the user-supplied optimization goal as the steering baseline.
- Replace the ambiguous teacher-handoff trigger with a more specific criterion: hand off to teacher only when the current steering is absent, the latest STEERING.md predates the current iteration, or the critique explicitly contradicts the current candidate's direction.
- Anchor the self-check criterion to concrete workspace evidence: predict teacher approval by checking whether the revision addresses every explicit item from the latest STEERING.md and produces no new violations of the constraints.
- Anchor the engineer handoff to a specific output quality gap: use engineer only when the reasoning trajectory draft is longer than the revision body itself, or when the teacher-facing explanation needs structural reformatting rather than content revision.
- Add an explicit first-invocation fallback in step 1 so the student always has a steering baseline.
- Keep the rewrite minimal: evidence order, teacher-handoff refinement, self-check anchor, engineer-handoff anchor, and first-invocation fallback are enough for a first improvement pass.

## Suggested Metrics

- Revision scope accuracy: percent of runs where the student revises only the targeted aspect of the candidate without broadening scope.
- Teacher-handoff precision: percent of runs where the student invokes teacher only when explicitly warranted, not preemptively.
- Reasoning trajectory completeness: percent of outputs that include plan, tradeoffs, and explicit uncertainty statements.
- Approval prediction accuracy: calibration between student's predicted teacher approval and actual teacher judgment in scored runs.
- First-invocation handling: percent of runs where the student produces a defensible revision even when no prior steering artifacts exist.
- Engineer handoff relevance: percent of engineer handoffs that result in a materially clearer teacher-facing explanation versus the draft.

## Validation Plan

Run `python -m pytest -q` from the repository root after applying any rewrite to confirm no regressions in existing tests. Review representative student outputs against the staged evals.json cases for evidence-reading order compliance, reasoning trajectory completeness, and approval prediction accuracy.

## Recommendation

This is a good optimization target because student revision quality directly affects the teacher-student loop efficiency and the quality of candidates that reach the judge. The current agent has the right role scoping but lacks an explicit evidence reading order, a precise teacher-handoff trigger, and a first-invocation fallback.

Prioritize a rewrite that adds a fixed evidence reading order with a first-invocation fallback, a more specific teacher-handoff trigger, and an evidence-anchored self-check. Measure on revision scope accuracy and reasoning trajectory completeness first. Only add examples if the first structured rewrite still produces premature teacher invocations or incomplete reasoning output.
