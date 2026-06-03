# Operator Follow-up

## Blocker

The external model API was unavailable (session authentication error). The optimizer ran in `manual_followup` mode.

## Agent Handoff Summary

The `@trainer` agent answered the `model_prompt` directly based on analysis of the 6 training rows and 3 validation rows. The optimized candidate addresses all dataset-identified behavioral gaps:

1. Added explicit evidence reading order as step 1 of the Approach section.
2. Added out-of-scope revision guard to the Constraints section.
3. Tightened teacher handoff trigger from over-broad 4-condition rule to 2-condition rule.
4. Tightened engineer handoff: only for reformatting the reasoning trajectory, not for any part of the revision.
5. Added explicit write-back step (step 7) specifying `candidates/student/` under the active iteration.
6. Capped self-checks at one before teacher handoff (reduced from open-ended loop to single self-check).

## Saved Artifacts

- Report: `.github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/manual-followup-report.json`
- Candidate: `.github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/optimized-prompt.md`

## Rerun Command

```bash
python skills/trainer-optimize/scripts/run_optimize.py \
  --prompt-file .github/agents/student.agent.md \
  --train-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/train.jsonl \
  --val-file .github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/datasets/val.jsonl \
  --iterations 3 --algorithm apo --beam-width 4 --branch-factor 4 --n-runners 4 --judge-mode llm_judge
```
