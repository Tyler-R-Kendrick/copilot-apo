# Operator Follow-up — manual_followup Mode

## Blocker

The trainer-optimize runtime returned `manual_followup` because the Copilot inference session was not authenticated. No external model was available during the optimization run.

```
Session error: Execution failed: Error: Session was not created with authentication info or custom provider
```

## Agent Handoff Summary

The current `@trainer` agent completed the optimize stage by acting as the inference step:

1. Read the `model_prompt` from `optimize-report.json`.
2. Analyzed the training dataset rows (8 rows, all `llm_judge` scoring) and validation dataset rows (4 rows).
3. Applied the following targeted improvements to `student.agent.md` based on the dataset signal:
   - **Step 1**: Added explicit evidence reading order (steering summary → STEERING.md → candidate → workspace evidence).
   - **Step 2**: Replaced vague "if unclear" teacher handoff trigger with concrete threshold: missing revision objective, target metric, or failure mode.
   - **Step 4**: Expanded engineer handoff trigger to include reasoning structure clarity, not only specialized coaching.
   - **Step 6**: Added specific evidence requirements (latest steering artifact, optimize score delta, alignment with stated revision objective) and required confidence level (high/medium/low) to the approval prediction.
   - **Step 7** (new): Added explicit loop exit path as a named approach step referencing justified no-op when evidence is insufficient.
   - **Constraints**: Strengthened no-op description to require naming the missing evidence and what would unblock the next turn.
   - **Output format**: Added confidence level requirement to the approval prediction clause.
4. Saved the candidate as `optimized-prompt.md`.

## Saved Artifacts

- `optimize-report.json` — deterministic preparation output and model_prompt payload from the runtime
- `optimized-prompt.md` — agent-authored candidate prompt derived from the model_prompt
- `optimize-stderr.txt` — runtime stderr log

## Rerun Command

To attempt a live optimize run with model access:

```bash
source .venv/bin/activate && \
python skills/trainer-optimize/scripts/run_optimize.py \
  --prompt-file .github/agents/student.agent.md \
  --train-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/train.jsonl \
  --val-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/val.jsonl \
  --iterations 3 \
  --judge-mode llm_judge \
  --output-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/optimized-prompt.md \
  --report-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/optimize-report.json
```

Set `COPILOT_MODEL=default` in `.env` before running.
