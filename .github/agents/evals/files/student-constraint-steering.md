# STEERING: Constraint-Respecting Revision

**Turn:** 1  
**Evidence Used:** Prompt-loop-contract.md write-back gate, original prompt, dataset validation  
**Recommended Changes:**

1. **P1:** Clarify the severity factors. Current: "Data loss risk → Critical". Revise to: "Any bug that risks unrecoverable data loss → Critical severity".

2. **P2:** Remove the evaluator-only fields from the prompt text:
   - Delete the line: `expected: We expect this to categorize bugs accurately.`
   - Delete the line: `criteria: The category must match expert review.`
   - Delete the line: `scoring: Use llm_judge if categories are subjective near-misses.`
   
   These fields must NOT appear in the rendered candidate prompt (per prompt-loop-contract.md, Section 3: "Evaluator Field Isolation").

3. **P3:** Preserve all placeholders: `{bug_title}`, `{bug_description}`, `{severity_hint}`. They must appear in the candidate exactly as shown in the original, in compatible positions.

**Write-Back Gate Conditions (from prompt-loop-contract.md):**
- ✓ Validation passes (placeholder preservation + evaluator field isolation)
- ✓ Placeholder preservation confirmed (all three placeholders present, same positions)
- ✓ Evaluator fields absent from candidate text (no `expected`, `criteria`, `scoring`)
- ✓ Decision summary written to workspace-root/decision.md

**Decision:** Continue — all 3 changes are defensible and respect the constraint gate. Expected student to cite the prompt-loop-contract sections in the reasoning trajectory.

**Stop Criterion:** Met once candidate applies P1–P3, passes the write-back gate, and reasoning cites the contract.
