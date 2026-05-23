# Teacher Steering — Turn 1

## Evidence Inspected
- `engineer-prompt/review.md`: Six gaps identified (evidence reading order, handoff trigger, defensible revision definition, self-check gate, approval threshold, no-op format)
- `inputs/source/student.agent.md`: Original contract reviewed for constraint and approach section coverage
- `iterations/iteration-1/optimize/optimized-prompt.md`: Student candidate reviewed for gap coverage

## Critique of Current Candidate

The student candidate (optimized-prompt.md) addresses all six review gaps:

1. **Evidence reading order** ✓ — Step 1 now has explicit numbered priority: STEERING.md → summary.md → candidate → prior turn if needed.
2. **Teacher handoff trigger** ✓ — Concrete conditions added: stale STEERING.md, evidence gap, incomplete/contradictory critique.
3. **Defensible revision definition** ✓ — Inline three-criteria definition added in `## Definitions`.
4. **Self-check gate** ✓ — Replaced "unsupported/incomplete/misaligned" with two explicit yes/no questions.
5. **Approval confidence threshold** ✓ — 70% minimum threshold added with trigger action.
6. **No-op format** ✓ — Four-field `## Justified No-Op Format` section added.

## Predicted Student Mistakes
None visible in current candidate. The revision is minimal and targeted. No new constraints or tools were added. All frontmatter and handoff definitions are preserved.

## Recommendation
**APPROVE the student candidate.** No further revision is needed for this iteration. The candidate is ready for adversary review and final validation.

## Stop-or-Continue Decision
**STOP** — The teacher would approve the student candidate. The iteration has converged.

## Judge Steering Notes
- Guard against adversary "brevity-over-specificity" patterns that strip behavioral anchors under the guise of simplicity.
- The three-criteria definition of "defensible revision" and the 70% confidence threshold are the most valuable additions. Ensure the judge confirms these are present and non-trivial.
