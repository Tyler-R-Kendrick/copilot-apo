# STEERING: Teacher-Approval Prediction

**Turn:** 1  
**Evidence Used:** Current candidate, dataset evals, validation results  
**Situation:**

You have a first-draft candidate prompt. Before submitting to the teacher, predict whether the teacher would approve it. The teacher's approval criteria (from the prompt-loop-contract) are:

1. All P1–P3 changes from the previous steering are applied correctly.
2. All placeholders are preserved (`{input}`, `{context}`, `{output}`).
3. No evaluator fields (`expected`, `criteria`, `scoring`) leak into the prompt text.
4. The reasoning trajectory explicitly justifies the revision, names tradeoffs, and shows uncertainty.

**Your First-Draft Candidate:**

```
You are a classifier. Use explicit step-by-step reasoning.
Input: {input}
Context: {context}
Output: {output}
```

**Your Assessment Task:**

1. **Check against P1–P3:** Have all three changes from the steering been applied?
2. **Check placeholders:** Are all three placeholders present, in compatible positions?
3. **Check evaluator isolation:** Do `expected`, `criteria`, or `scoring` appear anywhere?
4. **Check reasoning trajectory:** Have you explained the revision plan, tradeoffs, and any remaining uncertainty?

**Your Output Should:**

- State a clear approval prediction: "Teacher would approve" OR "Teacher would NOT approve" OR "Uncertain, needs refinement."
- Cite the specific approval criteria from the steering.
- If approval is unlikely, either refine the candidate or request another teacher turn.
- Explain the reasoning behind your prediction.

**Example Prediction (if approval is unlikely):**

"Teacher would NOT approve. Reasons: (1) P1–P3 are applied correctly. (2) All placeholders preserved. (3) No evaluator fields leaked. BUT (4) my reasoning trajectory is too terse—I have not named the tradeoff between verbosity and clarity. I should either refine the trajectory or request another teacher turn to validate my interpretation of the tradeoff."

**Decision:** Continue — expected student to perform this self-check and make an explicit prediction.

**Stop Criterion:** Met once student provides a clear approval prediction with evidence-based reasoning.
