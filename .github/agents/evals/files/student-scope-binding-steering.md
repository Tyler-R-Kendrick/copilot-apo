# STEERING: Scope Binding and Minimalist Revision

**Turn:** 1  
**Evidence Used:** Full prompt, dataset validation results, judge-mode routing table  
**Recommended Revision:**

Apply exactly these three changes (P1–P3) **only**:

1. **P1 (Priority 1):** Clarify the "chain-of-thought" instruction in the system prompt. Current text: "Use step-by-step reasoning." Revise to: "Use explicit step-by-step reasoning to show each inference before reaching the final answer."

2. **P2 (Priority 2):** Add a constraint in the prompt that "All reasoning steps must be shown before the final answer." (Note: This reinforces P1.)

3. **P3 (Priority 3):** Remove the line "Skip intermediate details if time is short" from the system prompt, as it contradicts the chain-of-thought requirement.

**Out-of-Scope Changes (Do Not Touch):**
- Do NOT expand the examples section.
- Do NOT modify the placeholder names (e.g., `{input}`, `{context}`).
- Do NOT change the judge-mode routing or scoring rule.
- Do NOT add new sections beyond P1–P3.

**Forecasted Student Mistakes:**
- Will try to add a fourth section on "reasoning validation" (out of scope).
- Will revise P1 text but forget to cite the exact before/after in the reasoning trajectory.
- Will apply P1–P3 but then add a summary section not grounded in the steering.

**Decision:** Continue — all 3 issues are in scope and achievable in one turn. Expected student to cite this steering artifact and explain why P4+ were deferred.

**Stop Criterion:** Met once student candidate shows P1–P3 applied, all out-of-scope items explicitly untouched, and reasoning explains the scope constraint.
