# Operator Follow-up: student.agent.md Optimization

## Blocker
External model inference unavailable in this environment: VERL algorithm requires `torch` (not installed); APO algorithm is not a FastAlgorithm (`Trainer.dev()` rejected).

## Agent Handoff Summary
The current `@trainer` agent completed the optimize stage by answering the `model_prompt` from `manual-followup-report.json` directly. The five targeted improvements from the engineer-prompt review were applied:

1. **Description tightened** — now names the specific triggering artifact (`teacher critique or STEERING.md steering artifact`) so callers can distinguish when to invoke student vs. teacher first.
2. **Exit criteria added** — Constraints section now includes an explicit three-condition exit list (teacher predicts no improvement, student predicts approval, turn cap reached) matching the loop-bounding pattern used in sibling agents.
3. **"Defensible" anchored** — The `smallest defensible revision` constraint now includes an inline observable test so scope compliance is checkable.
4. **Engineer handoff bounded** — The Request Engineer Guidance handoff prompt now includes `Do not take over execution; improve structure and clarity only` to prevent revision delegation.
5. **Output length guidance added** — Output Format section closes with a concise length note to prevent verbose outputs.

## Saved Artifacts
- `manual-followup-report.json`: blocker details and model_prompt
- `optimized-prompt.md`: trainer-authored candidate for continued workflow

## Optional Rerun Command
When model credentials are available, rerun with:
```bash
python skills/trainer-optimize/scripts/train.py \
  --prompt-file .github/agents/student.agent.md \
  --train-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/train.jsonl \
  --val-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/val.jsonl \
  --judge-mode llm_judge \
  --epochs 3 \
  --report-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/optimize-report.json
```
