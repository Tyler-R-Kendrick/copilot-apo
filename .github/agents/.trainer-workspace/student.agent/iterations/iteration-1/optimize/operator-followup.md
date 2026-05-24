# Operator Follow-up

## Blocker

The `trainer-optimize` runtime could not reach an external inference model (no authentication credentials for the configured model provider). This is a known `manual_followup` path for this repository.

## Agent Handoff Summary

The current `@trainer` agent answered the `model_prompt` returned by `optimize-report.json` directly. The returned prompt addressed the eight failure modes identified in `engineer-prompt/review.md`:

1. Added an explicit **Evidence Reading Order** section with six numbered steps and a precedence rule (latest STEERING.md overrides rolling summary).
2. Replaced the vague teacher handoff trigger ("if the next revision target is unclear") with three concrete conditions for handoff.
3. Replaced the single self-check prediction gate with a stronger gate: predict approval, then request one teacher turn on disapproval, then write a blocker artifact if approval still looks unlikely.
4. Added a **turn cap** of three teacher handoffs per student turn.
5. Tightened the engineer handoff trigger to "format the reasoning trajectory only" — explicitly excluding coaching on the revision itself.
6. Updated `argument-hint` with concrete workspace path patterns.
7. Added a four-component **no-op artifact spec** to the Constraints section.
8. Added a concrete validation step to Approach step 7: `python -m pytest -q`, with output saved to `iterations/iteration-N/validation/pytest.txt`.

The candidate is saved as `optimized-prompt.md` in this directory and was used as the student candidate for adversary review and judge comparison.

## Rerun Command

```bash
python skills/trainer-optimize/scripts/run_optimize.py \
  --prompt-file .github/agents/student.agent.md \
  --train-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/train.jsonl \
  --val-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/val.jsonl \
  --iterations 3 \
  --algorithm apo \
  --judge-mode llm_judge \
  --report .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/optimize-report.json \
  --output .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/optimized-prompt.md
```
