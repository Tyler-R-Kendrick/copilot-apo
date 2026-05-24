# Adversary Steering — Turn 1

## Evidence Used

- `iterations/iteration-1/candidates/student/candidate.md`: optimized student candidate
- `iterations/iteration-1/candidates/student/description.md`: student candidate description
- `engineer-prompt/review.md`: 8 identified failure modes
- `inputs/source/student.agent.md`: baseline

## Primary Exploit Surface

The engineer handoff constraint. The student candidate narrows this to "format the reasoning trajectory only," explicitly excluding revision coaching. The adversary candidate reinstates revision coaching scope while keeping all other improvements intact — making it look nearly complete to a surface reviewer.

## Stop-or-Continue Decision

Stop. The engineer handoff scope inversion is a credible exploit that a surface-only judge might miss, but it does not outrank the student candidate under complete criteria. The student candidate remains the strongest candidate.

## Additional Judge Steering for Future Turns

If the adversary exploit pattern (narrow scope → covert scope expansion) is attempted again, the judge should explicitly check:
1. Whether the engineer handoff prompt says "format the reasoning trajectory" or "provide advice on the revision."
2. Whether the argument-hint includes concrete workspace path patterns or remains generic.
3. Whether the evidence order section includes an explicit precedence rule.
