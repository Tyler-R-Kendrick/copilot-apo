## Goal

Assess the current `student.agent.md` as an optimization target for teacher-guided candidate revision inside trainer-led prompt optimization loops, with emphasis on reasoning discipline, scope enforcement, and teacher-approval prediction accuracy.

The optimization target is candidate revision quality and loop efficiency. A strong student agent should absorb teacher critique accurately, produce the smallest defensible revision, surface an explicit reasoning trajectory, and produce an honest teacher-approval prediction — stopping rather than looping when the evidence does not support improvement.

## Current Strengths

- Role is clearly scoped to candidate revision, not judging or orchestration.
- Constraints explicitly prohibit using engineer skills directly and taking over judging or trainer orchestration.
- The approach steps include a teacher-approval forecast before finalizing.
- The output format requires explicit reasoning trajectory, plan, tradeoffs, and uncertainty — preventing answer-only output.
- Handoff triggers for `teacher` and `engineer` are described clearly.

## Main Risks

1. **Evidence reading order is underspecified.** The approach says "Read the teacher goal, latest teacher critique, current teacher turn STEERING.md, the relevant per-agent steering/summary.md files" but does not specify how many workspace artifacts to read before drafting, or what to do when steering artifacts are missing.

2. **No explicit blocker path for missing steering artifacts.** If `STEERING.md` or `steering/<agent>/summary.md` files are absent (e.g., first iteration), the agent has no instruction to request fresh teacher guidance or initialize its context from the available workspace files.

3. **Self-check loop is weakly bounded.** The approach allows "at most one extra self-check only if the draft still looks unsupported" — but does not define what "unsupported" means in a measurable way, leaving the decision open-ended.

4. **Teacher-approval forecast is underspecified.** The output requires a "predicted teacher approval outcome," but the approach gives no criteria for what constitutes approval vs. another loop turn, making forecasts subjective and unreliable.

5. **Engineer handoff trigger is vague.** The body says "when the task needs specialized prompt or Trace-oriented coaching" or "when the teacher-facing explanation needs clearer structure," but neither criterion is specific enough to make the trigger deterministic.

6. **No explicit stopping rule for indefinite loops.** The constraint says "do not loop indefinitely" but the only exit conditions mentioned are "teacher would approve" or "another teacher turn is needed," without a hard cap on self-check iterations.

## Rewrite Hypotheses

- Add a prioritized evidence reading order: latest STEERING.md → per-agent summary.md → current candidate text → workspace validation artifacts → then draft.
- Add an explicit fallback when steering artifacts are missing: hand off to `teacher` immediately rather than drafting from incomplete context.
- Tighten the self-check rule: allow exactly one self-check pass; if the draft is still unsupported, emit a justified no-op rather than continuing.
- Add 3–5 concrete teacher-approval criteria to the forecast step (e.g., revision is smallest change that addresses the critique, reasoning trajectory is explicit, no scope expansion, no evaluator fields in candidate text).
- Sharpen the engineer handoff trigger: invoke engineer only when (a) the agent's draft rationale is ambiguous or contradictory, or (b) the revision involves a prompt-engineering technique or Trace method the agent cannot resolve from context alone.
- Add a maximum turn cap: if the teacher-approval forecast is still negative after one self-check, stop and output a justified no-op with a recommendation for the trainer.

## Suggested Metrics

- Revision precision: percent of student candidates that make the smallest defensible change without scope expansion.
- Teacher-approval forecast accuracy: percent of forecasts that match the actual teacher verdict in the next steering turn.
- Reasoning trajectory completeness: percent of outputs that include plan, tradeoffs, and uncertainty in addition to the revision.
- Unnecessary loop rate: percent of student turns where an extra self-check loop ran but the teacher rejected the result anyway.
- Engineer handoff relevance: percent of engineer handoffs that produced a materially clearer explanation versus those that could have been omitted.

## Validation Plan

Run `python -m pytest -q` from the repository root after applying any rewrite to confirm no regressions. Review student outputs against trainer loop steering artifacts to confirm reasoning trajectory is explicit and teacher-approval forecasts are grounded in visible criteria.

## Next Optimization Hypothesis

Focus the first pass on: (1) adding a prioritized evidence reading order with an explicit missing-artifacts fallback, (2) tightening the self-check rule to exactly one pass with a no-op exit, and (3) adding 3–5 concrete teacher-approval criteria to the forecast step. Keep the rewrite minimal — structural discipline improvements without expanding the agent's scope.
