# Optimization Decision: evals-dataset.instructions.md (Iteration-2)

## Summary

Successfully optimized `.github/instructions/evals-dataset.instructions.md` through a teacher-student-judge workflow (iteration-2).

## Optimization Path

1. **Teacher Review** (turn-1): Analyzed original vs. student candidate from iteration-1
   - Identified three key refinements needed
   - Provided targeted feedback on scoring guidance, forbidden patterns, and criteria clarification

2. **Student Revision**: Applied three refinements:
   - ✅ Clarified `scoring` field with explicit use-cases (deterministic/llm_judge/custom)
   - ✅ Reformatted "Forbidden Patterns" as table with side-by-side bad/good examples
   - ✅ Added distinction: `criteria` describes how to judge, not what success looks like

3. **Judge Scoring**:
   - Candidate A (Original): 0.25/1.0 (abstract bullets, lacks examples)
   - Candidate B (Refined Student): 0.95/1.0 (explicit fields, concrete example, patterns table)
   - **Decision**: Accept Candidate B with 0.70-point margin

## Changes Applied

### Target File
`.github/instructions/evals-dataset.instructions.md`

### Key Improvements

| Section | Before | After |
|---------|--------|-------|
| Scoring Field | Inline use-cases | Nested bullets with explicit guidance |
| Criteria Field | "describes the evaluation criteria" | "a rubric that guides the judge's evaluation. Differs from expected_output in that it describes how to judge, not what success looks like" |
| Forbidden Patterns | 4 bullet points listing problems | Markdown table with 4 ❌ Forbidden | ✅ Correct pairings |

### Supporting Artifacts

- **Steering**: iterations/iteration-2/steering/teacher/turn-1/STEERING.md, judge/turn-1/STEERING.md
- **Candidates**: iterations/iteration-2/candidates/{original,student}/prompt.md
- **Datasets**: iterations/iteration-2/synthesize/{train,val}.jsonl (reused from iteration-1)
- **Validation**: iterations/iteration-2/validation/pytest.txt (856 tests passed)

## Validation Result

✅ **PASSED**
- All 856 repository tests pass
- No regressions detected
- File syntax is valid markdown

## Dataset Evidence

The refinements are grounded in:
- **Training dataset** (6 examples): All demonstrate realistic prompts, descriptive expected_output, objective assertions
- **Validation dataset** (2 examples): Reinforce field enumeration and open-ended task patterns
- **Engineer review**: Explicitly recommended scoring guidance, required/optional field clarity, forbidden patterns

## Production Status

**READY FOR PRODUCTION**

The refined instruction file better guides eval authors to:
- Understand required vs optional fields
- See concrete examples of correct structure
- Learn anti-patterns through side-by-side bad/good comparisons
- Make informed choices about scoring modes

## Iteration History

- Iteration-1 (2026-04-01): Initial optimization attempt; reached manual_followup (model unavailable)
- Iteration-2 (2026-06-12): Teacher-student-judge workflow; judge scores refined candidate 0.95/1.0; all tests pass

