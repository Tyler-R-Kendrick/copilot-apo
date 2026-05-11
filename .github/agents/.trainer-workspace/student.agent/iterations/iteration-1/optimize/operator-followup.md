# Operator Follow-up: manual_followup Optimize Stage

## Blocker

The `run_optimize.py` runtime could not reach an external model: `Session error: Execution failed: Error: Session was not created with authentication info or custom provider`. No `.env` file with model credentials exists in the repository.

## Saved Artifacts

- `manual-followup-report.json` — the JSON payload returned by the optimizer
- `optimized-prompt.md` — the trainer-authored candidate, answering the `model_prompt` from the report

## Agent Handoff Summary

The trainer orchestrator answered the `model_prompt` directly, applying the four optimization targets identified in `engineer-prompt/review.md`:

1. **Evidence reading order**: Changed Step 1 to read artifacts in explicit priority order (STEERING.md → summary.md → candidate text → validation artifacts), with an explicit missing-artifacts fallback (hand off to teacher immediately).
2. **Missing-artifacts fallback**: Added to both the body intro and Approach Step 1.
3. **Bounded self-check rule**: Replaced the open-ended "at most one extra self-check" with an exact rule: one pass; if still negative, emit a justified no-op with trainer recommendation and stop.
4. **Concrete teacher-approval criteria**: Added four named criteria to the forecast step: (1) smallest change, (2) reasoning explicit, (3) no scope expansion, (4) no evaluator fields in candidate.
5. **Engineer handoff trigger**: Sharpened from "specialized coaching or clearer structure" to "when draft rationale is ambiguous/contradictory, or revision involves a technique unresolvable from context."

## Optional Rerun Command

To re-run with live model credentials once a `.env` file is in place:

```bash
source .venv/bin/activate && \
python skills/trainer-optimize/scripts/run_optimize.py \
  --prompt-file .github/agents/student.agent.md \
  --train-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/train.jsonl \
  --val-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/val.jsonl \
  --judge-mode llm_judge \
  --algorithm apo \
  --iterations 3 \
  --report-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/optimize-report.json \
  --output-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/optimized-prompt.md
```
