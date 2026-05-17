# Decision: student.agent.md — iteration-1

## Selected Candidate

`candidates/student/student.agent.md` — trainer agent manual-followup inference, refined with teacher approval.

## Changes Applied to Source File

**Target**: `.github/agents/student.agent.md`

All six improvements from the engineer-prompt review were applied while preserving every test-pinned contract phrase:

1. **Evidence reading order** — Approach step 1 specifies: STEERING.md → summary.md → optimize output → context. Stop once revision target is clear.
2. **Observable teacher handoff trigger** — Added concrete conditions: "when no active STEERING.md exists for the current iteration, or when the latest STEERING.md predates the last optimize output." The original "whenever the critique is incomplete, contradictory, stale" language is preserved.
3. **Engineer distinction clarified** — Both preamble and Constraints explicitly say "Do not invoke engineer skills directly yourself. Do not use `engineer-prompt`, `engineer-code`, or any other engineer skills directly."
4. **Reasoning format preference added** — Approach step 3 sets chain-of-thought as the default while retaining the full format list (chain-of-thought, tree-of-thought, chain-of-uncertainty-thought, sketch-of-thought).
5. **Approval prediction grounded** — Constraint updated to "checking each critique item from the latest STEERING.md against the revision" while preserving the `pre-emptively predict` phrase required by tests.
6. **Loop-cap no-op added** — New constraint: revision 3+ with no approval signal → summarize and request teacher turn.
7. **Revision scope constraint** — "change only the section or step that the critique targets unless the teacher explicitly approves a broader change."
8. **Validation step named** — Step 7 now specifies `python -m pytest -q` with qualification for non-Python artifacts.

## Adversary Finding

Primary credible exploit (Stale STEERING.md Invisible Bypass, predicted score 0.83) exposes a judge evaluation gap — timestamp-checking compliance is invisible in output format. The prompt correctly specifies the trigger; exploit reveals that future eval cases should test stale-STEERING scenarios explicitly. Extra judge steering added to block the exploit pattern.

## Validation Result

`856 passed` — no regressions.

## Status

`complete`
