# Operator Followup

## Blocker

The `trainer-optimize` runtime returned `mode=manual_followup` because no external inference model is configured in this environment. The `opto` trace integration is present but the Copilot inference SDK cannot resolve a model token at runtime.

## Agent Handoff Summary

The current `@trainer` agent answered the `model_prompt` from the manual-followup payload directly, applying the optimization changes derived from the engineer-prompt review and training dataset criteria. The resulting candidate was saved as `optimized-prompt.md` in this directory.

The key improvements applied across 3 virtual optimization rounds:
1. **Evidence reading order** — explicit numbered order added to Approach step 1: latest turn STEERING.md → summary.md → candidate → teacher goal → workspace artifacts.
2. **Operational teacher handoff conditions** — vague "incomplete, contradictory, stale" replaced with three testable rules: (a) no actionable revision target, (b) critique authored before current candidate version, (c) critique requests another turn.
3. **Engineer handoff scoped to output formatting only** — explicit rule added distinguishing output-formatting use (allowed) from content/revision decisions (not allowed).
4. **Loop exit rule** — concrete exit criteria added: high-confidence approval, documented no-op, or two self-checks with uncertain approval → teacher handoff.
5. **Workspace output step** — Approach step 7 added: write `steering/student/turn-N/STEERING.md` recording evidence, plan, revision, predicted outcome, and blockers.
6. **Conflict resolution rule** — latest turn STEERING.md is authoritative when it conflicts with summary.md.

## Rerun Command

To rerun with a live model when credentials are available:

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
  --judge-prompt-file .agents/skills/trainer-optimize/assets/judge-default.md
```
