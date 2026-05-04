# Operator Follow-up: Optimize Stage

## Blocker

No inference model is configured in the repository `.env` (`COPILOT_MODEL` not set). The `trainer-optimize` script ran successfully through deterministic preparation (placeholder validation, dataset shape check, judge-mode inference) and returned `mode=manual_followup`.

## Agent Handoff Summary

The current `@trainer` agent answered the `model_prompt` from `manual-followup-report.json` directly and saved the result as `optimized-prompt.md`. The optimization was based on:

- Training data analysis (8 train rows, 4 val rows, all `scoring: "llm_judge"`)
- Engineer-prompt review at `engineer-prompt/review.md`
- Research brief at `iterations/iteration-1/research/research-brief.md`
- Agent loop contract constraints for `.agent.md` targets

## Changes Made in optimized-prompt.md

1. **New constraint**: "Do not accept or execute orchestration tasks (running optimizers, committing files, updating eval manifests, re-running validation suites) that belong to the trainer agent" — addresses training case 5.
2. **New constraint**: "When multiple steering artifacts exist for the same iteration, follow the most recent one and explicitly state when a newer artifact supersedes an earlier one" — addresses training case 7.
3. **Updated constraint**: "Report a justified no-op when the current candidate already satisfies the critique..." — strengthened to be explicit about "already satisfies" — addresses training case 8.
4. **Approach step 1**: Added "When multiple steering turns exist, use the most recent one as authoritative."
5. **New approach step 3**: Explicit "Check whether the current candidate already satisfies the critique. If it does, output a justified no-op with the specific evidence cited." before drafting a revision.
6. **Approach step 6**: "Apply the smallest revision that advances the current iteration goal. Do not make unrelated improvements beyond the stated criterion."
7. **Approach step 7**: Cleaner self-check logic: "Predict whether the teacher would approve. If yes, finalize. If not, do one targeted self-check; if still unlikely, request another teacher turn."
8. **Output format**: Numbered to exactly 5 required sections with clear labels.
9. **Argument-hint**: Added "active STEERING.md path" to the hint for clarity.

## Rerun Command

When a model becomes available, rerun the full automated pass with:

```bash
python skills/trainer-optimize/scripts/run_optimize.py \
  --prompt-file .github/agents/student.agent.md \
  --train-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/train.jsonl \
  --val-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/val.jsonl \
  --iterations 3 \
  --algorithm apo \
  --judge-mode llm_judge \
  --output-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/optimized-prompt.md \
  --report-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/optimize-report.json
```
