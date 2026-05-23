## Goal

Assess the current `student.agent.md` as an optimization target for teacher-guided candidate revision inside trainer-led optimization loops. The optimization focus is on revision discipline, reasoning transparency, and bounded teacher-handoff behavior.

A strong student agent should absorb teacher critique, implement the smallest defensible revision, expose a clear reasoning trajectory, and accurately predict whether the teacher would approve the result — stopping rather than looping indefinitely.

## Current Strengths

- The role is tightly scoped: absorb critique, revise, explain reasoning. No orchestration or judging.
- The constraints correctly prohibit judging, adversarial review, and trainer-loop orchestration.
- The approach section correctly references `steering/<agent>/turn-N/STEERING.md` and per-agent `summary.md` files as the guidance record.
- The `teacher` handoff condition ("incomplete, contradictory, stale, or needs fresh recommendation") is directionally correct.
- The `engineer` handoff is narrowly scoped to formatting the reasoning trajectory, not executing engineer skills directly.
- The output format lists six distinct sections that support downstream teacher review.

## Main Risks

1. **No explicit evidence reading priority.** Step 1 says "read the teacher goal, latest teacher critique, current teacher turn STEERING.md, relevant per-agent summary.md files, and workspace evidence," but gives no ordered priority. When multiple steering turns conflict, the agent has no rule for which artifact wins.

2. **Teacher-handoff trigger is underspecified.** The handoff condition "incomplete, contradictory, stale, or needs a fresh evidence-based recommendation" lacks concrete examples of each trigger, leading to inconsistent handoff behavior across runs (too many handoffs, or too few).

3. **"Smallest defensible revision" is undefined.** The constraint says "implement the smallest defensible candidate revision," but the approach never defines what makes a revision defensible — no criteria for correctness, no minimum bar for scope, and no guidance on how to handle revisions that are minimal but incorrect.

4. **Self-check loop exit condition is vague.** Step 6 says "do at most one extra self-check only if the draft still looks unsupported, incomplete, or misaligned," but "unsupported" is not given a concrete meaning. The agent may misuse this gate, either looping when it should stop or skipping the check when it is needed.

5. **Teacher approval prediction lacks a threshold.** The approach says "predict whether the teacher would approve the revision," but does not specify what confidence level or evidence triggers another teacher turn versus proceeding. This leads to vague approval predictions that do not reliably prevent incomplete candidates from advancing.

6. **No artifact output rule for justified no-ops.** The constraint says "report a justified no-op when the supplied evidence does not support a better candidate," but the output format does not include a no-op branch with any required structure (e.g., which evidence was inspected, why it was insufficient).

7. **Revision scope is ambiguous.** The agent targets "prompt, context, evaluation, or supporting implementation details that are actually in scope," but does not define what makes something in-scope versus out-of-scope in a given loop turn, leaving the boundary unclear when the teacher requests a broad change.

## Rewrite Hypotheses

- Add an explicit evidence reading order: (1) current teacher turn `STEERING.md` → (2) per-agent `summary.md` for latest agent → (3) current candidate → (4) prior teacher turn STEERING.md if needed for conflict resolution → stop and plan.
- Add a concrete teacher-handoff threshold: trigger the handoff when the current STEERING.md is older than the candidate under review, or when the critique asks for a change the current evidence cannot support.
- Add an inline definition of "defensible revision": a change that a) directly addresses a specific critique point, b) does not introduce new ambiguity or break existing constraints, and c) can be validated with at least one observable outcome.
- Replace the self-check condition with a two-question gate: (1) Does the revision address at least one specific critique point from the current STEERING.md? (2) Does the revision avoid introducing new constraints not present in the steering artifact? If both are yes, proceed; otherwise request teacher guidance.
- Add a teacher approval threshold: if the student estimates less than 70% confidence that the teacher would approve (e.g., the revision addresses only a subset of critique points, or the reasoning trajectory has unresolved uncertainty), trigger another teacher turn.
- Add a no-op output template: when reporting a justified no-op, state (a) the steering artifact inspected, (b) the candidate revision considered, (c) why no revision is supportable, and (d) the specific teacher guidance needed to unblock.
- Define in-scope revisions as those directly referenced in the current STEERING.md plus cascade effects on adjacent constraints; define out-of-scope as any change not grounded in the current steering artifact.

## Suggested Metrics

- Teacher-handoff precision: percent of teacher handoffs that produce new steering content versus returning unchanged guidance.
- Revision address rate: percent of revisions that directly address at least one critique point from the current STEERING.md.
- Approval prediction accuracy: percent of runs where the student's predicted approval matches the teacher's actual verdict.
- No-op quality: percent of no-op reports that include all four required fields (steering artifact, revision considered, reason, needed guidance).
- Loop efficiency: average number of student turns per approved candidate; lower is better.
- Constraint preservation rate: percent of revisions that do not introduce new ambiguity or break existing constraints.

## Validation Plan

Run `python -m pytest -q` from the repository root after applying any rewrite to confirm no regressions. Review representative student outputs using the `skills/trainer-train/` and `skills/trainer-optimize/` eval cases, focusing on whether the reasoning trajectory, revision, and approval prediction sections are populated and consistent with the current steering artifact.

## Next Optimization Hypothesis

Focus the first pass on: (1) adding an explicit evidence reading priority, (2) replacing the vague self-check condition with the concrete two-question gate, (3) adding an inline definition of "defensible revision," and (4) adding the approval-confidence threshold that triggers a teacher turn. Keep the rewrite minimal — structural clarity improvements only, without expanding the agent's scope or adding new tools.
