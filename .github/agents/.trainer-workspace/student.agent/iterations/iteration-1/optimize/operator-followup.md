# Operator Follow-up: Manual Optimization

## Blocker

The `trainer-optimize` runtime could not reach an external model due to missing authentication credentials (`Session was not created with authentication info or custom provider`). The optimizer returned `manual_followup` mode with a `model_prompt` describing the optimization task.

## Handoff

The `model_prompt` was answered manually by producing an improved version of the student agent prompt. The improvements address the key weaknesses identified in `engineer-prompt/review.md`:

1. **Evidence reading order** — Added an explicit 5-step reading sequence (teacher goal → STEERING.md → summary.md → candidate → workspace evidence).
2. **Handoff conditions** — Replaced vague conditions with concrete, checkable signals (STEERING.md absent/outdated/blocked for teacher; paragraph length/concept count for engineer).
3. **Convergence logic** — Added explicit stopping conditions tied to BLOCKER flag resolution and teacher approval signals.
4. **Artifact contract** — Expanded the output format with required fields for reasoning trajectory steps, justified no-ops, and before/after diffs.
5. **Trainer-specific focus** — Added a constraint requiring scoring mode awareness, dataset shape compliance, workspace staging contract checks, and authored-eval vs. synthesized-dataset distinction.
6. **Minimum revision depth** — Added requirement that a revision must change at least one instruction, constraint, or output requirement; otherwise submit a justified no-op citing a specific BLOCKER.

## Next Step

The optimized prompt is saved at:
`iterations/iteration-1/optimize/optimized-prompt.md`

Apply it to `.github/agents/student.agent.md` and run pytest to validate no regressions.
