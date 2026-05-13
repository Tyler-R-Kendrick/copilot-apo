## Goal

Assess the current `student.agent.md` as an optimization target for teacher-guided candidate revision work in trainer-led prompt-optimization loops, with emphasis on evidence reading discipline, stopping-condition clarity, and artifact output structure.

The optimization target is revision reliability and teacher-loop efficiency. A strong student agent should read the right workspace evidence in the right order before drafting a revision, apply a concrete stopping condition rather than looping indefinitely, and expose the full reasoning trajectory rather than answer-only output so the teacher can calibrate quickly.

## Current Strengths

- The role is clearly scoped: draft or revise prompt candidates from teacher guidance, not orchestrate the loop.
- The constraints correctly prohibit taking over judging, adversarial review, or trainer-loop orchestration.
- The handoffs to `teacher` (for incomplete or stale critique) and `engineer` (for formatting) are well-specified and bounded.
- The "predict teacher approval" step is a sound feedback mechanism that prevents premature loop termination.
- The approach mentions an explicit one-extra-self-check cap, which limits runaway self-review.
- The output format lists chain-of-thought and tree-of-thought as explicit reasoning options, which is useful for teacher review.

## Main Risks

1. **No evidence reading order.** The approach says "read the teacher goal, latest teacher critique, current teacher turn STEERING.md, per-agent summary.md, and workspace evidence," but does not specify the read order or when to stop reading and start drafting. An agent with partial context may start revising prematurely.

2. **Stopping condition is vague.** "At most one extra self-check only if the draft still looks unsupported" does not define what "unsupported" means — missing workspace evidence? contradictory steering? unresolvable uncertainty? This can lead to inconsistent loop behavior across runs.

3. **"Smallest defensible revision" is undefined.** The constraint appears twice but the prompt never explains what makes a revision defensible: alignment with the latest steering artifact? absence of scope creep? passing the validation step? A concrete definition would tighten the revision surface.

4. **No guidance on conflicting critiques.** If teacher turn-1 and turn-2 contain contradictory recommendations, the current prompt offers no instruction. The agent may apply the latest critique blindly or mix guidance from multiple turns.

5. **Output format is overly broad.** The output section lists five distinct reasoning styles (chain-of-thought, tree-of-thought, chain-of-uncertainty-thought, sketch-of-thought, "another explicit reasoning format") without guidance on which to prefer for typical student revision work, leading to inconsistent output quality.

6. **No missing-evidence handling.** The approach says "if the next revision target is unclear, hand off to teacher," but does not address what to do when workspace evidence is missing entirely (no STEERING.md exists, no source snapshot). The agent may produce a no-op without reporting the blocker.

7. **Engineer handoff purpose overlap.** The engineer handoff says "format the reasoning and solution plan into a clearer teacher-ready explanation," but the output format already asks for chain-of-thought reasoning. It is unclear when the student should handle this itself versus handing off to the engineer.

## Rewrite Hypotheses

- Add an explicit evidence reading order as a numbered list: teacher goal → latest teacher critique → current turn STEERING.md → per-agent summary.md → workspace state (source snapshot, validation log) → then stop and plan.
- Replace the vague self-check cap with a concrete stopping condition: stop when (a) teacher would approve, (b) the revision scope was already the smallest addressable unit and the critique is fully satisfied, or (c) the draft is still unsupported after one self-check and a teacher turn is needed instead.
- Define "smallest defensible revision" concretely: the revision that addresses exactly the current steering artifact's focus without adding new constraints, expanding scope, or changing the prompt interface.
- Add a conflicting-critique resolution rule: when consecutive teacher turns conflict, prefer the most recent turn's guidance and note the conflict in the output.
- Narrow the output reasoning style: recommend chain-of-thought as the default; reserve tree-of-thought for branching design decisions and chain-of-uncertainty-thought for high-stakes tradeoff steps.
- Add a missing-evidence blocker path: if STEERING.md or source snapshot is absent, write a blocker artifact and hand off to the teacher rather than producing a no-op silently.
- Clarify the engineer handoff criterion: invoke `engineer` only when the reasoning explanation needs prompt-engineering or Trace-specific reframing, not just for basic output cleanup.

## Suggested Metrics

- Evidence order compliance: percent of runs where the agent reads evidence in the specified order before drafting.
- Stopping discipline: percent of runs that include an explicit stop-or-continue decision with justification.
- Revision scope accuracy: percent of revisions that address exactly the steering focus without introducing unrelated changes.
- Teacher approval prediction accuracy: calibration between predicted and actual teacher approval across multiple runs.
- Engineer handoff precision: percent of engineer handoffs that were justified by prompt-engineering or Trace-specific needs rather than general formatting.
- Blocker report quality: percent of runs with missing STEERING.md that produce a structured blocker artifact rather than a silent no-op.

## Validation Plan

Run `python -m pytest -q` from the repository root after applying any rewrite to confirm no regressions in existing tests. Review the optimized prompt against representative teacher/student loop eval cases for evidence-reading compliance and stopping-condition clarity.

## Next Optimization Hypothesis

Focus the first pass on: (1) adding an explicit evidence reading order, (2) adding a concrete stopping condition, (3) defining "smallest defensible revision," and (4) adding a missing-evidence blocker path. Keep the rewrite minimal — structural clarity improvements only, without expanding the agent's scope or adding new constraints.
