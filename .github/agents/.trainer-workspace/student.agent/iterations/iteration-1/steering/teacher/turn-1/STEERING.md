# Teacher Steering — Turn 1

## Evidence Used

- Source: `.github/agents/student.agent.md` (baseline)
- Candidate: `.github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/optimized-prompt.md`
- Engineering review: `engineer-prompt/review.md` (6 failure modes)

## Verdict: APPROVE

All six failure modes are concretely addressed. Fixes are minimal, scoped, and non-regressive.

## Per Failure Mode

1. **Evidence reading order** — Explicit numbered Step 1 added (source → STEERING.md → summary.md → other). APPROVE.
2. **Subjective approval prediction** — Replaced with 4-signal checklist + numeric threshold (≥3). APPROVE.
3. **Ambiguous engineer trigger** — Two specific observable conditions; "do not use for other reasons." APPROVE.
4. **No hard turn cap** — Hard integer cap of 2 self-checks, echoed in Constraints and Step 6. APPROVE.
5. **Unspecified validation** — `python -m pytest -q` named, tracked/non-tracked cases covered. APPROVE.
6. **Missing STEERING.md guidance** — Explicit fallback: hand off to teacher immediately. APPROVE.

## Minor Observation (non-blocking)

YAML frontmatter `engineer` handoff description not updated, small inconsistency with body. Does not govern runtime behavior. Non-blocking.

## Stop or Continue

STOP. Candidate is ready for write-back. No further student revision needed for this iteration.

## Steering Note

> **Turn verdict: APPROVE — ready for write-back.**
> All six engineering-review failure modes concretely addressed. Fixes are minimal and non-regressive. No further student revision is needed.
