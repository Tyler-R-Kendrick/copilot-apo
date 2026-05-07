## Goal

Assess the current student agent as an optimization target for teacher-guided candidate revision work in prompt-optimization workflows, with emphasis on evidence-reading discipline, handoff precision, convergence logic, and artifact contract completeness.

The optimization target is revision quality and loop efficiency, not role expansion. A strong student agent should read teacher critique and workspace evidence in a defined order, apply the smallest defensible revision to the candidate, expose an explicit reasoning trajectory, and converge to a clear outcome rather than looping indefinitely.

## Current Strengths

- The role is sharply scoped: absorb teacher critique, revise the candidate, explain reasoning.
- The constraints correctly prohibit judging, adversarial review, and trainer-loop orchestration.
- The handoff contract distinguishes `teacher` (for incomplete or stale critique) from `engineer` (for formatting the reasoning trajectory), which is the right split.
- The output format calls for explicit reasoning trajectory, which supports teacher review.
- The convergence hint in step 6 ("at most one extra self-check") limits iteration drift.
- The steering artifact reference is correct: `steering/<agent>/turn-N/STEERING.md` and per-agent `summary.md`.

## Main Risks

1. **No evidence reading order.** Step 1 lists artifacts to read (teacher goal, critique, STEERING.md, summary.md, workspace evidence) but gives no sequencing rule for when evidence conflicts or is absent. An agent following this prompt may read artifacts in any order and resolve conflicts arbitrarily.

2. **Handoff conditions are imprecise.** "Unclear next revision target" and "task needs specialized coaching" are subjective thresholds. The teacher handoff condition could trigger on any mild ambiguity; the engineer handoff condition is doubly vague ("if the task needs" vs. "if the teacher-facing explanation needs"). Both lack concrete signals the agent can check without judgment.

3. **Convergence logic is weak.** Step 6 allows "at most one extra self-check" but does not define what counts as an approval-likely draft, what blocker condition forces another teacher turn, or when the loop should be declared done. "Approval still looks unlikely" is not a checkable predicate.

4. **Artifact contract is thin.** The output format lists what to state but does not describe the required content structure of the revision, the reasoning trajectory format, or what a "justified no-op" must contain. An agent can satisfy the format with a single sentence for each field.

5. **No trainer-specific revision focus.** The approach does not mention scoring mode awareness (llm_judge vs. deterministic vs. custom), dataset shape constraints, workspace contract compliance checks, or the distinction between authored evals and synthesized datasets — all surfaces where student revisions commonly fail in this repo.

6. **No minimum revision depth.** The "smallest defensible revision" instruction is correct in spirit but gives no threshold for what counts as defensible. An agent may produce a single-word change and declare it complete.

7. **No explicit candidate-vs-original diff requirement.** The output format does not require the agent to state what changed relative to the original candidate, which makes teacher review harder.

## Rewrite Hypotheses

- Add an explicit evidence reading order: teacher goal → active iteration STEERING.md → per-agent summary.md → current candidate → workspace evidence (optimize report, validation log, prior steering turns).
- Tighten handoff conditions to concrete signals: teacher handoff when the STEERING.md is absent, older than the current iteration, or contains an unresolved blocker flag; engineer handoff when the reasoning trajectory draft exceeds three paragraphs or the teacher-facing explanation references more than two distinct technical concepts.
- Promote convergence logic to an explicit step with a named stopping condition: stop when the revised candidate addresses all open blocker flags in the latest STEERING.md, or when the teacher handoff returns an approval signal.
- Expand the artifact contract to describe the expected content of the reasoning trajectory (step name, evidence consulted, conclusion, uncertainty level), the no-op justification (blocker flag quoted, evidence gap named), and the revision diff (original → revised mapping).
- Add a trainer-specific revision focus section: scoring mode awareness, dataset shape compliance, workspace staging contract, authored-eval vs. synthesized-dataset distinction, and manual-followup classification.
- Add a minimum revision depth check: the revision must change at least one instruction, constraint, or output requirement, or the no-op justification must cite at least one quoted blocker from the STEERING.md.
- Add a diff requirement to the output format: state what changed between the original candidate and the revised candidate using a before/after comparison.

## Suggested Metrics

- Blocker resolution rate: percent of runs where all open STEERING.md blocker flags are addressed in the revision.
- Artifact completeness: percent of runs where the reasoning trajectory, revision diff, and predicted-teacher-approval fields are all non-trivial.
- Handoff precision: percent of runs where the teacher handoff is triggered only when the STEERING.md is absent or blocked (not on mild ambiguity).
- Convergence rate: percent of runs that include a clear stopping statement ("all blockers resolved" or "teacher approval predicted").
- Trainer-specific revision recall: percent of runs that surface at least one trainer-specific gap (scoring mode, dataset shape, workspace contract) when the input contains such a gap.
- No-op quality: percent of no-op responses that cite a specific quoted blocker from the STEERING.md.

## Validation Plan

Run `python -m pytest -q` from the repository root after applying any rewrite to confirm no regressions in existing tests. Review representative student outputs against staged evals.json cases for artifact completeness and blocker resolution accuracy.

## Recommendation

This is a valuable optimization target because student revision quality directly affects trainer-loop convergence speed and teacher review burden. The current agent has the right role scoping and handoff structure but lacks evidence reading discipline, precise handoff conditions, explicit convergence logic, and a complete artifact contract.

Prioritize a rewrite that adds a fixed evidence order, tightened handoff conditions, and an explicit convergence stopping condition. Measure on blocker resolution rate and artifact completeness first. Add the trainer-specific focus section and diff requirement as a second pass if the first rewrite still produces shallow or non-converging revisions.
