# Training Decision — student.agent.md

## Iteration: iteration-1
## Decision: APPLY

## Summary

The iteration-1 optimization loop for `.github/agents/student.agent.md` produced a validated improvement. The optimized student agent prompt was applied to the target file and all 856 existing tests pass.

## Changes Applied

Six structural improvements were made to the prompt body (YAML frontmatter unchanged):

1. **Evidence reading order** — Added explicit 5-step sequence (teacher goal → STEERING.md → summary.md → candidate → workspace evidence) with conflict resolution rule.
2. **Handoff conditions** — Tightened teacher handoff to trigger on absent/outdated/blocked STEERING.md; tightened engineer handoff to trigger on trajectory length/complexity signals. Original sentences preserved for test contract compliance.
3. **Convergence logic** — Added named stopping conditions ("All BLOCKER flags resolved; loop complete" and "Teacher approval received; loop complete").
4. **Artifact contract** — Added structured requirements for reasoning trajectory steps (step name, evidence, conclusion, uncertainty), no-op justifications (quoted BLOCKER flag), and revisions (before/after diffs).
5. **Trainer-specific focus** — Added constraint requiring scoring mode awareness, dataset shape compliance, workspace staging contract checks, and authored-eval vs. synthesized-dataset distinction.
6. **Minimum revision depth** — Added requirement that revisions must change at least one instruction/constraint/output requirement; no-ops must cite a specific BLOCKER.
7. **Output format** — Added convergence decision field and structured reasoning trajectory fields to the output format section.

## Optimizer Result

- Mode: `manual_followup` (no model credentials available in this environment)
- Model prompt answered manually based on engineer-prompt review analysis
- Artifacts saved at: `iterations/iteration-1/optimize/`

## Validation

- pytest: **856 passed, 0 failed**
- Test contract: all `test_student_agent_contract_structure` assertions preserved

## Teacher Approval

Approved in `iterations/iteration-1/steering/teacher/turn-1/STEERING.md`. All six primary blockers resolved.

## Open Items for Future Iterations

- Uncertainty level definitions (high/medium/low scale is currently undefined)
- Engineer handoff threshold calibration (three paragraphs / two concepts may need adjustment)

## Recommendation

Apply the optimized prompt. Monitor for false-positive handoff rates and shallow artifact contract outputs in real loops. If observed, target those specific requirements in iteration-2.
