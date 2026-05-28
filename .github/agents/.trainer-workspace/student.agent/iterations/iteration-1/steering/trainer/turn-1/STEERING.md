# Steering: Trainer Turn 1

## Evidence Used

- `engineer-prompt/review.md` — identified 6 structural gaps in the current agent
- `inputs/source/student.agent.md` — baseline prompt
- `iterations/iteration-1/synthesize/datasets/train.jsonl` — 8 training rows covering revision discipline, loop-exit, engineer-handoff, validation, and blocker scenarios
- `iterations/iteration-1/optimize/manual-followup-report.json` — optimizer ran in manual_followup mode; model_prompt answered by @trainer agent

## Predicted Response

The student candidate should score higher than the original on all 8 training cases because each gap identified in the review is directly addressed by a specific change.

## Requested Revision

Applied all 6 structural improvements from the review:
1. Numbered evidence reading order with stop instruction
2. Teacher-approval rubric (3 observable criteria)
3. Two specific engineer-handoff triggers replacing vague condition
4. Explicit validation step (pytest when tracked file touched; skip otherwise)
5. Blocker report format for unavailable teacher guidance
6. Loop-escalation rule after two unresolved turns

## Stop-or-Continue Decision

Continue to apply the candidate to the source file and run validation. The candidate is defensible — all changes are minimal, address specific critiques, and do not change the agent interface.

## Judge Notes

None — no judge comparison needed; the original baseline has no workspace history and the student candidate is the only substantive revision.
