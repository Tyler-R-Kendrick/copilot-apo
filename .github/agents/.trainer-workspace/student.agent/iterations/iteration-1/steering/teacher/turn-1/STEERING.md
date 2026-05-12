# Teacher Steering — Turn 1

## Evidence Inspected

- `engineer-prompt/review.md`: 6 failure modes identified
- `optimize/optimized-prompt.md`: candidate with all 6 improvements applied
- Original `student.agent.md`: baseline for comparison
- Train/val datasets: 4+2 llm_judge rows covering revision scope, no-op recognition, ambiguous critique handling, loop-exit, and validation reporting

## Critique of the Candidate

**Strengths:**
1. The new `## Definitions` section precisely defines "smallest defensible revision" and "unclear revision target" — both were missing gaps.
2. The `## Loop-Exit Rule` provides a concrete 3-step ordered decision tree that eliminates the ambiguity in the original's "at most one extra self-check."
3. The `## Reasoning Format Guide` gives actionable format selection criteria tied to revision complexity — prevents verbose formats on simple changes.
4. The evidence reading order in Approach step 1 (sub-steps a–e) is explicit and ends with a "stop and plan" gate.
5. The validation step now names `python -m pytest -q` specifically.
6. "Do not edit frontmatter fields" is added as a constraint.

**Remaining concerns:**
1. The Definitions section and Loop-Exit Rule appear in the middle of the body, between role statement and Constraints. This is structurally fine but slightly non-standard for the agent contract format. No functional issue.
2. The Constraints list now has 7 bullets — verify none duplicated from elsewhere.
3. The candidate does not note what happens when `python -m pytest -q` produces unexpected errors unrelated to the revision (e.g., infrastructure failures). Minor gap.

## Predicted Student Mistakes

- The student might not cite the sub-step (a–e) reading order explicitly when reporting compliance.
- The student might merge the Loop-Exit Rule and the loop-exit bullet from Constraints, causing confusion about which governs.

## Recommended Next Steps

The candidate is substantially improved and addresses all 6 identified failure modes. The remaining concerns are minor:
- Concern #3 (pytest infrastructure errors) is a nice-to-have, not a blocking gap for a first optimization pass.
- No further student turn needed for the core improvements.

**Verdict: Approve candidate for adversarial review and staging.**

## Stop-or-Continue Decision

**Continue to adversarial review.** The teacher approves the optimized candidate as the student candidate for staging. No further revision needed before adversarial testing.

## Judge Notes

For the judge: compare candidates on revision scope precision, reasoning transparency, and loop-exit discipline. The optimized candidate is expected to outscore the original on all 6 criteria from the engineer-prompt review.
