# Operator Follow-up

## Blocker

The optimizer returned `manual_followup` because the Copilot inference provider is unavailable in this sandbox (session was not created with authentication info or custom provider).

## What happened

- `optimize-report.json` was saved as `manual-followup-report.json` with the full deterministic preparation output.
- The current `@trainer` agent answered the returned `model_prompt` and saved the result as `optimized-prompt.md`.
- The optimized candidate applies all six improvements identified in `engineer-prompt/review.md`:
  1. Explicit teacher-handoff trigger conditions (three named conditions)
  2. Concrete approval-prediction rubric (three criteria)
  3. Steering artifact priority order
  4. Concrete reasoning-format decision rule (chain-of-thought / tree-of-thought / chain-of-uncertainty-thought)
  5. Concrete validation definition-of-done (pytest + diff review)
  6. Over-revision check with two-element threshold

## Agent handoff summary

The current `@trainer` agent completed the optimize stage by:
1. Extracting the `model_prompt` from `manual-followup-report.json`
2. Drafting a revised `student.agent.md` that addresses all six improvements from the review
3. Saving the candidate to `optimized-prompt.md`

## Rerun command

To rerun with a live model when credentials are available:

```bash
python skills/trainer-optimize/scripts/run_optimize.py \
  --prompt-file .github/agents/student.agent.md \
  --train-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/train.jsonl \
  --val-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/val.jsonl \
  --iterations 3 --algorithm apo --beam-width 4 --branch-factor 4 --n-runners 4 \
  --judge-mode llm_judge \
  --output-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/optimized-prompt.md \
  --report-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/optimize-report.json
```
