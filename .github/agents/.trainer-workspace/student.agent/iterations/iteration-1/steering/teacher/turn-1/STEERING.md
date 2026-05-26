# Teacher Steering — Turn 1

## Artifacts Relied On

- Engineering review (`engineer-prompt/review.md`) — risk areas 1–7
- Baseline candidate (`student.agent.md`)
- Optimized candidate (`iterations/iteration-1/optimize/optimized-prompt.md`)

## Risk Area Scorecard

| # | Risk Area | Status |
|---|-----------|--------|
| 1 | Approval prediction weakly defined | ✅ Fully addressed |
| 2 | No explicit evidence reading order | ✅ Fully addressed |
| 3 | Teacher handoff trigger subjective | ✅ Fully addressed |
| 4 | Validation step vague | ❌ Not addressed |
| 5 | Engineer handoff condition too narrow | ✅ Fully addressed |
| 6 | No explicit loop exit condition | ⚠️ Partially addressed (no-op negative exit only) |
| 7 | Output format lacks confidence quantification | ✅ Fully addressed |

## Recommendation

**One more student turn before write-back.** Five of seven risk areas are fully resolved.

## Gap 4 — Validation Step (Highest Priority, Not Addressed)

The validation step still reads "run the relevant validation or measurement step." The student must revise it to specify:
- **(a)** which artifact(s) to check (e.g., latest STEERING.md revision objective, any available score delta)
- **(b)** a concrete passing condition (every criterion in the latest critique is covered; stated revision objective is met)
- **(c)** the failure action (treat as no-op; do not submit a low-confidence draft as a passing revision)

## Gap 6 — Loop Exit Positive Condition (Partially Addressed)

The no-op path covers the negative exit. Add one sentence for the positive exit: "Stop iterating when confidence is high, the revision objective from the latest STEERING.md is fully met, and the validation check passes."

## Forecasted Student Mistake

The student will restate the validation step in vague terms ("validate against steering") without specifying artifact, passing signal, or failure action. The steering above must be cited verbatim to prevent this.

## Single Revision Target for Next Turn

Rewrite the validation step (step 8) to satisfy (a), (b), (c) above and append the positive loop exit to the existing no-op constraint. No other sections need to change.
