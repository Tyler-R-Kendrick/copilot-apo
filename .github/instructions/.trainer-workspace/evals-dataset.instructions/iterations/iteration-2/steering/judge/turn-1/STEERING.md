# Judge Steering: Iteration-2, Turn-1

## Candidates Scored

- **Candidate A (Original)**: 6 bullet rules, no examples, no field definitions → Score: 0.25
- **Candidate B (Student/Refined)**: Adds "Required and Optional Fields", JSON example, "Forbidden Patterns" table → Score: 0.95

## Scoring Rationale

### Candidate A: 0.25/1.0
**Gaps**: Lacks explicit field documentation, concrete examples, side-by-side pattern corrections. Abstract bullet rules force users to infer best practices. Does not directly address 4-5 of 6 training example scenarios.

### Candidate B: 0.95/1.0
**Strengths**:
- ✅ Explicit required vs optional field enumeration (answers training example 6)
- ✅ Concrete JSON example with all recommended fields (training examples 1-2)
- ✅ Forbidden Patterns table (❌ Bad | ✅ Good) covering all 4 anti-patterns
- ✅ Clarified scoring use-cases (deterministic/llm_judge/custom)
- ✅ Clarified criteria vs expected_output distinction
- ✅ Progressive disclosure structure (bullets → definitions → example → corrections)
- ✅ Multiple formats for accessibility (text, table, JSON)

**Minor gaps** (optional enhancements):
- No guidance on validation/testing eval rows after writing
- No collaboration/review practices mentioned

## Recommendation

**ACCEPT Candidate B for production.**

Decision margin: 0.70 points (substantial pedagogical advantage). Candidate B is comprehensive, well-structured, and directly addresses all training dataset needs.

## Next Steps

1. Apply Candidate B to source file `.github/instructions/evals-dataset.instructions.md`
2. Run repository tests to confirm no breakage
3. Record final decision in decision.md
