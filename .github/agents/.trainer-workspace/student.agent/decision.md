# Decision: student.agent.md Optimization — Iteration 1

## Selected Target
`.github/agents/student.agent.md`

## Selection Reason
First `.agent.md` file without an existing trainer workspace (alphabetical tiebreaker among `student`, `teacher`, `trainer`).

## Workspace Root
`.github/agents/.trainer-workspace/student.agent/`

## Outcome
**Student candidate wins.** Applied to source file and validated.

## Changes Applied

1. **Evidence reading order** (Approach step 1): Added explicit requirement to read active `STEERING.md` and per-agent `summary.md` before taking any other action.
2. **Out-of-scope revision guard** (Constraints): Added explicit rule against changing placeholders, eval shapes, or constraints without teacher steering authorization.
3. **Tightened teacher handoff trigger**: Added inline restriction after original trigger sentence — limit to cases where critique references unreadable evidence or directly contradicts steering summary.
4. **Tightened engineer handoff**: Added inline clarification that engineer handoff is for reasoning trajectory formatting only, not for implementing or correcting the revision.
5. **Write-back step** (Approach step 7): Added explicit step to save revised candidate to `candidates/student/` under the active iteration directory.
6. **Self-check cap** (Approach step 6): Narrowed from open-ended "at most one extra self-check" to explicit: one self-check, then hand off to teacher if still uncertain.
7. **Argument-hint updated**: Added `active iteration STEERING.md path` to the argument hint.
8. **Engineer frontmatter prompt updated**: Clarified that engineer should not revise the candidate.

## Adversary Evaluation
Adversary candidate explored surface-level compliance exploit (preamble vs. numbered step, incomplete scope guard). Conclusion: not credible — student candidate wins on all 6 key criteria.

## Validation
All 856 tests passed (`python -m pytest -q`).
Log: `iterations/iteration-1/validation/pytest.txt`

## Key Artifacts
- Research brief: `iterations/iteration-1/research/research-brief.md`
- Train dataset: `iterations/iteration-1/synthesize/datasets/train.jsonl` (6 rows)
- Val dataset: `iterations/iteration-1/synthesize/datasets/val.jsonl` (3 rows)
- Eval manifest: `iterations/iteration-1/synthesize/evals/evals.json`
- Optimize report: `iterations/iteration-1/optimize/manual-followup-report.json`
- Optimized candidate: `iterations/iteration-1/optimize/optimized-prompt.md`
- Teacher steering: `iterations/iteration-1/steering/teacher/turn-1/STEERING.md`
- Candidates manifest: `iterations/iteration-1/candidates/candidates.json`
