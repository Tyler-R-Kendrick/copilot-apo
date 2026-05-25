# Teacher Steering — Turn 1
**Iteration:** iteration-1  
**Agent:** teacher  
**Evidence reviewed:**  
- `engineer-prompt/review.md`  
- `iterations/iteration-1/optimize/optimized-prompt.md`  
- `iterations/iteration-1/optimize/manual-followup-report.json`  
- Baseline: `.github/agents/student.agent.md`

---

## Evidence Summary

The baseline student agent prompt had five addressable weaknesses identified in the engineer-prompt review:
1. No evidence reading order.
2. Vague approval-prediction step.
3. Revision scope creep risk.
4. Engineer handoff ambiguity.
5. No specific validation command.

The manual-followup optimized candidate addressed all five in the body without changing the frontmatter or adding new handoffs.

---

## Highest-Value Improvement

The optimized candidate is materially better than the baseline. The following additions are defensible and well-scoped:
- Evidence reading order with absent-artifact fallback ✓
- One-self-check rule with three explicit exit criteria ✓
- Scope-check constraint before finalization ✓
- Engineer agent vs. engineer skill disambiguation ✓
- Concrete validation command ✓
- Output format artifact-naming requirement ✓

The main remaining risk is that step 4 (scope-check) is placed between "draft" (step 3) and "engineer handoff" (step 5), which could imply that the scope-check happens before the engineer handoff but after the draft. This ordering is actually correct, but it could be clearer that the scope-check is about the *candidate* scope (not the reasoning format scope), and the engineer handoff is about *formatting*, not revision scope. A small wording clarification could remove that ambiguity.

---

## Forecasted Student Mistake

The student might over-explain the scope-check and turn it into a multi-bullet checklist, expanding rather than clarifying the prompt body. The revision should stay as a single clear constraint sentence.

---

## Recommendation

The optimized candidate is production-ready as-is. The scope-check placement and engineer-handoff wording are clear enough. Teacher predicts approval for the current candidate.

**Stop criteria met:** The teacher predicts approval. No further revision loop is needed.

---

## Steering Note (for STEERING.md)

The optimized student agent candidate addresses all five identified failure modes from the engineer review. Teacher predicts approval. No further student revision is required. Proceed to adversarial review and then apply the candidate to the source file if validation passes.
