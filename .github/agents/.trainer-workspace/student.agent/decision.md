# Decision Summary — student.agent.md

## Target
`.github/agents/student.agent.md`

## Iteration
`iteration-1`

## Selection Reason
`.github/agents/student.agent.md` was the first `.agent.md` file (alphabetically) among agent files without an existing trainer workspace. Files `adversary.agent.md`, `conservator.agent.md`, `engineer.agent.md`, `judge.agent.md`, and `researcher.agent.md` already had workspaces.

## Optimization Mode
`manual_followup` — no live inference model available. The `@trainer` agent answered the `model_prompt` from the APO manual-followup payload directly.

## Changes Applied

Six improvements from the engineer-prompt review were applied, plus a bonus improvement (unbundling rule):

1. **Evidence reading order** — Approach step 1 now mandates: latest turn STEERING.md → per-agent summary.md → current candidate → teacher goal → workspace artifacts.
2. **Operational teacher handoff conditions** — Three testable rules replace the vague "incomplete, contradictory, stale": (a) no actionable revision target, (b) critique authored before current candidate, (c) critique requests another turn.
3. **Engineer handoff scoped to output formatting** — Explicit prohibition on using engineer handoff for subject-matter revision decisions.
4. **Loop exit rule** — Three concrete exit conditions: high-confidence approval, documented no-op, or 2 self-checks with uncertain approval → teacher handoff.
5. **Workspace output step** — Approach step 7: write `steering/student/turn-N/STEERING.md`.
6. **Conflict resolution rule** — Latest turn STEERING.md is authoritative when it conflicts with summary.md.
7. **Unbundling rule (bonus)** — Approach step 5: do not bundle multiple critique items unless teacher requests it.

## Teacher Verdict
APPROVE for write-back (turn-1). All six engineer-prompt risks resolved. Open minor concern: teacher handoff condition (2) has no tie-breaking fallback when temporal ordering is ambiguous.

## Adversary Verdict
CREDIBLE_EXPLOIT — three exploits found targeting `llm_judge` surface compliance. Extra judge steering added. Exploits do not undermine prompt soundness.

## Validation
856 tests passed. 0 failures. `python -m pytest -q` from repository root.

## Key Artifacts
- Optimized prompt: `.github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/optimized-prompt.md`
- Optimize report: `.github/agents/.trainer-workspace/student.agent/iterations/iteration-1/optimize/manual-followup-report.json`
- Teacher steering: `.github/agents/.trainer-workspace/student.agent/iterations/iteration-1/steering/teacher/turn-1/STEERING.md`
- Adversary steering: `.github/agents/.trainer-workspace/student.agent/iterations/iteration-1/steering/adversary/turn-1/STEERING.md`
- Candidates manifest: `.github/agents/.trainer-workspace/student.agent/iterations/iteration-1/candidates/candidates.json`
- Validation log: `.github/agents/.trainer-workspace/student.agent/iterations/iteration-1/validation/pytest.txt`
