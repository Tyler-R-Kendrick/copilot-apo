# Decision: student.agent.md — Iteration 1

## Target
`.github/agents/student.agent.md`

## Workspace
`.github/agents/.trainer-workspace/student.agent/`

## Outcome
**Student candidate applied.** The optimized prompt was written back to the source file.

## Changes Applied
5 minimal, targeted improvements:

1. **Stale-critique gate (Step 1)**: Added an explicit check that the supplied critique is current and actionable as the first step in the approach, before reading any workspace evidence.

2. **Smallest-change filter (Constraints)**: Tightened from "smallest defensible revision" to "exactly one critique point without modifying unrelated contract text."

3. **Engineer handoff trigger (inline + Constraints)**: Replaced the vague "needs prompt-engineering or Trace-oriented expertise" condition with two concrete cases: (a) revision involves prompt-engineering patterns such as few-shot, chain-of-thought, or structured output; (b) teacher-facing explanation needs clearer structure.

4. **Hard stopping criterion (Constraints + Step 6)**: Added explicit hard stop: if predicted teacher approval is still negative after two revision passes within this student turn, stop and request another teacher turn; do not attempt a third pass.

5. **Named validation command (Step 7 + Output Format)**: Specified `python -m pytest -q` as the concrete validation command in both the approach step and the output format section.

## Adversary Review
Adversary exploit attempted to replace smallest-change filter and hard stop with "most comprehensive revision" and unbounded loop. Exploit was not credible: training row 4 explicitly covers the hard stopping criterion and the judge would detect the missing stop. Adversary candidate ranked below student candidate.

## Validation
- `python -m pytest -q`: **856 passed** — no failures

## Iteration Artifacts
- `iterations/iteration-1/research/research-brief.json`
- `iterations/iteration-1/synthesize/datasets/train.jsonl` (6 rows, llm_judge)
- `iterations/iteration-1/synthesize/datasets/val.jsonl` (2 rows, llm_judge)
- `iterations/iteration-1/optimize/manual-followup-report.json`
- `iterations/iteration-1/optimize/optimized-prompt.md`
- `iterations/iteration-1/candidates/candidates.json`
- `iterations/iteration-1/steering/teacher/turn-1/STEERING.md`
- `iterations/iteration-1/steering/adversary/turn-1/STEERING.md`
- `iterations/iteration-1/validation/pytest.txt`
