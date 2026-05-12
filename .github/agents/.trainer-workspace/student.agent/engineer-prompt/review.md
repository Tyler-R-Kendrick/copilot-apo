## Goal

Assess the current `student.agent.md` as an optimization target for teacher-guided candidate revision work in trainer-led optimization loops. The optimization goal is revision fidelity, reasoning transparency, and loop-exit discipline.

A strong student agent should: absorb teacher critique accurately, inspect workspace steering artifacts, implement the smallest defensible revision, expose the reasoning trajectory (plan, tradeoffs, uncertainty), and correctly predict teacher approval before finalizing. It should know when to request another teacher turn rather than looping indefinitely.

## Current Strengths

- Role is tightly scoped: absorb critique, implement smallest revision, expose reasoning trajectory.
- Constraints correctly prohibit taking over judging, adversarial review, or orchestration.
- Step 6 adds a self-prediction gate ("predict whether teacher would approve") before finalizing.
- Handoff to `engineer` for formatting the reasoning trajectory is well-motivated.
- Output format includes plan, reasoning, tradeoffs, uncertainty, predicted approval, and validation — a complete set.

## Main Risks

1. **No evidence reading order before drafting.** Step 1 says "Read the teacher goal, latest critique, current turn STEERING.md, relevant summary.md files, and workspace evidence," but lists them in a flat way with no priority. An agent may start editing before reading all steering context, or miss an older summary that contradicts the latest critique.

2. **"Smallest defensible revision" is undefined.** The constraint and step 5 both use "smallest defensible candidate revision" without defining what makes a revision defensible (e.g., does it need to cite the steering artifact it addresses?). Agents may apply arbitrarily large rewrites and claim minimality.

3. **Loop-exit criterion is underspecified.** Step 6 caps self-checks at "at most one extra self-check only if the draft still looks unsupported." This is an internal thought-process rule, not a loop-exit rule. The agent is not told what to output when it concludes the loop should exit vs. when another teacher turn is truly needed — it might keep requesting teacher turns indefinitely.

4. **Reasoning format choice is unconstrained.** The agent can pick any of several reasoning formats (chain-of-thought, tree-of-thought, chain-of-uncertainty-thought, sketch-of-thought) "when useful," but there is no guidance on which format fits which revision scenario. Agents may default to the most verbose format regardless of revision complexity.

5. **No definition of "unclear revision target."** Step 2 says "if the next revision target is unclear, hand off to teacher." But there is no concrete criterion for what makes a target unclear. This could cause premature teacher handoffs for simple revisions, increasing loop length unnecessarily.

6. **Validation step is underspecified.** Step 7 says "run the relevant validation or measurement step and report what changed," but does not name which validation commands are appropriate (e.g., `python -m pytest -q`) or how to interpret the output.

## Rewrite Hypotheses

- Add an explicit evidence reading order: turn STEERING.md → per-agent summary.md → teacher goal → current candidate → workspace evidence → stop and plan. This prevents premature editing.
- Define defensibility: a revision is defensible when it addresses exactly the critique cited in the active steering artifact and does not expand scope, change the agent's interface, or alter unrelated behavior.
- Add a concrete loop-exit rule: if the predicted approval is "yes" → finalize and return; if "no" and one self-check was already done → request another teacher turn with a specific rationale; never loop beyond two self-checks before escalating.
- Add reasoning format guidance: use sketch-of-thought for small, obvious revisions; chain-of-thought for multi-step rewrites; chain-of-uncertainty-thought when the critique is ambiguous or the revision has side effects; tree-of-thought only when multiple branching revision paths need explicit comparison.
- Clarify "unclear revision target": the target is unclear when the latest steering artifact does not name a specific file section, behavior, or constraint to change. If the steering artifact names a target, proceed even if the solution path is uncertain.
- Specify validation: run `python -m pytest -q` from the repository root after any revision that touches prompt text; report pass/fail count and any new failures.

## Suggested Metrics

- Revision scope compliance: percent of revisions that address exactly the cited critique without scope expansion.
- Reasoning transparency: percent of outputs that include plan, tradeoffs, and uncertainty (not just the revised text).
- Loop-exit discipline: percent of turns that correctly finalize when predicted approval is "yes" vs. percent that unnecessarily request another teacher turn.
- Validation compliance: percent of runs that execute the named validation command and include pass/fail output.
- Teacher approval prediction accuracy: percent of predicted approvals that match actual teacher verdicts in multi-turn logs.

## Validation Plan

Run `python -m pytest -q` from the repository root after applying any rewrite to confirm no regressions. Spot-check representative student outputs against turn-scoped STEERING.md artifacts to verify the revision addresses the cited critique and exposes the full reasoning trajectory.

## Next Optimization Hypothesis

Focus the first pass on: (1) adding an explicit evidence reading order, (2) defining what makes a revision defensible, (3) specifying a concrete loop-exit criterion, and (4) naming `python -m pytest -q` as the validation command. Keep the rewrite structural — do not expand scope or add new handoffs.
