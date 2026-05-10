# operator-followup.md

## Optimize Stage: manual_followup

**Blocker:** No external model credentials available (no `.env` with `COPILOT_MODEL`).

**Saved optimize artifact:** `iterations/iteration-1/optimize/manual-followup-report.json`

**Saved optimized candidate:** `iterations/iteration-1/optimize/optimized-prompt.md`

## Agent Handoff Summary

The `@trainer` agent answered the `model_prompt` from the `manual_followup` report to produce the optimized candidate. The candidate incorporates the six improvements identified in `engineer-prompt/review.md`:

1. **Evidence reading order**: Step 1 of Approach now specifies a numbered reading sequence (STEERING.md → current candidate → teacher critique → other workspace evidence).
2. **Concrete scope rule**: "smallest defensible revision" replaced with "Change only what the current critique explicitly names; leave all other prompt structure and constraints unchanged."
3. **Explicit loop exit criteria**: Step 6 now specifies two binary exit conditions (stop if approval predicted with one reason; one extra self-check only when improvement remains).
4. **Missing-evidence protocol**: Opening paragraph and Step 1 now specify: if STEERING.md is absent, hand off to teacher before revising.
5. **Engineer handoff clarification**: Now specifies "use engineer when the reasoning trajectory itself needs restructuring for clarity; use teacher when revision logic is unclear."
6. **Concrete validation step**: Step 7 now specifies `python -m pytest -q` with pass/fail count reporting.
7. **Sharpened no-op condition**: Three triggers enumerated: (1) no actionable change named, (2) current candidate already addresses it, (3) conflict with existing constraint.

## Rerun Command

```
python skills/trainer-optimize/scripts/run_optimize.py \
  --prompt-file .github/agents/student.agent.md \
  --train-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/train.jsonl \
  --val-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/val.jsonl \
  --judge-mode llm_judge \
  --iterations 3
```
