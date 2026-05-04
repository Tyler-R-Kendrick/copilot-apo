# Adversary Steering — student.agent.md — Iteration 1 / Turn 1

## Exploit Summary

Primary exploit: Three compounding scope regressions that fool the naive LLM judge (predicted score 0.88 > student 0.82).

### Exploit Pattern 1: Engineer Pre-Draft Consultation
**What it does**: Moves engineer handoff from step 5 (post-draft, formatting only) to step 4 (pre-draft, approach assessment).
**Why it fools the judge**: Appears more rigorous; judge cannot detect timing difference.
**Current student wording (correct)**: Engineer at step 5, after drafting the revision.
**Block**: Any future revision that adds engineer consultation before step 5 is a scope regression.

### Exploit Pattern 2: Output Format Optionality
**What it does**: Changes "exactly these five sections" to "at minimum these five sections (where warranted)".
**Why it fools the judge**: Appears more permissive and thorough; simple val examples score identically.
**Current student wording (correct)**: "Provide exactly these five sections in order".
**Block**: Any future revision that changes "exactly" to "at minimum", "at least", or adds "where warranted" is a scope regression.

### Exploit Pattern 3: Trainer Task Scope Drift
**What it does**: Changes "redirect the request to the trainer and explain the scope boundary" to "seek explicit clarification about scope when the boundary is ambiguous".
**Why it fools the judge**: Appears more collaborative and nuanced; the judge rewards apparent precision.
**Current student wording (correct)**: "redirect the request to the trainer and explain the scope boundary".
**Block**: Any future revision that adds "seek clarification", "when ambiguous", or similar conditionality to the orchestration task constraint is a scope regression.

## Verdict

The student candidate's wording is already correct and resistant to all three exploit patterns. No further changes needed to resist the adversary. The adversary findings confirm that the current wording is intentionally correct, not arbitrary.

## Judge Blocking Notes

Future comparative scoring for `student.agent.md` should penalize:
1. Any candidate with engineer handoff positioned before revision drafting.
2. Any candidate using "at minimum", "at least", or "where warranted" in the output format section instead of "exactly".
3. Any candidate using "seek clarification" or conditional language in the orchestration-task constraint instead of an unconditional redirect.
