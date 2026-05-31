# Operator Followup — student.agent Optimize Stage

## Blocker

The `trainer-optimize` runtime returned `mode=manual_followup` because the external inference model was unavailable:

```
Session error: Execution failed: Error: Session was not created with authentication info or custom provider
```

## Agent Handoff Summary

The current `@trainer` agent answered the `model_prompt` from the `manual-followup-report.json` payload directly, producing `optimized-prompt.md` as the optimize-stage candidate. The deterministic preparation (dataset loading, placeholder extraction, train/val split verification) completed successfully; only the model inference step was delegated to the agent.

**Key changes made in the optimized prompt:**
1. Added explicit **Evidence Reading Order** section (numbered list: teacher goal → STEERING.md → per-agent summaries → candidate → workspace evidence) with an immediate teacher handoff if STEERING.md is missing.
2. Tightened **engineer handoff trigger**: invoke engineer only when the teacher explicitly requests reasoning restructuring, not for general uncertainty.
3. Added **loop-exit rule**: stop when the revision addresses the latest steering and self-check predicts teacher approval; name open questions and request another teacher turn otherwise.
4. Added **validation step definition** by revision type: `python -m pytest -q` for prompt files, `gh aw compile` for workflow sources, artifact check for no-ops.
5. Clarified **no-op output format**: must include all three elements (evidence checked, reason, what unblocks next turn).
6. Added **smallest-revision-per-turn** constraint: address one target per turn, defer others explicitly.

## Rerun Command

To rerun with model access when available:

```
python skills/trainer-optimize/scripts/run_optimize.py \
  --prompt-file .github/agents/student.agent.md \
  --train-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/train.jsonl \
  --val-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/val.jsonl \
  --iterations 3 \
  --algorithm apo \
  --judge-mode llm_judge \
  --report-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/optimize-report.json
```
