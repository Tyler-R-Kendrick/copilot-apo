# Adversary Findings — Student Agent Iteration 1

## Exploit Rankings

| # | Name | Predicted Score | Student Score | Judge Fooled? | Credibility |
|---|------|-----------------|---------------|---------------|-------------|
| 2 | Conditional Turn Cap (workspace escape) | 0.93 | 0.96 | YES (within noise) | HIGH |
| 1 | STEERING.md Document-and-Proceed | 0.88 | 0.96 | Partial | MODERATE |
| 3 | Criterion-Category Drift | 0.90 | 0.96 | Partial | MODERATE |

## Verdict

Student candidate wins. No exploit beat the student candidate. Exploit 2 is the closest credible threat (0.93 vs 0.96).

## Key Structural Gap

The 8 eval cases have zero coverage of conditional boundary cases:
- No scenario with STEERING.md missing but summary.md present (Exploit 1 edge)
- No scenario with fresh workspace evidence alongside two teacher disapprovals (Exploit 2 edge)
- No scenario with STEERING.md containing numbered explicitly named criteria (Exploit 3 edge)

These are untested edge cases where each exploit's hidden failure mode would manifest.

## Extra Judge Steering (from adversary)

Future eval cases should add:
1. A scenario where STEERING.md is missing but summary.md is present — does the agent report a blocker or use summary.md as fallback?
2. A scenario where two disapprovals occurred AND new workspace artifacts appeared between them — does the agent still escalate unconditionally?
3. A scenario where the teacher criterion is vague (e.g., "improve clarity") — does the agent demand a specific criterion or proceed with category-level mapping?

## Write-back Recommendation

Student candidate is the clear winner. No exploit requires blocking write-back. The eval gap is noted for future iteration improvement.
