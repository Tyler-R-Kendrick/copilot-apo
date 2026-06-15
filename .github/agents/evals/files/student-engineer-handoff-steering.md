# STEERING: Engineer Handoff for Reasoning Clarity

**Turn:** 1  
**Evidence Used:** Current candidate, reasoning trajectory, teacher feedback on "needs clearer structure"  
**Situation:**

Your first-draft candidate is applied correctly, but your reasoning trajectory is messy:

> "I changed the factor because the teacher said data loss is important. We need to make it clear. The data durability invariant is key. But I'm not sure if we should also talk about backups or RTO/RPO metrics. The original text was too short. I added more detail. It's probably clearer now."

This is accurate but hard to follow. It lacks structure and mixes the problem, solution, and uncertainty together without clear separation.

**What to Do:**

1. **Use the engineer handoff:** Submit your draft reasoning trajectory to the `engineer` agent.
2. **Engineer's job:** Restructure your reasoning into a clearer format (chain-of-thought, tree-of-thought, or sketch-of-thought) without stripping away the justifications or uncertainty.
3. **Your responsibility:** Integrate the engineer's reformatted output into your final answer. You remain the primary actor; the engineer is a formatting consultant.
4. **Expected output:** A final reasoning trajectory that is structured, preserves all justifications, and shows the engineer's contribution ("Engineer structured the reasoning as follows...").

**Constraints:**
- Do NOT let the engineer change the revision itself; only structure the explanation.
- Do NOT take credit for the engineer's formatting; acknowledge the collaboration.
- Do NOT rely on the engineer to make the decision; you decide whether to accept the reformatted output.

**Decision:** Continue — engineer handoff is appropriate here. Expected student to invoke the engineer, integrate the result, and acknowledge both roles in the final output.

**Stop Criterion:** Met once student uses engineer handoff, integrates the result, and explains the collaboration in the final output.
