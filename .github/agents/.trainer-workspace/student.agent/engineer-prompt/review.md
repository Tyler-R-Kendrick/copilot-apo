## Goal

Assess the current `student.agent.md` as an optimization target for teacher-guided candidate revision work in trainer-led optimization loops. Emphasis on revision discipline, reasoning trajectory quality, teacher-prediction accuracy, and handoff boundary compliance.

The optimization goal is to produce a student agent that implements the smallest defensible candidate revision per loop turn, exposes an explicit reasoning trajectory the teacher can follow, predicts teacher approval accurately before finalizing, and hands off to the teacher or engineer only when the steering is genuinely incomplete.

## Current Strengths

- Role is clearly scoped: absorb teacher critique, revise candidates, and explain reasoning trajectory.
- Constraints correctly prohibit taking over judging, adversarial review, or trainer orchestration.
- The approach section establishes a sensible turn structure: read steering → draft revision → predict teacher approval → refine → validate.
- The requirement to use chain-of-thought or similar explicit reasoning format is present, which supports transparency.
- The "smallest defensible revision" framing is well-aligned with APO goals of incremental improvement.
- The output format lists distinct sections (steering followed, reasoning trajectory, revision, engineer-handoff note, teacher prediction, validation).

## Main Risks

1. **Approval prediction is under-specified.** Step 6 says "predict whether the teacher would approve" and do one self-check, but does not say what constitutes evidence of teacher approval vs. disapproval, or what specific attributes the teacher would look for. Without this, the prediction is superficial.

2. **Teacher handoff trigger is vague.** Step 2 says "If the next revision target is unclear, explicitly hand off to teacher for refreshed guidance before editing." But "unclear" is ambiguous — an agent could over-trigger (handing off for any uncertainty) or under-trigger (proceeding when steering is actually stale or contradictory).

3. **Engineer handoff is ambiguous.** The contract says to use `engineer` handoff when "the task needs specialized prompt or Trace-oriented coaching" or when "the teacher-facing explanation needs clearer structure." But the agent may not know when clarity is sufficient without an explicit threshold. This can lead to either skipping the handoff when it would help, or delegating formatting when it isn't needed.

4. **Revision scope is not anchored to iteration artifacts.** The approach says to "Read the teacher goal, latest teacher critique, current teacher turn STEERING.md, the relevant per-agent steering summary files…" but does not specify the priority order when these artifacts conflict or when some are missing.

5. **Reasoning format guidance is aspirational but not operational.** The instruction to use "chain-of-thought, tree-of-thought, chain-of-uncertainty-thought, sketch-of-thought, or another explicit reasoning format" lists options but does not say when to choose each one, which can lead to random format selection.

6. **Validation step lacks a definition of done.** Step 7 says "Run the relevant validation or measurement step and report what changed." But what constitutes the right validation step for a candidate revision — a pytest run, a manual diff inspection, a scoring check — is left unspecified. An agent may skip validation entirely or run an inappropriate check.

7. **Loop exit criterion is one-sided.** The agent is told to do "at most one extra self-check" but the primary check has no explicit exit criterion. The agent could declare success too early if the first draft looks superficially reasonable.

## Rewrite Hypotheses

- Tighten the teacher handoff trigger: hand off to teacher when (a) the steering artifact is more than one iteration old, (b) multiple STEERING.md files contradict each other, or (c) the revision goal cannot be derived from the latest STEERING.md within two readings.
- Tighten the approval prediction: the teacher would approve when the revision addresses the named failure mode in the latest STEERING.md, does not introduce new scope, and preserves all existing prompt interface elements. Use this as the prediction rubric.
- Specify the priority order for steering artifacts: latest turn STEERING.md > per-agent summary.md > engineer-prompt/review.md > earlier turns.
- Replace aspirational reasoning format guidance with a concrete decision rule: use chain-of-thought for linear revisions, tree-of-thought when multiple mutually exclusive options exist, chain-of-uncertainty-thought when key facts are missing or contested.
- Add a concrete definition of done for the validation step: run `python -m pytest -q` after any change to a source file; run a diff review after any change to a prompt-only file.
- Add an explicit over-revision check: if the candidate changes more than two structural elements (sections, constraints, approach steps, output fields) compared to the original, flag the revision as potentially too broad before finalizing.

## Suggested Metrics

- Teacher approval prediction accuracy: percent of turns where the student's prediction matches the teacher's actual response.
- Revision scope compliance: percent of revisions that change two or fewer structural elements.
- Handoff precision: percent of teacher handoffs triggered by one of the three explicit trigger conditions vs. open-ended uncertainty.
- Validation step coverage: percent of revisions followed by a `python -m pytest -q` run or explicit diff review.
- Reasoning trajectory quality: presence of explicit chain-of-thought, tree-of-thought, or sketch-of-thought notation in the revision output.
- No-op justification rate: percent of no-op responses that include an explicit evidence citation for why no revision is supported.

## Validation Plan

Run `python -m pytest -q` from the repository root after applying any rewrite to confirm no regressions. Review representative student outputs against the student's eval cases to check reasoning trajectory completeness, handoff trigger discipline, and approval prediction quality.
