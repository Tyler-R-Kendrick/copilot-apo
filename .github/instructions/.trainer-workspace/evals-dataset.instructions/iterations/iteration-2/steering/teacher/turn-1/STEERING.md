# Teacher Steering: Iteration-2, Turn-1

## Candidates Reviewed

- **Original** (iteration-1/candidates/original): 6 bullet rules, no examples
- **Student** (iteration-1/candidates/student): Added "Required and Optional Fields", JSON example, "Forbidden Patterns" section
- **Dataset**: 6 training examples, 2 validation examples; all use realistic prompts, descriptive expected_output, objective assertions

## Key Feedback

### Three Refinements Needed (in priority order):

1. **Scoring Field Guidance** (Critical) ✅ Addressed
   - Original: Mentioned `deterministic`, `custom`, `llm_judge` but didn't explain when to use each
   - Refined: Now uses nested bullets with explicit use-cases for each scoring mode
   - Evidence: Training examples show consistent use of `deterministic` for objective checks, `llm_judge` for judgment

2. **Forbidden Patterns → Bad/Good Pairings** (Highest Priority) ✅ Addressed
   - Original: 4 bullet points listing problems without showing fixes
   - Refined: Converted to markdown table with ❌ Bad Form | ✅ Good Form
   - Evidence: Training example 5 explicitly teaches bad → good pattern for prompts

3. **Clarify `criteria` vs. `expected_output`** (Medium) ✅ Addressed
   - Original: Both fields in example but not differentiated
   - Refined: Added explicit distinction—`expected_output` describes success, `criteria` describes how to judge
   - Evidence: Training examples consistently use both when `scoring: llm_judge`

## Predicted Approval

- ✅ All three teacher requirements addressed
- ✅ No scope creep (minimal, focused edits)
- ✅ Table format aligns with training pattern
- ✅ Ready for judge review

## Decision

**APPROVE refined candidate for judge scoring.**
