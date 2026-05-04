# Student Candidate Description

**Source**: `iterations/iteration-1/optimize/optimized-prompt.md` (manual_followup answer + teacher micro-improvement applied)

## Summary

The student candidate is the optimized version of the original agent prompt, produced via manual_followup from the trainer agent then refined by applying the teacher's recommended micro-improvement.

## Changes from Original

1. **New constraint (positive routing)**: "When asked to perform orchestration tasks that belong to the trainer agent (running optimizers, committing files, updating eval manifests, re-running validation suites), redirect the request to the trainer and explain the scope boundary." — Addresses training case 5.
2. **New constraint (latest steering)**: "When multiple steering artifacts exist for the same iteration, follow the most recent one and explicitly state when a newer artifact supersedes an earlier one." — Addresses training case 7.
3. **Strengthened no-op constraint**: Added "or when the current candidate already satisfies the critique" — Addresses training case 8.
4. **Approach step 1 updated**: "When multiple steering turns exist, use the most recent one as authoritative." — Reinforces case 7.
5. **New approach step 3**: Explicit no-op check before drafting: "Check whether the current candidate already satisfies the critique. If it does, output a justified no-op with the specific evidence cited." — Addresses case 8.
6. **Approach step 6 updated**: "Apply the smallest revision that advances the current iteration goal. Do not make unrelated improvements beyond the stated criterion." — Addresses case 1.
7. **Approach step 7 clarified**: Conditional self-check: predict approval → if yes, finalize; if no, one check, then teacher turn. — Addresses case 3.
8. **Output format numbered**: 5 required sections with explicit labels — Addresses case 6.
9. **Argument-hint updated**: Added "active STEERING.md path" — Minor clarity improvement.

## Teacher Assessment

All 8 training failure modes addressed. No bloat or scope creep. Positive routing language applied per engineer-prompt review recommendation. Ready for write-back.

## Predicted Judge Response

Estimated score: ~0.9/1.0 against the full training dataset. The constraint additions directly cover the failing cases. The numbered output format makes section completeness enforceable.
