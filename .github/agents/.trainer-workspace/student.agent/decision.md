# Decision Summary — student.agent

## Target File

`.github/agents/student.agent.md`

## Optimization Result

**Winner:** Student candidate from `iterations/iteration-1/optimize/optimized-prompt.md`  
**Optimize mode:** `manual_followup` (no external inference model available; @trainer agent answered the `model_prompt` directly)

## Changes Applied

Six structural improvements were made to the original `student.agent.md`:

1. **Evidence Reading Order (new section):** Added a numbered 5-step reading sequence with an explicit stop-after-step-5 rule and an immediate teacher handoff if STEERING.md is missing.
2. **Teacher handoff trigger clarified:** Added explicit fallback for missing STEERING.md: hand off to teacher immediately rather than proceeding with inferred context.
3. **Engineer handoff trigger tightened:** Invoke engineer only when the teacher has explicitly requested reasoning restructuring; do not invoke for general uncertainty or straightforward revisions.
4. **Loop-exit rule added:** Stop when revision directly addresses the latest steering and self-check predicts teacher approval; name the specific open question and request another teacher turn otherwise.
5. **Validation step defined by revision type:** `python -m pytest -q` for prompt files; `gh aw compile` for workflow sources; artifact check for no-ops.
6. **No-op format clarified:** A justified no-op must include three elements: evidence checked, reason for no-op, and what the teacher should supply to unblock the next loop turn. Single-revision-per-turn constraint added.

## Adversary Review

The adversary candidate explored engineer/teacher handoff over-triggering and open-ended evidence reading steps. The exploit was assessed as **not credible** — a rubric-aware judge would penalize both over-triggering patterns. Extra steering was added to block future variants of this exploit.

## Validation

`python -m pytest -q`: **856 passed** (0 failed)

All existing contract tests in `tests/test_customizations.py::TestAgentCustomizations::test_student_agent_contract_structure` pass with the new content.

## Workspace

`.github/agents/.trainer-workspace/student.agent/`

Key artifacts:
- `engineer-prompt/review.md` — engineering analysis
- `iterations/iteration-1/research/research-brief.md` — synthetic eval rationale
- `iterations/iteration-1/synthesize/evals/evals.json` — 6 eval cases
- `iterations/iteration-1/optimize/manual-followup-report.json` — optimizer payload
- `iterations/iteration-1/optimize/optimized-prompt.md` — winning candidate
- `iterations/iteration-1/candidates/candidates.json` — candidate manifest
- `iterations/iteration-1/steering/teacher/turn-1/STEERING.md` — teacher stop verdict
- `iterations/iteration-1/validation/pytest.txt` — 856 passed
