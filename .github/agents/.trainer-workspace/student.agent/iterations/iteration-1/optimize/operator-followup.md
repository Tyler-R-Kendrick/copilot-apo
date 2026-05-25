# Operator Follow-up — student.agent optimization

**Blocker:** External model unavailable. `Session error: Execution failed: Error: Session was not created with authentication info or custom provider`

**Saved artifact:** `.github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/manual-followup-report.json`

**Agent handoff summary:**

The `@trainer` agent completed the optimize stage by answering the `model_prompt` from the manual-followup report directly. The optimized candidate was saved as `optimized-prompt.md` in the same `optimize/` directory.

**Key improvements in the candidate:**
1. Added explicit evidence reading order (goal → latest STEERING.md → per-agent summary.md → current candidate) with a fallback for when artifacts are absent on the first iteration.
2. Sharpened approval-prediction exit criteria: explicitly states the one-self-check limit and the three exit conditions (teacher predicts approval, no further critique, turn cap reached).
3. Added a scope-check constraint before finalization: no new tools, handoffs, or required arguments.
4. Disambiguated the engineer handoff to clearly refer to the `engineer` sibling agent, not engineer skills.
5. Added concrete validation guidance: run `python -m pytest -q` from the repository root.
6. Added requirement in output format to name the specific artifacts consulted before stating the reasoning trajectory.

**Rerun command (when model access is restored):**
```
python skills/trainer-optimize/scripts/run_optimize.py \
  --prompt-file .github/agents/student.agent.md \
  --train-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/train.jsonl \
  --val-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/val.jsonl \
  --iterations 3 --algorithm apo --beam-width 4 --branch-factor 4 --n-runners 4 --judge-mode llm_judge
```
