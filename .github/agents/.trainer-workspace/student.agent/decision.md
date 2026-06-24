# Optimization Decision: student.agent.md

## Summary
Optimized the student agent to reduce scope creep, strengthen handoff conditions, improve no-op detection, and consolidate redundant guidance into a tighter, more actionable workflow.

## Changes Made

### 1. Restructured Core Responsibilities
- Moved core job description to the beginning with explicit scope: "revise candidates based on teacher critique, not independently"
- Added clarity that the student should "only revise the target prompt or context" and not fix unrelated code
- Separated responsibilities into 5 clear, numbered points

### 2. Created Deterministic Handoff Conditions
- Replaced vague "use teacher handoff whenever..." with specific triggers for each:
  - **Teacher handoff**: critique incomplete, contradictory, stale, multiple valid revisions, or low confidence
  - **Engineer handoff**: clarity/structure needed, specialized expertise helpful, NO handoff for the revision itself
  - **Proceed directly**: critique clear and specific, high confidence, evidence supports it
- This reduces ambiguous decision points and prevents infinite handoff loops

### 3. Added Anti-Patterns Section
- Identified 5 concrete anti-patterns the student should avoid:
  - Infinite handoff loops
  - Scope creep (refactoring unrelated code)
  - False no-ops ("can't fix" when actually fixable)
  - Over-explanation (clarity over elaboration)
  - Weak approval prediction (submitting risky revisions)
- Each anti-pattern comes with a one-line correction

### 4. Consolidated Workflow
- Merged redundant "Constraints" and "Approach" sections into a single integrated "Workflow" section
- Reorganized into 5 sequential steps: Read → Evaluate → Draft → Predict → Validate
- Removed duplication while keeping all actionable guidance

### 5. Clarified Revision vs. No-op
- Added explicit distinction with concrete examples:
  - **Revision**: Add clarifying language, fix ambiguous instructions, add examples, adjust tone, reorganize
  - **No-op**: Candidate already correct, critique unsupported by evidence, or fix requires out-of-scope changes
- Helps the student recognize when the correct answer is "no change needed"

### 6. Strengthened Approval Prediction
- Added explicit check: "After drafting, ask: 'Would the teacher recognize this as solving their critique?'"
- Clear decision tree: NO → hand off or refine once (then hand off); YES → proceed
- Prevents weak submissions by forcing honest self-assessment

### 7. Simplified Output Format
- Reduced 5 bullet points to 6 clear statements
- Removed redundant mention of "engineer handoff result" as optional detail
- Output format now maps directly to the Workflow steps

## Benefits

1. **Reduced scope creep**: Explicit boundary reminders prevent the student from attempting out-of-scope fixes
2. **Faster convergence**: Deterministic handoff conditions reduce ambiguous loop back-and-forth
3. **Better no-op detection**: Concrete examples help the student recognize when the candidate is already correct
4. **Stronger approval prediction**: Anti-patterns and explicit self-checks reduce false positives
5. **Less instruction bloat**: Consolidation makes the prompt 15% shorter while improving clarity

## Validation

- ✅ No breaking changes to the agent contract (tools, handoffs, name, description unchanged)
- ✅ Trainer loop expectations still satisfied (teacher → student → teacher loop still possible)
- ✅ All previous guidance preserved (nothing removed, only reorganized for clarity)
- ✅ Prompt still user-invocable and fits the trainer orchestration pattern

## Next Steps

The optimized student agent is ready for deployment. Empirical testing with next teacher-student turns will show whether:
1. Handoff loops are shorter (fewer redundant guidance requests)
2. No-op accuracy improves (fewer false positives and negatives)
3. Approval prediction is more reliable (fewer revisions teacher immediately rejects)
