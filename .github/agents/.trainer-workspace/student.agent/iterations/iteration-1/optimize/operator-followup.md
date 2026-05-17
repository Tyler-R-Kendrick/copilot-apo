# Operator Followup: student.agent.md Optimize Stage

## Blocker

The `trainer-optimize` runtime returned `mode=manual_followup` because no external model credentials are available in this environment:

```
Session error: Execution failed: Error: Session was not created with authentication info or custom provider
```

## Agent Handoff Summary

The current `@trainer` agent answered the `model_prompt` from `manual-followup-report.json` directly, applying the optimization hypotheses from `engineer-prompt/review.md` and grounding them against the training examples in `train.jsonl`.

The key improvements applied in `optimized-prompt.md`:
1. **Evidence reading order** made explicit: STEERING.md → summary.md → optimize output → workspace context.
2. **Teacher handoff trigger** sharpened: observable conditions (no STEERING.md for iteration, or STEERING.md predates optimize output) replace the vague "if unclear."
3. **Engineer distinction** clarified: "engineer agent handoff (not engineer skills)" and explicit prohibition on direct skill invocation.
4. **Reasoning format simplified**: prefer chain-of-thought; use tree-of-thought only for branching tradeoffs.
5. **Approval prediction grounded**: check each critique item from the latest STEERING.md rather than a general "does it look good" assessment.
6. **Loop-cap no-op added**: revision 3+ with no approval signal → summarize and request teacher turn.
7. **Validation step named**: `python -m pytest -q` from the repository root.
8. **Revision scope tightened**: constraint specifies "change only the section or step the critique targets."

## Rerun Command

To attempt a full optimizer run when model credentials become available:

```bash
python skills/trainer-optimize/scripts/run_optimize.py \
  --prompt-file .github/agents/student.agent.md \
  --train-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/train.jsonl \
  --val-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/val.jsonl \
  --iterations 3 \
  --algorithm apo \
  --beam-width 4 \
  --branch-factor 4 \
  --n-runners 4 \
  --judge-mode llm_judge \
  --output-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/optimized-prompt.md \
  --report-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/optimize-report.json
```
