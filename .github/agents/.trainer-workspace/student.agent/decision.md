# Decision Summary — student.agent

**Target:** `.github/agents/student.agent.md`
**Iteration:** `iteration-1`
**Outcome:** Student candidate accepted and applied to source.

## What changed

The `student.agent.md` was optimized to address six weaknesses identified in `engineer-prompt/review.md`:

1. **Teacher handoff trigger** — replaced vague "whenever incomplete, contradictory, or stale" with three named conditions: (a) stale STEERING.md from prior iteration, (b) contradictory turn artifacts, (c) target unresolvable within two readings.
2. **Approval prediction rubric** — replaced open-ended "predict whether teacher would approve" with a three-criteria rubric: addresses named failure mode, no new scope, preserves interface.
3. **Artifact priority order** — added explicit priority: latest turn STEERING.md → summary.md → review.md → earlier turns.
4. **Reasoning format decision rule** — added concrete format selection: chain-of-thought (linear), tree-of-thought (mutually exclusive), chain-of-uncertainty-thought (missing facts), sketch-of-thought (exploratory).
5. **Validation definition of done** — `python -m pytest -q` for source changes; diff review for prompt-only changes.
6. **Over-revision check** — flag revisions that change more than two structural elements.

## Validation

`python -m pytest -q` → **856 passed**

## Optimizer mode

`manual_followup` — Copilot inference unavailable in sandbox. Agent-authored candidate used in place of scored APO output.

## Artifacts

- `iterations/iteration-1/optimize/manual-followup-report.json`
- `iterations/iteration-1/optimize/optimized-prompt.md`
- `iterations/iteration-1/candidates/candidates.json`
- `iterations/iteration-1/steering/teacher/turn-1/STEERING.md`
- `iterations/iteration-1/validation/pytest.txt`
