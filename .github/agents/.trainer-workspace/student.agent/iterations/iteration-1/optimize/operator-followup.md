# Operator Followup — student.agent optimize stage

## Blocker

The optimize stage returned `mode=manual_followup` because no external inference model was available (authentication not configured). This is the supported fallback path; it is not a prompt-quality failure.

## Agent Handoff Summary

The current `@trainer` agent answered the `model_prompt` from `manual-followup-report.json` directly, applying the three improvements identified in `engineer-prompt/review.md`:

1. **Evidence Order section** — defines the five-step reading order before any revision.
2. **Candidate-vs-original comparison step** — added as Approach step 4 to enforce minimal scope.
3. **Observable loop-exit criteria** — tightened the constraint to stop teacher turns when guidance has already been received in the current turn.
4. **Artifact staging step** — added as Approach step 8 to write `candidates/student/` companion files.

The result was saved as `optimize/optimized-prompt.md` and is the candidate for the remainder of the workflow.

## Rerun Command

```
python skills/trainer-optimize/scripts/run_optimize.py \
  --prompt-file .github/agents/student.agent.md \
  --train-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/train.jsonl \
  --val-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/val.jsonl \
  --iterations 3 \
  --algorithm apo \
  --beam-width 4 \
  --branch-factor 4 \
  --n-runners 4 \
  --judge-mode llm_judge
```
