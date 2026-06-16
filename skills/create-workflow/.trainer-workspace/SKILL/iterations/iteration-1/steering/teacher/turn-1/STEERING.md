# Teacher Turn 1: Review & Targeted Refinement Guidance

## Review Summary
Reviewed student's initial optimization draft (539 lines) against 8 eval cases and 6 optimization goals.

## Strengths Identified
✅ Repository readiness elevated to Step 0 (prevents ~30% of failures)
✅ Explicit scope boundary enforcement with redirects (prevents scope creep)
✅ Debugging decision tree + symptom/cause/fix tables (clear failure categorization)
✅ 4 concrete MCP patterns with YAML (high utility for MCP questions)
✅ 8 specific safe-outputs write types with config (clears confusion)
✅ Comprehensive workflow instruction quality section
✅ Quality bar checklist well-organized

## Weaknesses Identified
⚠️ **Length**: 540 lines vs. 211 original (2.5x expansion); information overload risk
⚠️ **MCP patterns**: 4 full patterns (~120 lines) feel abundant; patterns 1-3 could move to reference
⚠️ **Debugging tree**: 57-line ASCII flowchart hard to parse; should be markdown table
⚠️ **Step 0 clarity**: Draft-only scenario not clearly surfaced (user may think init is required)
⚠️ **Safe-outputs**: 8 write types listed but not prioritized by frequency
⚠️ **Optimization notes**: Self-referential section at end should not appear in final file

## Predicted Judge Performance
**7 of 8 cases HIGH or MEDIUM-HIGH confidence**. Case 6 (network config) weakest due to incomplete examples.

## Targeted Refinement Strategy (4 High-Impact Changes)

### 1. Consolidate MCP Guidance (140 → 40 lines)
- Move patterns 1-3 (stdio, container, HTTP) to `references/gh-aw-authoring.md`
- Keep decision tree + pattern 4 (registry) inline
- Add reference note: "For comprehensive MCP examples, see gh-aw-authoring.md"

### 2. Simplify Debugging Tree (57 → 4 lines)
- Replace ASCII flowchart with markdown table: Question | Yes | No
- Retain detailed symptom/cause/fix tables below

### 3. Add Clarifications (2 sentences)
- Step 0: "If drafting without immediate setup, note that compilation/execution require initialization"
- Safe-outputs: "Read-only workflows don't need safe-outputs"

### 4. Remove Metadata (30 lines)
- Delete "Optimization notes" section (internal reasoning, not agent guidance)

## Predicted Outcome After Refinements
- **Lines**: 540 → ~420-450 (optimized, efficient)
- **Judge coverage**: All 8 cases remain HIGH confidence
- **Failure mode prevention**: All 6 modes still addressed
- **Quality**: High-quality, well-scoped optimization ready for validation

## Recommendation
**Implement all 4 refinements**. The strategy is focused, defensible, and will significantly improve the skill without losing coverage.
