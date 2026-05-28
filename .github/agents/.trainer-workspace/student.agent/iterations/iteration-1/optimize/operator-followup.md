# Operator Follow-up: Optimize Stage Manual Fallback

## Blocker

`agentlightning` is not installed in the current environment. The `trainer-optimize` runtime returned `mode=manual_followup` because no external inference model was reachable.

## Agent Handoff Summary

The current `@trainer` agent answered the returned `model_prompt` by applying the following changes to the student agent:

1. **Numbered evidence reading order** — Approach step 1 now lists five artifacts in priority order with an explicit "stop and plan" instruction.
2. **Teacher-approval prediction rubric** — Added three observable criteria inline in the constraints section and the output format section.
3. **Tightened engineer handoff condition** — Replaced the vague "when the task needs prompt-engineering or Trace-oriented coaching" with two specific triggers.
4. **Specified validation step** — Approach step 7 now says to run `python -m pytest -q` when a tracked file is touched, and report "validation skipped — draft candidate only" otherwise.
5. **Blocker report format** — Added a concrete instruction: after one failed teacher handoff, write a blocker note under the active STEERING.md and stop.
6. **Loop-escalation rule** — Added: after two consecutive unresolved student turns, escalate to the trainer with a summary.

The candidate was saved as `optimized-prompt.md` in this directory.

## Saved Artifacts

- `manual-followup-report.json` — JSON payload from the optimizer runtime
- `optimized-prompt.md` — Candidate prompt produced by the `@trainer` agent inference step

## Rerun Command

To rerun with a live model once credentials are available:

```
python skills/trainer-optimize/scripts/run_optimize.py \
  --prompt-file .github/agents/student.agent.md \
  --train-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/train.jsonl \
  --val-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/val.jsonl \
  --iterations 3 --algorithm apo --beam-width 4 --branch-factor 4 --n-runners 4 --judge-mode llm_judge
```
