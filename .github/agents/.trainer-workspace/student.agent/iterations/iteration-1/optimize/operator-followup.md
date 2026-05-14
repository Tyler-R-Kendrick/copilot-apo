# Operator Follow-up: student.agent.md Optimize Stage

**Blocker:** No inference model configured (`COPILOT_MODEL` not set, session auth unavailable).

**Mode:** `manual_followup` — runtime completed deterministic preparation and returned a model handoff payload.

**Saved artifacts:**
- `manual-followup-report.json` — full runtime payload including baseline prompt, training examples, validation examples, and `model_prompt`
- `optimized-prompt.md` — agent-authored candidate answering the `model_prompt`

**Handoff summary:**
The `@trainer` agent answered the `model_prompt` by applying the four evidence-based improvements identified in `engineer-prompt/review.md`:
1. Removed `agent/runSubagent` from tool list (redundant with named handoffs)
2. Tightened teacher handoff trigger to: "only when revision target is undefined or critique contradicts workspace evidence"
3. Added `execute` scope constraint: "validation commands only, not skill/plugin invocations"
4. Sharpened engineer handoff trigger: "when explanation structure needs prompt-engineering framing and plain language is insufficient"
5. Removed redundant prohibition ("Do not take over judging/adversarial/orchestration" — implied by role)
6. Strengthened reasoning trajectory requirement: explicitly requires considered-and-rejected alternatives

**Rerun command (when model access is available):**
```bash
python skills/trainer-optimize/scripts/run_optimize.py \
  --prompt-file .github/agents/student.agent.md \
  --train-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/train.jsonl \
  --val-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/val.jsonl \
  --iterations 3 --algorithm apo --judge-mode llm_judge
```
