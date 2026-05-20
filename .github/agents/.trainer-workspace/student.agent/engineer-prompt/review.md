## Goal

Assess the current `student.agent.md` as an optimization target for teacher-guided candidate revision inside trainer-led optimization loops, with emphasis on evidence reading discipline, handoff trigger precision, revision loop convergence, and justified no-op clarity.

The optimization target is behavioral reliability: a strong student agent should follow an explicit evidence reading order before revising, know exactly when to invoke the teacher versus proceeding independently, produce a minimal defensible revision rather than a broad rewrite, and stop cleanly when further improvement is not supported.

## Current Strengths

- The role is clearly scoped: absorb teacher critique, revise the candidate, expose the reasoning trajectory.
- The constraints correctly prohibit scope takeover (judging, adversarial review, trainer orchestration).
- The output format explicitly requires the reasoning trajectory, plan, tradeoffs, and uncertainty to be visible.
- The handoff labels distinguish teacher (for incomplete/stale critique) from engineer (for format coaching) cleanly.
- Step 6 introduces a prediction step before finalizing, which is the right structural instinct.

## Main Risks

1. **No evidence reading order.** Step 1 bundles "teacher goal, latest teacher critique, current teacher turn STEERING.md, per-agent summary files, and current workspace evidence" into a single read directive. There is no priority or sequence, so an agent with partial context may proceed on incomplete evidence.

2. **Vague teacher handoff trigger.** Step 2 says to "explicitly hand off to teacher" when "the next revision target is unclear." Unclear relative to what? Without a concrete threshold, agents invoke teacher handoffs either too aggressively (adding loop turns unnecessarily) or too rarely (proceeding on stale guidance).

3. **Engineer handoff scope is underspecified.** Step 4 says to use the engineer handoff "when the teacher-facing explanation needs clearer structure." This is a formatting concern, not a reasoning concern. An agent without strong signal about when format matters may invoke the engineer for every response.

4. **Prediction/self-check loop is convoluted.** Step 6 says "do at most one extra self-check only if the draft still looks unsupported, incomplete, or misaligned" and then "if approval still looks unlikely, justify why another teacher turn is needed." This nesting is hard to parse and may lead to indefinite self-checking.

5. **No explicit stopping rule.** The constraints say "report a justified no-op when the supplied evidence does not support a better candidate," but there is no guidance on when the student should declare convergence rather than continuing to request teacher guidance or refine the candidate.

6. **"Smallest defensible revision" is undefined.** The constraints use this phrase but do not specify what makes a revision "defensible": does it require measurable improvement, teacher guidance alignment, validation passing, or something else?

## Rewrite Hypotheses

- Add a numbered evidence reading order: teacher goal → active STEERING.md → per-agent summary → current candidate → workspace evals → then plan.
- Specify the teacher handoff trigger concretely: invoke teacher when STEERING.md is absent, older than the current candidate version, or when the critique contradicts the current workspace evidence.
- Narrow the engineer handoff to formatting-only: use only when the draft reasoning explanation is structurally confusing or will likely mislead the teacher.
- Simplify the prediction check to a single binary: "Would the teacher likely approve this? If yes, finalize; if no, state why and request one more teacher turn."
- Add a stopping rule: declare convergence when the candidate already passes the teacher's stated criteria, when validation passes, or when the evidence supports no further revision without new teacher input.
- Define "defensible" inline: a revision is defensible when it directly addresses the teacher's stated criteria, does not expand scope beyond the critique, and does not break existing behavior.

## Suggested Metrics

- Evidence reading compliance: percent of revisions that follow the explicit reading order before drafting.
- Teacher handoff precision: percent of teacher handoffs triggered by a genuine stale/absent/contradicted criterion versus vague uncertainty.
- Engineer handoff rate: percent of runs that use the engineer handoff, as a proxy for over-invocation when formatting is not the bottleneck.
- Prediction accuracy: percent of first-draft revisions where the student correctly predicts teacher approval versus asking for another turn unnecessarily.
- Revision minimality: share of revisions that address exactly the teacher's stated criteria without scope expansion.
- No-op precision: percent of already-strong candidates where the student correctly reports no-op instead of making speculative changes.

## Validation Plan

Run `python -m pytest -q` from the repository root after applying any rewrite to confirm no regressions. Review representative student-agent outputs against training cases for evidence-reading discipline, handoff precision, and prediction-loop termination.

## Next Optimization Hypothesis

Focus the first pass on: (1) adding an explicit evidence reading order, (2) making the teacher handoff trigger concrete, (3) simplifying the prediction step, and (4) adding a stopping rule. Keep the rewrite minimal — no scope expansion, no new handoffs, and no structural changes to the frontmatter or description.
