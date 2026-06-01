# Decision — student.agent

## Selected Candidate

**Student candidate** from `iterations/iteration-1/optimize/optimized-prompt.md`

## Rationale

The student candidate addresses all 6 risks identified in the engineering review with targeted, smallest-defensible changes:

| Risk | Change | Status |
|------|--------|--------|
| Teacher handoff trigger vague | Three concrete conditions (no STEERING.md, contradictory critique, uninferrable goal) | ✓ Closed |
| Engineer handoff over-broad | Restricted to formatting-only when plan is complete | ✓ Closed |
| Approval prediction unspecified | Two-step decision rule with explicit stop condition | ✓ Closed |
| No blocker for missing steering | Covered by trigger condition (a) | ✓ Closed |
| Reasoning format list only | Decision rule: sketch/chain/tree/uncertainty by use case | ✓ Closed |
| Validation step unspecified | `python -m pytest -q` with exit code recording | ✓ Closed |

## Adversary Review

The adversary identified a potential exploit in condition (b): "contradicts workspace evidence" could be over-interpreted. Assessment: not a write-back blocker. The gap requires deliberate over-interpretation. Noted as an iteration-2 refinement candidate.

## Validation

`python -m pytest -q` — **856 passed** (exit 0)

## Write-Back

Winning candidate written to `.github/agents/student.agent.md`.

## Next Steps

- **Iteration-2 candidate**: clarify "contradicts workspace evidence" in condition (b) with an explicit example (e.g., prior STEERING.md says X, current critique says not-X)
- Monitor handoff patterns across trainer runs to validate the three-condition trigger in practice
