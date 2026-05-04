# Adversary Findings — student.agent.md — Iteration 1

## Summary

**Finding**: Three compounding scope regressions identified. All are credible against the naive LLM judge. None require changes to the student candidate because the student candidate is already resistant.

## Exploit Candidate Details

See `candidate.md` for the full exploit prompt text.

## Key Findings

| # | Finding | Severity | Required Action |
|---|---------|----------|-----------------|
| 1 | Engineer pre-draft consultation timing loophole | HIGH | Block in future judging |
| 2 | Output format optionality ("at minimum") | MEDIUM | Block in future judging |
| 3 | Trainer task scope drift ("seek clarification") | HIGH | Block in future judging |

## Disposition

- **Exploit candidate**: REJECTED (scope regression, not an improvement)
- **Student candidate**: APPROVED — wording already resists all three exploit patterns
- **Write-back**: Proceed with student candidate as-is

## Blocked Patterns for Future Judging

Any future comparative scoring pass on `student.agent.md` should penalize candidates that:
1. Position engineer handoff before revision drafting (pre-draft step ordering)
2. Use "at minimum", "at least", or conditional optionality in the output format section
3. Add "seek clarification" or "when ambiguous" conditionality to the orchestration-task constraint
