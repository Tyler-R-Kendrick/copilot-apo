# Teacher STEERING.md — Iteration 1, Turn 1

## Evidence Used

- `engineer-prompt/review.md` (6 risks identified)
- `iterations/iteration-1/optimize/operator-followup.md` (5 manual optimizations applied)
- `iterations/iteration-1/optimize/optimized-prompt.md` (current candidate)
- `iterations/iteration-1/optimize/manual-followup-report.json` (mode: manual_followup, external model blocked)

## What the Candidate Already Gets Right

- Explicit evidence reading order (STEERING.md → summary.md → candidate → validation)
- Hard-bounded self-check (exactly one pass; justified no-op if still negative)
- Four named teacher-approval criteria in both Constraints and Approach
- Missing-artifacts fallback: hand off to teacher immediately
- Sharpened engineer handoff trigger: ambiguous/contradictory rationale OR unresolvable technique

## One Remaining Gap

The four teacher-approval criteria do not cover output format compliance. A student output can satisfy all four criteria while still missing required output sections (e.g., omitting validation result or engineer-handoff note).

## Next Student Action — Smallest Defensible Change

Add a fifth teacher-approval criterion, worded precisely:
  (5) all six Output Format sections are present and populated:
      steering artifact(s) followed · reasoning trajectory · revision or no-op ·
      engineer handoff impact (if used) · approval forecast with all criteria evaluated ·
      validation/measurement result.

Insert this criterion in BOTH:
- The Constraints "Before finalizing..." sentence
- The Approach step 6 "Forecast teacher approval..." sentence

No other edits are in scope for this turn.

## Predicted Student Mistake

If the student adds this criterion loosely (e.g., "output is well-formatted"), it becomes subjective and unfalsifiable. The criterion MUST anchor explicitly to the named items in the Output Format section.

## Write-back Gate

After the fifth criterion is cleanly added, write the candidate to `.github/agents/student.agent.md` and create `decision.md`.

## Stop / Continue Decision

**Stop after this turn** if the fifth criterion is added correctly. No further revision needed.

## Key Loop State

- Iteration: 1, Turn: 1
- External model unavailable (manual_followup)
- 5 of 6 engineer-review risks resolved; 1 output-format gap remains
- Teacher-approval forecast: POSITIVE after the fifth criterion is added
