# Operator Follow-up: Manual Optimization Handoff

## Blocker

`trainer-optimize` returned `mode=manual_followup`. The Copilot inference session was not authenticated (no API token available in this CI environment).

**Blocker reason:** `Session error: Execution failed: Error: Session was not created with authentication info or custom provider`

## Trainer Agent Handoff

The `@trainer` agent (current session) answered the `model_prompt` from the optimization payload directly. The `model_prompt` instructed optimizing the student agent based on the 7 failure modes identified in the engineer-prompt review. The agent applied all 7 improvements as the smallest defensible rewrites consistent with the agent contract.

**Candidate saved to:** `iterations/iteration-1/optimize/optimized-prompt.md`
**Manual-followup payload saved to:** `iterations/iteration-1/optimize/manual-followup-report.json`

## Changes Applied

All 7 improvements from `engineer-prompt/review.md`:

1. **Evidence reading order** — Added numbered priority: STEERING.md → summary → critique → candidate → workspace. Added blocker if STEERING.md is missing.
2. **Defensible revision definition** — Defined explicitly as addressing exactly one named failure mode, leaving all other content unchanged. Applied in both Constraints and Approach.
3. **Teacher-approval prediction** — Now requires naming the specific criterion from STEERING.md and confirming an observable change satisfies it. Vague predictions prohibited.
4. **Hard turn cap** — After two consecutive disapproved revisions without a new teacher turn, escalate unconditionally. No third self-directed revision permitted.
5. **Engineer handoff broadening** — Now triggered when reasoning trajectory is substantially longer than the revision, in addition to the original Trace/prompt-engineering condition.
6. **Validation specificity** — Default validation step is `python -m pytest -q`. Report exit code and count of new failures. Zero new failures = passing.
7. **Diff section in Output Format** — Added explicit before/after diff requirement between the revision and the engineer-handoff note.

## Rerun Command

To rerun with a live model when credentials are available:

```bash
python skills/trainer-optimize/scripts/run_optimize.py \
  --prompt-file .github/agents/student.agent.md \
  --train-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/train.jsonl \
  --val-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/val.jsonl \
  --iterations 3 --algorithm apo --beam-width 4 --branch-factor 4 --n-runners 4 \
  --judge-mode llm_judge
```
