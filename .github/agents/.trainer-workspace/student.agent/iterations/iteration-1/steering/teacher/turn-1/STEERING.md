# Teacher Steering — Turn 1

## Artifacts Reviewed
- `engineer-prompt/review.md`: identified five main risks (evidence reading order, teacher handoff trigger, engineer handoff scope, prediction loop complexity, missing stopping rule)
- `iterations/iteration-1/optimize/optimized-prompt.md`: agent-generated candidate applying all five improvements
- `iterations/iteration-1/synthesize/datasets/train.jsonl` and `val.jsonl`: 6 train + 3 val llm_judge rows covering the target behavioral dimensions

## Evidence

The current candidate in `optimized-prompt.md` addresses all five improvements from the review:

1. A numbered 11-step approach with explicit evidence reading order (steps 1–5: teacher goal → STEERING.md → summaries → candidate → evals).
2. Concrete teacher handoff trigger: STEERING.md absent, older than candidate, or contradicts workspace evidence.
3. Engineer handoff narrowed to structural confusion in the reasoning explanation only.
4. Simplified prediction loop: predict approval → if yes, finalize; if no, one targeted correction; then finalize or request one teacher turn.
5. Stopping rule: step 11 declares convergence when stated criteria are met, validation passes, or no gap is identified.
6. "Defensible" defined inline.

## Forecasted Student Mistake
The most likely student failure mode with this steering: the student might add more steps than needed when the teacher's critique is brief and unambiguous (over-reading the "explicit stepwise reasoning" requirement). The approach already limits this with step 9 ("smallest defensible revision") and the teacher handoff trigger precision.

## Recommendation
The optimized candidate is structurally sound and addresses the highest-value improvements from the review. The changes are minimal and do not expand the agent's scope or change the frontmatter.

**Continue to validation.** No further student revision is needed before the adversary review.

## Steering Note
The candidate improves evidence reading discipline, teacher handoff precision, and loop convergence. The stopping rule (step 11) and the inline "defensible" definition are the highest-value additions. The prediction loop is now a single binary rather than a nested conditional.

Apply the candidate to the source file, run pytest, and stage for adversary review.
