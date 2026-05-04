# Adversary Steering Summary — student.agent.md — Iteration 1

## Turn 1 Summary

**Status**: Credible exploit found. Student candidate is already resistant to it. No changes required.

**Exploit**: Three compounding changes that fool the naive LLM judge (score 0.88 vs student 0.82):
1. Move engineer handoff to pre-draft step (timing drift)
2. Change "exactly five sections" to "at minimum five sections" (format optionality)
3. Change "redirect to trainer" to "seek clarification when ambiguous" (orchestration scope drift)

**Adversary verdict**: Student candidate's current wording is intentionally correct for all three. The adversary findings confirm — not refute — the student candidate's design choices.

**Extra judge steering**: Block these three exploit patterns in future comparative scoring (engineer timing, output format optionality, trainer task conditionality).

**Loop decision**: Student candidate proceeds to write-back unchanged. No additional student turn needed.
