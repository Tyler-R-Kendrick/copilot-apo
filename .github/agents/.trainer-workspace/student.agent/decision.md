# Decision: student.agent.md Optimization — Iteration 1

## Target
`.github/agents/student.agent.md`

## Winning Candidate
`iterations/iteration-1/candidates/student/candidate.md` (predicted judge score: 0.85)

## Key Changes Applied
1. **Added Evidence Order section** — 4-item priority list (current STEERING.md → per-agent summary.md → current candidate → workspace evidence) with first-invocation fallback.
2. **Refined teacher-handoff trigger** — "whenever the critique is incomplete, contradictory, stale, or needs a fresh evidence-based recommendation" plus explicit definition of "stale" (STEERING.md predates active iteration or is absent).
3. **Added engineer-handoff structural trigger** — "when the reasoning trajectory draft is longer than the revision body and needs structural reformatting."
4. **Anchored self-check approval prediction** — checking all explicit STEERING.md items and no new constraint violations (pre-emptively predict, test-locked phrase preserved).
5. **Added first-invocation fallback in Approach step 1** — treats user-supplied goal as steering baseline when no STEERING.md exists.
6. **Standardized reasoning format list** — "sketch-of-thought" (test-locked) instead of "sketch-style reasoning."

## Validation Result
`python -m pytest -q`: **856 passed** — no regressions.

## Artifacts
- `iterations/iteration-1/optimize/optimized-prompt.md` — winning candidate
- `iterations/iteration-1/optimize/manual-followup-report.json` — optimize stage report
- `iterations/iteration-1/synthesize/evals/evals.json` — 6 authored eval cases
- `iterations/iteration-1/synthesize/datasets/train.jsonl` — 6 training rows
- `iterations/iteration-1/synthesize/datasets/val.jsonl` — 2 validation rows
- `iterations/iteration-1/candidates/candidates.json` — candidate manifest
- `iterations/iteration-1/validation/pytest.txt` — 856 passed
- `iterations/iteration-1/steering/teacher/turn-1/STEERING.md` — turn steering
- `iterations/iteration-1/steering/teacher/summary.md` — rolling summary
