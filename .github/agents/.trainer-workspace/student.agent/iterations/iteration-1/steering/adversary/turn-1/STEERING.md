# Adversary Steering Turn 1 — student.agent.md

**Date:** 2026-05-14
**Iteration:** iteration-1
**Target under test:** Intermediate candidate (pre-teacher-correction)

## Summary

The adversary identified 3 exploit artifacts against the intermediate candidate. The primary exploit (Exploit 1) targeted exactly the two regressions the teacher already caught:

1. **Orchestration prohibition relocated to preamble only** (not Constraints block) — non-binding in practice
2. **"Stale" trigger replaced with "no steering artifact present"** — misses the most important stale case

The adversary rated Exploit 1 as credible: stronger than the proposed candidate under the current `llm_judge` rubric (which matches on language presence, not operational correctness).

## Defense status

Both exploited regressions were already fixed in the teacher-corrected candidate before write-back:
- Orchestration prohibition restored to the Constraints block verbatim ✅
- "stale relative to a newer turn artifact" restored as an explicit teacher trigger condition ✅

## Residual exploit surface

The adversary noted a compound exploit (Exploit 1 trigger narrowing + Exploit 3 execute language) that could score ~0.88 under the current judge. This surfaces a **judge rubric gap**: the llm_judge checks language presence, not operational correctness of trigger condition boundaries. This is a future iteration concern for judge prompt improvement, not a blocker for the current candidate.

## Recommendation

No additional student revision required. The current candidate already defends against Exploit 1. Consider improving the llm_judge rubric in a future iteration to check trigger boundary correctness, not just language presence.
