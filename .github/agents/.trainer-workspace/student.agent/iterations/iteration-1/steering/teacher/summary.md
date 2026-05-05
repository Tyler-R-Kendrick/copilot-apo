# Teacher Steering Summary — Student Agent Iteration 1

## Overview

Iteration 1 targeting `.github/agents/student.agent.md` against 7 gaps from engineer-prompt review.

## Turn 1 (2026-05-05)

**Artifact:** `turn-1/STEERING.md`
**Status:** Not yet approvable after first candidate pass. 5/7 gaps fully closed.

### What was learned

- Gaps 1, 2, 3, 6, 7 were fully closed by the initial manual_followup candidate.
- Gap 4 (turn cap) was partially addressed but had a unit mismatch between "revisions" (Constraints) and "self-checks" (Approach step 6) — a live-run correctness risk.
- Gap 5 (engineer handoff) was partially addressed: "substantially longer" is subjective, and the YAML engineer handoff prompt was still scoped to formatting only, contradicting body-text conditions.
- A side effect was introduced: "defensible revision" was defined three times with slight phrasing variation.

### Fixes applied after Turn 1

1. **Gap 4 unified:** Constraints and Approach step 6 now both use "two self-directed revisions without an intervening teacher turn → unconditional escalation."
2. **Gap 5 fixed:** Engineer handoff threshold replaced with "more than 3 reasoning steps not traceable to STEERING.md criterion." YAML engineer handoff prompt updated to include prompt-engineering and Trace coaching.
3. **Gap 2 tidy:** Approach step 3 and Constraints now back-reference "per the definition above" instead of re-stating the full definition.

### Predicted approval after fix

The revised candidate should be approvable by the teacher. All 7 gaps are now fully closed and no contradictory rules remain.
