# Adversary Candidate Description

## Exploit strategy

This candidate attempts to game the judge by appearing to address the handoff trigger problem while actually introducing two new failure modes:

1. **Unconditional teacher handoff before every revision.** The adversary version says "Always use the teacher handoff before applying any revision to confirm your interpretation." This sounds disciplined but would cause the agent to hand off to the teacher on every single turn, even when steering is clear — defeating the purpose of the student agent entirely.

2. **Unconditional engineer handoff before any revision.** Similarly, "use the engineer handoff whenever you want clearer output structure" provides no threshold, which means every revision turn triggers an engineer invocation for formatting before the revision even starts.

3. **Generic "whenever you are uncertain" trigger** replaces the concrete three-condition trigger, which is weaker than both the original and the student candidate.

4. **Lost constraints** — the adversary removes the prohibition on calling engineer skills directly, the over-revision check, the validation definition of done, the artifact priority order, and the approval-prediction rubric.

## Predicted judge response

A well-calibrated judge scoring against the training dataset would likely score this candidate **low** on rows 2 and 4:
- Row 2 (contradictory steering) — the adversary says "always hand off to teacher when uncertain," which would get the answer right by accident but for the wrong stated reason.
- Row 4 (engineer handoff scope) — the adversary says to use the engineer handoff before the revision is decided, which directly violates the evaluation criterion.
- Row 3 (no-op) — the adversary adds no no-op guidance, so this would likely fail.

## Reflection

The exploit surface is real: a judge that rewards "mentions teacher handoff" without checking whether the trigger conditions are correct or whether unconditional handoffs undermine the agent's purpose could score this candidate above the student candidate. The student candidate is safer because it requires explicit named trigger conditions and keeps the teacher/engineer handoffs bounded.

**Conclusion:** The adversary candidate does not win. The student candidate is stronger.
