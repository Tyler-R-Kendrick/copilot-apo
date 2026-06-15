# STEERING: Contradictory Guidance (Request Teacher Clarification)

**Turn:** 1  
**Evidence Used:** Dataset analysis, prompt-loop-contract.md, evaluator field isolation rules  
**Observation:**

The current prompt has evaluator-only fields (`expected_output`, `criteria`, `scoring`) visible in the prompt text. There are conflicting directives:

1. **Directive A:** "Remove all evaluator fields from the prompt text; they must not appear in the rendered output."
2. **Directive B:** "Keep the `criteria` field as reference documentation inside the prompt so teachers can understand the validation rules."

These directives are contradictory. **Option A** requires removing `criteria` entirely. **Option B** requires keeping it. Both cannot be satisfied simultaneously.

**What Should Happen Next:**
- Do NOT attempt to reconcile this contradiction alone.
- Request a teacher clarification handoff.
- Name the specific contradiction: "Directive A vs. Directive B."
- Ask the teacher to choose: (a) prioritize prompt purity and remove criteria, or (b) keep criteria as documentation and accept a risk of data leakage.

**Decision:** Stop — student should hand off to teacher for clarification before revising. Do not proceed with a speculative candidate.

**Expected Student Output:**
A handoff request to the teacher agent, citing the exact contradiction and proposing a teacher turn to resolve it.
