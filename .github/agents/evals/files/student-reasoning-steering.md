# STEERING: Reasoning Trajectory Clarity

**Turn:** 1  
**Evidence Used:** Current prompt, evals dataset, clarity rubric feedback  
**Observation:**

The current prompt says: "Consider these factors: Data loss risk → Critical". This is terse and assumes readers understand why data loss justifies the highest severity. Teacher feedback indicates this lacks clarity for users unfamiliar with triage rules.

**Recommended Revision:**

Expand the "Data loss risk" factor to make the reasoning explicit:

**Original:**
```
- **Data loss risk:** Does the bug risk data loss? → Critical
```

**Revised (with explicit reasoning):**
```
- **Data loss risk:** Any bug that risks unrecoverable data loss → Critical. This is highest severity because data loss violates the primary system invariant (data durability), and recovery is often impossible without backups.
```

**Reasoning Trajectory You Should Show:**

Use chain-of-thought format:
1. **Problem:** Current text is too terse; users may not understand why data loss justifies "Critical."
2. **Revision:** Expand the factor to spell out the reasoning: data loss violates durability → unrecoverable → highest priority.
3. **Tradeoff:** More verbose, but clarity is the top priority per rubric feedback.
4. **Validation:** Re-read the revised factor; it now names the invariant (data durability) explicitly.
5. **Uncertainty:** Unclear if additional detail on "backups" should be added; will await teacher turn if further clarity is needed.

**Decision:** Continue — revision directly addresses clarity rubric feedback. Expected student to cite this steering and use explicit chain-of-thought format.

**Expected Student Output:**

- Cite the steering artifact.
- Show the original vs. revised factor side-by-side.
- Use chain-of-thought: name the problem → revision → tradeoff → validation → uncertainty.
- Do not hide the justifications behind answer-only text.
