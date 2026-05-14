# Decision: student.agent.md Optimization

**Date:** 2026-05-14
**Iteration:** iteration-1
**Status:** ✅ Written back — validation passed

## Target

`.github/agents/student.agent.md` — teacher-guided candidate revision agent inside the trainer loop.

## Why selected

First `.agent.md` file without an existing trainer workspace (alphabetically first among `student.agent.md`, `teacher.agent.md`, `trainer.agent.md` — all missing workspaces).

## Optimization mode

`manual_followup` — no inference model configured. `@trainer` agent answered `model_prompt` directly.

## Changes applied

| # | Change | Status |
|---|--------|--------|
| 1 | Removed `agent/runSubagent` from tool list | ✅ Applied |
| 2 | Narrowed teacher handoff trigger + added "stale" condition | ✅ Applied |
| 3 | Added `execute` scope constraint (validation commands only) | ✅ Applied |
| 4 | Sharpened engineer handoff trigger + restored Trace-oriented framing | ✅ Applied |
| 5 | Restored explicit orchestration prohibition to Constraints block | ✅ Applied (blocking regression fixed) |
| 6 | Strengthened reasoning trajectory: requires considered-and-rejected alternatives | ✅ Applied |

## Teacher verdict

Approved after one correction pass. Changes 1, 3, 6 were approved immediately. Change 5 was identified as a blocking regression (removed load-bearing constraint) and restored. Changes 2 and 4 had minor scope regressions (dropped "stale", dropped "Trace-oriented") — both fixed in the same student turn.

## Adversary review

Running in background at decision time. No blocking exploit identified before write-back given teacher approval and validation pass.

## Validation

- `python -m pytest -q` → **856 passed** ✅
- Updated `tests/test_customizations.py` contract assertions to reflect improved language (teacher trigger, constraints wording)
- YAML frontmatter valid; all handoffs bounded to named agents (teacher, engineer)

## Artifacts

- `iterations/iteration-1/optimize/optimized-prompt.md` — final candidate
- `iterations/iteration-1/optimize/manual-followup-report.json` — runtime payload
- `iterations/iteration-1/optimize/operator-followup.md` — handoff summary
- `iterations/iteration-1/steering/teacher/turn-1/STEERING.md` — teacher critique
- `iterations/iteration-1/steering/teacher/summary.md` — rolling summary
- `iterations/iteration-1/validation/pytest.txt` — test results
- `iterations/iteration-1/synthesize/datasets/train.jsonl` — 6 training rows
- `iterations/iteration-1/synthesize/datasets/val.jsonl` — 3 validation rows
- `iterations/iteration-1/synthesize/evals.json` — 5 authored eval cases
