## Goal

Assess the current `student.agent.md` as an optimization target for teacher-guided candidate revision work in prompt-optimization loops, with emphasis on evidence reading discipline, revision correctness, loop exit behavior, and teacher handoff quality.

The optimization target is revision reliability and reasoning transparency. A strong student agent should read teacher critique and workspace steering before drafting revisions, implement the smallest defensible change, expose reasoning trajectory explicitly, and correctly predict whether the teacher would approve before finishing a turn.

## Current Strengths

- The role is clearly scoped: revise candidates from teacher critique, not orchestrate the loop.
- The constraint "implement the smallest defensible candidate revision" correctly limits scope creep.
- The approach section includes a teacher-approval prediction step before finalizing, which supports loop termination.
- The handoffs to `teacher` and `engineer` are clearly labeled and constrained to specific triggers (stale critique, formatting needs).
- The output format requires explicit reasoning trajectory (plan, tradeoffs, uncertainty), which prevents answer-only output.

## Main Risks

1. **Ambiguous evidence reading order.** The approach says "Read the teacher goal, latest teacher critique, current teacher turn `STEERING.md`…", but does not specify which artifact to read first or how many turns back to look for context. An agent with conflicting or multi-turn steering may act on stale critique.

2. **No explicit revision scope guard.** The constraint "implement the smallest defensible candidate revision" is correct, but there is no guidance on what counts as out-of-scope for a student revision — for example, changing prompt interface/placeholders, removing constraints, or modifying eval shapes. This allows scope creep in practice.

3. **Loop exit conditions are underspecified.** The agent says "do at most one extra self-check only if the draft still looks unsupported", but the exit criteria ("teacher would approve", "no further supported improvement") depend on prediction quality that may be unreliable. There is no explicit maximum turn count or fallback rule when the agent cannot determine whether teacher approval is likely.

4. **Teacher handoff trigger is too broad.** The condition "critique is incomplete, contradictory, stale, or needs a fresh evidence-based recommendation" covers nearly any case where the agent is uncertain. Without a minimum confidence threshold or a more concrete trigger, the agent may overuse the teacher handoff instead of making a reasonable revision.

5. **Engineer handoff purpose is insufficiently constrained.** The trigger "needs prompt-engineering or Trace-oriented expertise, or the teacher-facing explanation needs clearer structure" is vague. The agent is told not to invoke engineer skills directly, but the boundary between "formatting" and "delegating the revision" is unclear.

6. **No artifact write-back contract.** The agent describes reading workspace evidence and making revisions, but does not specify how the revised candidate is persisted (which file, which path, what format) or how the teacher can locate it in the workspace tree.

## Rewrite Hypotheses

- Add an explicit evidence reading order: active `STEERING.md` for the current turn → per-agent `steering/<agent>/summary.md` → current candidate file → most recent optimize or validate artifact → then plan.
- Add an explicit out-of-scope guard: do not change prompt interface placeholders, eval shapes, or constraints unless the teacher steering explicitly authorizes that change.
- Strengthen loop exit: add a maximum of two self-checks and specify that if teacher-approval prediction still looks uncertain after two checks, the agent should hand off to teacher rather than looping.
- Tighten teacher handoff trigger: hand off to teacher only when the critique explicitly references evidence the agent cannot read, or when the critique from the most recent turn directly contradicts the current steering summary.
- Tighten engineer handoff trigger: use engineer handoff only to reformat the reasoning trajectory artifact (the teacher-facing explanation), not for any part of the candidate revision itself.
- Add a write-back step: specify that the student saves the revised candidate to the correct `candidates/student/` path under the active iteration directory.

## Suggested Metrics

- Revision precision: fraction of revisions that change only the content authorized by the teacher critique without touching out-of-scope elements.
- Teacher approval prediction accuracy: fraction of turns where the agent's approval prediction matches whether the teacher actually approves in the next turn.
- Handoff overuse rate: fraction of student turns that trigger a teacher handoff when the available steering evidence already provides a clear direction.
- Reasoning trajectory completeness: fraction of outputs that include all required sections (steering followed, plan, tradeoffs, uncertainty, predicted approval).
- Write-back compliance: fraction of student turns that persist the revised candidate to the correct workspace path.

## Validation Plan

Run `python -m pytest -q` from the repository root after any rewrite to confirm no regressions. Review representative student outputs against expected workspace artifact layout and revision scope discipline.

## Next Optimization Hypothesis

Focus the first pass on: (1) adding an explicit evidence reading order, (2) adding a clear out-of-scope revision guard, (3) tightening the teacher handoff trigger to avoid overuse, and (4) specifying the candidate write-back path. Keep the rewrite minimal — structural improvements only, without expanding the scope of the student role.
