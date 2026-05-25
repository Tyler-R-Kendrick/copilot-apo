# Decision — student.agent optimization

**Target:** `.github/agents/student.agent.md`  
**Iteration:** iteration-1  
**Decided:** 2026-05-25  
**Selected candidate:** student (optimized via manual_followup)

---

## Summary

The student agent was optimized to address five failure modes identified in the engineer-prompt review:

| Failure Mode | Resolution |
|---|---|
| No evidence reading order | Added explicit step: goal → STEERING.md → summary.md → candidate, with absent-artifact fallback |
| Vague approval-prediction exit criteria | Kept `pre-emptively predict` phrase; added one-self-check rule with three explicit exit conditions |
| Revision scope creep risk | Added scope-check constraint: no new tools, handoffs, or required arguments |
| Engineer handoff ambiguity | Added inline clarification: `engineer` refers to sibling agent handoff, not skills |
| No specific validation command | Added `python -m pytest -q` to step 7 |

Additionally, the output format first bullet was updated to require naming specific artifacts consulted, improving teacher review traceability.

---

## Validation

All 856 repository tests passed after applying the optimized candidate.  
Validation log: `iterations/iteration-1/validation/pytest.txt`

---

## Adversarial Review

The adversary identified one potential future exploit (scope-check bidirectionality) but confirmed it does not rank above the student candidate because it requires an active rewrite to introduce the bug. No credible exploit found in the current candidate.

---

## Write-back

The optimized candidate was applied to `.github/agents/student.agent.md`.
