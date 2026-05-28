## Goal

Assess the current `student.agent.md` as an optimization target for teacher-guided candidate revision inside trainer-led optimization loops, with emphasis on reasoning-trajectory quality, revision discipline, teacher-approval prediction accuracy, and workspace navigation clarity.

The optimization target is revision fidelity and loop-exit discipline. A strong student agent should read the correct workspace artifacts in order, implement the smallest defensible revision that addresses the current critique, expose a credible reasoning trajectory, and predict teacher approval reliably enough to avoid unnecessary loop turns.

## Current Strengths

- Role scope is clearly bounded: revise candidates from teacher critique, do not orchestrate loops or perform judging.
- Constraints correctly prohibit engineer-skill invocation and answer-only output.
- The "smallest defensible revision" rule is explicit and prevents scope creep.
- The "at most one extra self-check" rule bounds the self-review loop.
- The output format requires explicit reasoning trajectory exposure, which supports teacher inspection.
- Handoffs to `teacher` and `engineer` are named, with distinct triggering conditions.

## Main Risks

1. **No evidence reading order.** The approach says "Read the teacher goal, latest teacher critique, current teacher turn STEERING.md, the relevant per-agent summary.md files… and the current workspace evidence," but does not specify a priority or stopping point. An agent may read partial context and draft a revision prematurely or read too many artifacts and lose focus.

2. **Teacher-approval prediction criteria are undefined.** The constraint says to "predict whether the teacher would approve the revision," but gives no rubric for what approval looks like. Without criteria, this prediction is either trivially optimistic or structurally useless.

3. **Engineer handoff triggering condition is vague.** "When the task needs specialized prompt or Trace-oriented coaching, or when the teacher-facing explanation needs clearer structure" is too subjective. Agents may invoke engineer unnecessarily or skip it when the explanation genuinely needs restructuring.

4. **Validation step is underspecified.** "Run the relevant validation or measurement step" does not say which step, when it is required, or what to do when no deterministic check exists. Agents may skip validation silently or run unrelated tests.

5. **Blocker reporting format is absent.** When critique is "incomplete, contradictory, stale," the agent should hand off to teacher — but no structured blocker report format or fallback is defined if teacher guidance remains unavailable after the handoff.

6. **Loop exit is only partially specified.** "Do at most one extra self-check" bounds the self-review loop, but does not address when the student should escalate to the trainer rather than continuing indefinitely with teacher handoffs.

7. **Workspace artifact paths are implicit.** The approach references "current workspace evidence" and "per-agent summary.md files" without naming which directories to inspect first, leaving agents to guess the workspace layout.

## Rewrite Hypotheses

- Add an explicit evidence reading order: (1) active iteration's `steering/<agent>/turn-N/STEERING.md`, (2) per-agent `steering/<agent>/summary.md`, (3) current candidate prompt text, (4) teacher critique, (5) workspace `workflow-status.json` for iteration state — then stop and plan.
- Add an inline teacher-approval prediction rubric: the teacher would approve when the revision addresses the specific critique bullet, does not introduce new scope, and the predicted response aligns with the requested changes without requiring another round.
- Replace the vague engineer-handoff condition with two clear triggers: (a) the reasoning trajectory contains a domain-specific claim about token efficiency, grounding technique, or Trace patterns that the student cannot justify independently; (b) the teacher-facing explanation is longer than needed and could be shortened without losing any justification.
- Specify the validation step: always run `python -m pytest -q` from the repository root when the revision touches a tracked file; report "validation skipped — draft candidate only" otherwise.
- Add a blocker report format: if teacher guidance is unavailable, missing, or contradictory after one handoff attempt, write a short blocker note under `steering/student/turn-N/STEERING.md` and stop rather than looping further.
- Add a loop-escalation rule: after two consecutive student turns without teacher approval, escalate to the trainer with a summary of what changed and why approval was not reached.

## Suggested Metrics

- Revision compliance: percent of revisions that address the specific critique without expanding scope.
- Teacher-approval prediction accuracy: percent of predictions that match the actual teacher verdict in the next turn.
- Evidence reading efficiency: average number of workspace artifacts read before drafting a revision (lower is better when the evidence reading order is clear).
- Engineer handoff precision: percent of engineer handoffs that result in a materially shorter or clearer teacher-facing explanation.
- Validation coverage: percent of revisions that include a validation step result.
- Loop escalation rate: percent of runs that escalate to the trainer after two unresolved student turns (should be low when teacher guidance is clear).

## Validation Plan

Run `python -m pytest -q` from the repository root after applying any rewrite to confirm no regressions. Inspect the `skills/trainer-train/references/collaboration-contract.md` for the student agent's expected role in the collaboration chain and confirm the rewrite stays compatible with that contract.

## Next Optimization Hypothesis

Focus the first pass on: (1) adding an explicit evidence reading order, (2) adding an inline teacher-approval prediction rubric, (3) tightening the engineer-handoff triggering condition to two concrete cases, and (4) specifying the validation step and blocker report format. Keep the rewrite minimal — structural clarity improvements without adding new constraints or changing the agent's scope.
