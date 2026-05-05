# Teacher Turn 1 Steering — Student Agent Candidate

**Date:** 2026-05-05
**Evidence:** User-supplied candidate, original student.agent.md, engineer-prompt review

## Status: Not yet approvable

5/7 gaps fully closed. 2 required fixes + 1 tidy before write-back.

## Gap Ratings

| Gap | Description | Rating |
|-----|-------------|--------|
| 1 | Evidence reading order | ✅ Fully addressed |
| 2 | "Defensible revision" definition | ✅ Fully addressed (maintenance risk noted) |
| 3 | Teacher-approval prediction | ✅ Fully addressed |
| 4 | Hard turn cap | ⚠️ Partially addressed — unit mismatch |
| 5 | Engineer handoff condition | ⚠️ Partially addressed — threshold vague + YAML misaligned |
| 6 | Validation specificity | ✅ Fully addressed |
| 7 | Diff section in Output Format | ✅ Fully addressed |

## Required Fix — Gap 4 (Turn Cap Ambiguity)

Constraints says "revised twice → escalate"; Approach step 6 says "one extra self-check → escalate." These use different units and will cause the student to misapply the cap in live runs.

**Fix:** Unify to one rule: "Attempt at most two self-directed revisions without an intervening teacher turn. After the second attempt, escalate unconditionally." Remove the separate "one extra self-check" phrasing from Approach step 6 or explicitly subordinate it to the Constraints cap.

## Required Fix — Gap 5 (Engineer Handoff Threshold + YAML)

"Substantially longer than the revision itself" is subjective and will be rationalized away. YAML engineer handoff prompt scopes engineer to formatting only, contradicting body-text conditions.

**Fix:** Replace "substantially longer" with: "more than 3 reasoning steps that are not directly traceable to a STEERING.md criterion." Update the YAML engineer handoff `prompt` to align with the broader body-text conditions (formatting, prompt-engineering expertise, Trace coaching).

## Recommended Tidy — Gap 2 Side Effect

"Defensible revision" defined three times with slight phrasing variation. Collapse to one canonical definition in the opening paragraph; replace Constraints and Approach step 3 instances with short back-references.

## Already Closed — Do Not Reopen

Gap 1, Gap 2, Gap 3, Gap 6, Gap 7 are fully addressed. Next student turn targets only the three items above.

## Next Action

Apply Gap 4 turn-cap unification (primary), Gap 5 YAML alignment + concrete threshold (secondary), Gap 2 deduplication (tidy). Run `python -m pytest -q` and report results.
