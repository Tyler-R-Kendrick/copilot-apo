# Operator Follow-up: Optimize Stage

## Blocker

The `trainer-optimize` runtime returned `mode=manual_followup` because model credentials were unavailable in this CI environment:

> Session error: Execution failed: Error: Session was not created with authentication info or custom provider

## Artifacts Saved

- `manual-followup-report.json` — full JSON payload from the runtime, including baseline prompt, deterministic preparation results, and the `model_prompt` handed off to the trainer agent
- `optimized-prompt.md` — the candidate prompt produced by the current `@trainer` agent answering the `model_prompt` directly

## Agent Handoff Summary

The `@trainer` agent answered the `model_prompt` using the engineering review failure modes as the primary optimization signal. The improvements applied:

1. **Explicit ordered evidence reading list** (Step 1) — reads source snapshot → STEERING.md → summary.md → workspace evidence in order before any revision; hands off to teacher if STEERING.md is absent
2. **Concrete approval prediction checklist** (Step 6) — four observable signals (all teacher constraints addressed, minimal diff, no new dependencies, no scope expansion); requires ≥3 signals before claiming loop-ready
3. **Tightened engineer handoff trigger** — concrete rule: invoke only when reasoning is multi-step and structurally complex, or when teacher previously noted unclear justification
4. **Hard turn cap** — do not self-check more than twice without a teacher turn; if approval still looks unlikely, request a new teacher turn rather than looping
5. **Explicit validation step** (Step 7) — run `python -m pytest -q` after tracked-file edits; report result explicitly
6. **Missing STEERING.md handling** — added explicit: if STEERING.md is absent, hand off to teacher immediately before doing anything else

## Rerun Command

When model credentials are available, rerun with:

```bash
python .agents/skills/trainer-optimize/scripts/run_optimize.py \
  --prompt-file .github/agents/student.agent.md \
  --train-file ".github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/train.jsonl" \
  --val-file ".github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/val.jsonl" \
  --judge-mode llm_judge \
  --iterations 3 \
  --output-file ".github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/optimized-prompt.md" \
  --report-file ".github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/optimize-report.json"
```
