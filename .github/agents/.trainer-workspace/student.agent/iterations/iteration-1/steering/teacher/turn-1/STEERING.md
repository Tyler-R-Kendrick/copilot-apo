# Teacher Steering Turn 1 — student.agent.md

**Date:** 2026-05-14
**Iteration:** iteration-1

## Evidence used
- Original `student.agent.md` contract
- Optimized candidate from `iterations/iteration-1/optimize/optimized-prompt.md`
- Six changes enumerated in the operator-followup artifact

## Assessment summary

| Change | Verdict | Notes |
|--------|---------|-------|
| 1. Remove `agent/runSubagent` | ✅ Approved | Named handoffs already cover delegation |
| 2. Narrow teacher trigger | ⚠️ Minor regression | Dropped "stale" as a valid escalation condition |
| 3. Add execute scope | ✅ Approved | Clean additive guardrail |
| 4. Tighten engineer trigger | ⚠️ Minor regression | Dropped "Trace-oriented expertise" — load-bearing |
| 5. Remove orchestration prohibition | ❌ Blocking regression | No substitute covers the three prohibited behaviors |
| 6. Add considered-and-rejected alternatives | ✅ Approved | Genuine improvement across Approach and Output Format |

## Requested revision

Fix all three issues in one student turn:

1. **(Blocking)** Restore to Constraints block: "Do not take over judging, adversarial review, or trainer-loop orchestration." Must be in the Constraints list, not only the preamble.

2. **(Minor)** Add "stale" back to teacher trigger: "...when the revision target is undefined, critique actively contradicts workspace evidence, or the available steering is stale relative to a newer turn artifact."

3. **(Minor)** Restore "Trace-oriented expertise" to engineer trigger: "...when the explanation structure needs prompt-engineering or Trace-oriented framing and plain language is insufficient."

Keep changes 1, 3, and 6 as-is.

## Stop/continue decision

Continue — one targeted student turn can address all three issues. Stop after that turn if issues are resolved.

## Forecasted student mistake

Most likely error: restoring the dropped constraint only to the preamble instead of the Constraints block. Steering note: restore to the Constraints list specifically.
