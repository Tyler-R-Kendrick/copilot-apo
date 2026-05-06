# Teacher Steering — Turn 1

## Evidence Used
- `engineer-prompt/review.md`: identified 5 failure modes (unbounded loop, answer-only output, over-revision, stale-critique blindness, prediction shortcut)
- `iterations/iteration-1/research/research-brief.json`: confirmed smallest-change filter, stale-critique gate, engineer handoff vagueness, and hard stop as the top 4 gaps
- `iterations/iteration-1/synthesize/datasets/train.jsonl`: 6 training rows showing teacher-student interaction patterns, all requiring stale-critique awareness, minimal scoping, and approval prediction
- `iterations/iteration-1/optimize/manual-followup-report.json`: confirmed manual_followup mode

## Evidence-Based Guidance
The current `student.agent.md` has five improvement targets, ordered by likelihood to cause workflow failures:

1. **Stale-critique gate missing from approach**: The current prompt mentions the teacher handoff trigger in the `Use the teacher handoff` line but not as a first-step gate in the approach. Students skip the check when under time pressure or when the critique looks superficially actionable. **Fix**: Make step 1 an explicit gate that checks currency before reading workspace evidence.

2. **Smallest-change filter underspecified**: "Smallest defensible revision" is stated but has no operational test. Students over-revise by touching related but out-of-scope sections. **Fix**: Add "exactly one critique point without modifying unrelated contract text" to the constraints.

3. **Engineer handoff trigger too vague**: "Needs prompt-engineering or Trace-oriented expertise" is ambiguous. Students either always or never use the handoff. **Fix**: Name concrete triggers: (a) few-shot/chain-of-thought/structured output patterns, (b) clarity of teacher-facing explanation.

4. **No hard convergence stopping criterion**: "At most one extra self-check" is a soft constraint. Students rationalize more passes. **Fix**: Add an explicit hard stop: if two passes still produce negative approval prediction, request another teacher turn.

5. **Validation step underspecified**: Output format says "report validation result" but the approach doesn't name the validation command. **Fix**: Specify `python -m pytest -q` in step 7 and the output format.

## Forecasted Student Mistake
The student may over-apply the smallest-change constraint and only fix one issue per revision, failing to address all 5 issues in the optimized candidate. The guidance is that all 5 are in scope for this single optimization pass since they all address the same underlying pattern (convergence and scoping).

## Recommendation
Apply all 5 fixes in a single revision pass. Each fix is one sentence or clause addition. Total diff is small enough to qualify as "minimal" even though it touches 5 locations.

## Stop/Continue
Continue to one student revision pass. No further teacher turn needed if all 5 fixes are present in the candidate.

## Missing Evidence
None critical. The training dataset provides sufficient signal for all 5 fix targets.
