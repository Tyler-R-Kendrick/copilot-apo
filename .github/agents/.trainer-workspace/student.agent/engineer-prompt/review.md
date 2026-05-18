## Goal

Assess the current student agent as an optimization target for teacher-guided candidate revision inside trainer-led optimization loops, with emphasis on evidence reading order, self-check discipline, and artifact staging compliance.

The optimization target is operational reliability in multi-turn teacher-student loops. A strong student agent should read workspace evidence in a defined order, implement the smallest defensible revision that addresses teacher critique, predict teacher approval accurately, and produce staged artifacts that the workspace staging contract can consume.

## Current Strengths

- Role is sharply scoped: absorb critique, revise candidate, explain reasoning trajectory.
- Constraints correctly prohibit judging, adversarial review, and trainer-loop orchestration.
- The seven-step approach provides a reasonable progression from evidence reading to validation.
- The output format calls for reasoning trajectory exposure rather than answer-only output.
- The self-check step (step 6) prevents runaway looping by capping iterations.

## Main Risks

1. **No evidence order.** The approach says "read the teacher goal, latest teacher critique, current teacher turn STEERING.md" but does not specify which workspace paths to read, what to do when steering artifacts are absent, or how incomplete evidence should affect the revision.

2. **No artifact staging guidance.** The student agent produces a candidate revision but the prompt gives no guidance on writing it to `candidates/student/` with companion `description.md`, `predicted-judge-response.md`, and `reflection.md` files that the judge and adversary need.

3. **Ambiguous loop-exit criteria.** Step 6 says to stop when the draft still looks "unsupported, incomplete, or misaligned" without defining observable exit criteria. This leaves the student to decide subjectively when another teacher turn is needed.

4. **No validation artifact reference.** The output format calls for a validation or measurement result, but the approach gives no guidance on what to run, where to find existing test artifacts, or how to interpret pass/fail outcomes.

5. **No explicit workspace path reading.** Unlike the teacher and adversary agents that reference the iteration staging bundle, the student prompt does not mention `iterations/iteration-N/` structure, candidate directories, or steering summary files explicitly.

6. **Missing candidate comparison step.** The student should be able to compare its revision to the original before returning it, but the current prompt has no comparison step or quality threshold to reach.

## Rewrite Hypotheses

- Add an explicit evidence order: latest teacher critique and STEERING.md first, per-agent steering summaries second, current candidate and original prompt third, validation evidence last.
- Add an artifact staging step that writes the revision to `candidates/student/` with `description.md`, `predicted-judge-response.md`, and `reflection.md` companion files.
- Tighten the loop-exit criteria to: stop when teacher would clearly approve, or when the teacher handoff has already been used once in this turn with no new blocking critique.
- Reference `iterations/iteration-N/` structure for evidence reading to align with how teacher and adversary agents navigate workspace state.
- Add a brief candidate-vs-original comparison to verify the revision is narrowly scoped.
- Keep the rewrite minimal: evidence order, artifact staging, and tighter exit criteria should be enough for a first improvement pass.

## Suggested Metrics

- Teacher approval prediction accuracy: percent of runs where the student correctly predicts whether the teacher will approve.
- Artifact completeness: percent of runs where `candidates/student/` contains all required companion files.
- Loop termination rate: percent of runs that terminate within two teacher turns rather than looping indefinitely.
- Revision scope: average edit distance between original and student candidate (verify revisions remain minimal).
- Validation report presence: percent of runs that include an explicit validation result.

## Validation Plan

Run `python -m pytest -q` from the repository root after applying any rewrite. Review representative student outputs against synthesized eval cases for reasoning trajectory completeness and artifact staging compliance.

## Recommendation

The student agent is a valuable optimization target because it directly affects loop efficiency and candidate quality in every trainer run. The current agent has the right scoping and output-format discipline but lacks evidence order, artifact staging, and observable exit criteria.

Prioritize a rewrite that adds a fixed evidence order, an explicit artifact staging step, and measurable loop-exit conditions. Keep the core constraint set and approach steps — only add structure where the current prompt is ambiguous or missing.
