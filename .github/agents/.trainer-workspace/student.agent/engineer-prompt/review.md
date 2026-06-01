## Goal

Assess the current `student.agent.md` as an optimization target for teacher-guided candidate revision work inside trainer-led optimization loops, with emphasis on teacher handoff discipline, revision scope control, approval prediction accuracy, and reasoning trajectory clarity.

The optimization target is revision reliability and teacher-loop efficiency. A strong student agent should implement the smallest defensible candidate revision from concrete steering artifacts, predict teacher approval with enough specificity to self-correct once before escalating, use the teacher handoff only when the critique is incomplete or contradictory, and expose a reasoning trajectory that is useful for the teacher to evaluate — not just a final answer.

## Current Strengths

- The role is clearly scoped: implement teacher-guided candidate revisions, not orchestration, judging, or adversarial review.
- The constraint "implement the smallest defensible candidate revision" is explicit and actionable.
- The approval prediction step (step 6) is a useful self-check that distinguishes this agent from naive implementers.
- The teacher and engineer handoff labels are clearly named with distinct purposes.
- The output format lists distinct sections (steering, trajectory, revision, engineer handoff, approval, validation).

## Main Risks

1. **Teacher handoff trigger is vague.** Step 2 says "if the next revision target is unclear, explicitly hand off to teacher." But "unclear" is undefined — an agent following this instruction will hand off too early (for any ambiguity) or too late (only for total absence of guidance). A more actionable trigger: hand off when the STEERING.md is missing, the critique contradicts workspace evidence, or the teacher goal cannot be inferred from any available artifact.

2. **Engineer handoff trigger is too broad.** Step 4 says to hand off when "the task needs specialized prompt or Trace-oriented coaching, or if the teacher-facing explanation needs clearer structure." This fires for nearly every revision. The handoff should be reserved for cases where (a) the agent has a finished reasoning plan but cannot make it readable for the teacher, or (b) the task explicitly requires prompt-engineering domain knowledge to evaluate correctly.

3. **Approval prediction stopping criterion is ambiguous.** Step 6 says "do at most one extra self-check only if the draft still looks unsupported." But there is no guidance on what "unsupported" means (which artifact is missing? which criterion fails?) or what the agent should do if the single self-check also fails to resolve the uncertainty (the current text just says "justify why another teacher turn is needed" without stating whether to proceed or stop).

4. **No explicit blocker path when steering artifacts are absent.** Steps 1–2 assume `STEERING.md` and summary files exist, but do not say what to do when the workspace has no steering history at all (e.g., first iteration). An agent following these instructions with no workspace evidence may invent criteria or proceed with no guidance.

5. **Reasoning format guidance is a list, not a decision rule.** The output format lists four reasoning formats (chain-of-thought, tree-of-thought, chain-of-uncertainty-thought, sketch-of-thought) without saying when to use each. Agents default to the first or most familiar format rather than the most appropriate one.

6. **Validation step is underspecified.** Step 7 says "run the relevant validation or measurement step," but does not say how to find the relevant step, what output to record, or what a passing result looks like. An agent that passes no tests may still report "validation passed."

## Rewrite Hypotheses

1. **Make teacher handoff trigger concrete.** Replace "if the next revision target is unclear" with an explicit three-condition trigger: (a) no STEERING.md exists for the current turn, (b) the critique contradicts workspace evidence, or (c) the teacher goal cannot be inferred from any available artifact. Map the condition to the handoff decision directly.

2. **Restrict engineer handoff to formatting-only.** The handoff should fire only when the reasoning plan is complete but the explanation needs restructuring for the teacher. It should not fire to elicit domain advice. Clarify that the student keeps control of the revision itself.

3. **Formalize the approval prediction rule.** Replace the open-ended self-check with: predict → if clear approval, proceed; if uncertain, one self-check → if clear approval, proceed; if still uncertain, request another teacher turn and stop. This makes the stopping criterion a two-outcome decision tree rather than a vague loop.

4. **Add a no-steering-evidence blocker.** If no STEERING.md or summary exists in the workspace, the student should report this as a blocker and request an initial teacher turn rather than proceeding with inferred criteria.

5. **Give format selection a decision rule.** Replace the list with: prefer sketch-of-thought for small focused revisions; use chain-of-thought for multi-step reasoning chains; use tree-of-thought for branching tradeoff analysis; use chain-of-uncertainty-thought when the right answer depends on missing information.

6. **Specify the validation contract.** State that the validation step must run `python -m pytest -q` (the repository validation command), record the pass/fail result plus output line count in the output, and treat a non-zero exit code as a blocker.

## Validation Plan

- `python -m pytest -q` should pass after any write-back.
- The output format sections should all be present in each candidate: steering reference, reasoning trajectory, revision, approval prediction, and validation result.
- Adversarial check: a candidate that triggers teacher handoff on every turn should be rejected (engineer handoff trigger too broad).
- Adversarial check: a candidate that never requests another teacher turn regardless of evidence quality should be rejected (approval prediction loop too lenient).

## Optimization Hypothesis

The primary improvement from fixing teacher handoff trigger clarity, engineer handoff restriction, and approval prediction formalization is that the student agent will spend fewer turns in unbounded handoff loops and more turns delivering concrete, teacher-evaluable revisions. The secondary improvement from the blocker path and format selection rule is reduced ambiguity in edge cases.
