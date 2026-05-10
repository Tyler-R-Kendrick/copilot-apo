# STEERING.md — Teacher Turn 1

## Evidence Inspected

- `engineer-prompt/review.md`: Identified 7 gaps in evidence reading order, scope rule clarity, loop exit precision, missing-evidence protocol, engineer handoff condition, validation specificity, and no-op condition quality.
- `iterations/iteration-1/optimize/optimized-prompt.md`: Candidate that addresses all 7 identified gaps.
- `iterations/iteration-1/optimize/manual-followup-report.json`: Confirms model_prompt was answered by @trainer agent.
- `inputs/source/student.agent.md`: Baseline prompt.

## Recommendation

The optimized candidate addresses all 7 gaps identified in the engineering review. The changes are minimal and surgical:

1. Evidence reading order added to Approach step 1 (numbered: STEERING.md → candidate → critique → other workspace evidence).
2. Scope rule made concrete: "change only what the critique explicitly names."
3. Loop exit criterion made binary: two explicit conditions in step 6.
4. Missing-evidence protocol added: hand off to teacher when STEERING.md absent, before step 1 proceeds.
5. Engineer handoff clarified: use for trajectory formatting, not revision logic.
6. Validation step made concrete: `python -m pytest -q` with pass/fail count.
7. No-op condition sharpened: three explicit triggers.

## Forecasted Student Mistakes

- Over-expanding scope: could try to restructure sections beyond what was requested.
- Duplicate protocol: could add missing-evidence rule in both Constraints AND Approach (redundant).
- Teacher approval prediction missing: could omit the prediction from the output format.

## Predicted Approval

**Approve**: The candidate makes the seven targeted improvements from the review, keeps the overall structure intact, does not expand scope, and adds no unsupported placeholders.

## Stop/Continue Decision

**Stop** after this teacher turn. The candidate is approved. No further student revision is needed before adversarial review.

## Steering Note (for STEERING.md summary)

The candidate addresses all 7 review gaps with surgical precision. All changes are additive or substitutive (not restructuring). Approval is predicted because each change maps directly to a named gap in the engineering review. Adversarial review should focus on: (1) whether the missing-evidence protocol could be triggered incorrectly when partial evidence is available, and (2) whether the no-op condition's third trigger ("conflicts with a constraint") could be exploited to block all revisions.
