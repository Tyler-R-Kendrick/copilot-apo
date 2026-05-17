## Goal

Assess the current `student.agent.md` as an optimization target for teacher-guided candidate revision work in trainer-led optimization loops, with emphasis on revision discipline, reasoning transparency, workspace artifact usage, and loop-exit behavior.

The optimization goal is to improve the student agent's ability to absorb teacher critique, apply the smallest defensible revision, expose clear reasoning trajectories, and correctly predict teacher approval — while avoiding over-iteration and loop inflation.

## Current Strengths

- The role is clearly scoped: absorb critique, revise, explain reasoning, predict teacher approval.
- The frontmatter correctly lists `teacher` and `engineer` handoff agents with precise prompts.
- The constraint section prohibits judge-usurpation and engineer-skill invocation, which prevents scope creep.
- The output format mandates explicit reasoning trajectory, which supports the teacher-student loop.
- The six-step approach mirrors the loop structure correctly and ends with validation.

## Main Risks

1. **No concrete evidence reading order.** Step 1 says "read the teacher goal, latest critique, current STEERING.md, summary files, and workspace evidence" — but does not specify how many artifacts to read, in what priority order, or when to stop gathering context and start revising.

2. **Approval prediction is weakly scoped.** Step 6 allows "at most one extra self-check" but does not specify what evidence would actually justify requesting another teacher turn vs. proceeding. This leaves the loop-exit condition ambiguous and allows premature self-approval.

3. **Teacher handoff trigger is imprecise.** "If the next revision target is unclear" in Step 2 is not observable. An agent that reads partial context may not know its target is unclear. A sharper trigger (e.g., "if the active steering artifact is older than the last optimize output") would reduce unnecessary teacher round-trips.

4. **Engineer handoff overlap with prompt engineering.** The constraint says "Do not use `engineer-prompt`, `engineer-code`, or any other engineer skills directly" and the body says "use the `engineer` handoff to format reasoning." The distinction between the engineer agent handoff (permitted) and engineer skills (prohibited) is unclear to a reader skimming the body. A clearer restatement would prevent accidental skill invocation.

5. **Reasoning format enumeration may inflate responses.** Step 3 lists chain-of-thought, tree-of-thought, chain-of-uncertainty-thought, and sketch-of-thought without guidance on which to use when. An agent might choose verbose formats when a single-chain explanation is sufficient, increasing token cost and reducing teacher readability.

6. **No stopping rule for loop exhaustion.** The constraints say stop when "the supplied evidence does not support a better candidate," but this condition is not observable. A student that has already completed 2–3 revisions with no improvement signal may not recognize this state. An explicit loop turn cap or diminishing-returns heuristic would help.

7. **Validation step is underspecified.** Step 7 says "run the relevant validation or measurement step" but does not say what constitutes a valid measurement for a prompt candidate, or what to do when validation fails.

## Rewrite Hypotheses

- Add an explicit evidence reading order: steering STEERING.md → teacher summary.md → optimize output → workspace evidence → stop.
- Sharpen the teacher handoff trigger: use "if no active steering artifact exists for this iteration, or the latest STEERING.md predates the last optimize output" as a concrete condition.
- Replace the reasoning-format enumeration with: "use the simplest reasoning format that makes the plan legible to the teacher; prefer chain-of-thought over tree-of-thought unless branching tradeoffs are the main uncertainty."
- Clarify the engineer distinction: "The `engineer` agent handoff formats your reasoning for teacher readability; do not call engineer skills (`engineer-prompt`, `engineer-code`, etc.) directly from this agent."
- Add loop-exit heuristic: "If you are on revision 3 or later with no teacher approval signal, report a loop-cap no-op with a summary of what changed across revisions."
- Tighten the approval-prediction step: "Predict teacher approval by checking the latest critique items against your revision — if any critique item is not addressed, either revise further or explain why it is out of scope."

## Suggested Metrics

- Teacher approval rate: percent of student revisions that receive teacher approval on the next turn without further revision requests.
- Revision precision: percent of revisions that change only the targeted critique item rather than unrelated sections.
- Loop-exit compliance: percent of runs that stop at or before the turn cap when no improvement signal is present.
- Reasoning trajectory completeness: percent of outputs that include plan, tradeoffs, and uncertainty in addition to the revision itself.
- Teacher-handoff appropriateness: percent of teacher handoffs that are justified by a concrete steering gap rather than uncertainty about the revision.

## Validation Plan

Run `python -m pytest -q` from the repository root after any rewrite to confirm no regressions. Review representative student outputs against train.jsonl eval rows for revision precision and reasoning completeness.

## Next Optimization Hypothesis

Focus the first pass on: (1) adding an explicit evidence reading order, (2) sharpening the teacher handoff trigger with an observable condition, (3) replacing the verbose reasoning-format list with a preference rule, and (4) adding a concrete approval-prediction check. Keep the rewrite minimal — structural clarity only.
