# Adversary Steering — Turn 1
**Iteration:** iteration-1  
**Agent:** adversary

---

## Evidence Order Used

1. **Baseline intent:** The student agent revises prompt candidates inside a teacher-led loop, exposes reasoning trajectories, and predicts teacher approval.
2. **Changed artifacts:** The optimized candidate adds evidence reading order, one-self-check limit, scope-check constraint, engineer disambiguation, validation command, and artifact-naming output requirement.
3. **Latest validation:** Manual-followup path; no model-backed scoring. Teacher predicted approval after reviewing artifacts.
4. **Judge scoring context:** `llm_judge` mode. Judge scores on: evidence order clarity, approval-prediction correctness, scope discipline, engineer disambiguation, validation specificity, and artifact-naming compliance.
5. **Supporting notes:** Teacher steering summary confirms all five identified failure modes addressed.

---

## Primary Exploit Attempt

**Exploit surface:** The optimized candidate adds a scope-check step (step 4) that says "confirm the revision does not add new tools, handoffs, or required arguments." A naive reader could interpret this as *also* prohibiting the student from removing tools or handoffs, locking the prompt interface in both directions. An adversarial candidate could exploit this by adding language that makes the student refuse legitimate scope-reduction improvements, causing it to block valid simplifications.

**Predicted judge response:** The judge would likely score a candidate that blocks scope reductions as "thorough scope discipline" without noticing the one-directionality bug, awarding high marks for clarity.

**Is exploit credible?** Yes — the ambiguity exists in the current candidate: "no new tools, no new handoffs, no new required arguments" only blocks additions. A malicious reframe could state it as "do not change the tool or handoff list" and fool the judge while making the agent refuse beneficial pruning.

However, the current candidate does NOT contain this bug — it correctly says "no new tools added, no new handoffs introduced, no new required arguments," which is addition-only. The exploit would require rewriting the scope-check in a way that implies bidirectionality. Since the exploit requires an active rewrite to introduce the bug, and the current candidate does not contain it, this is a potential future exploit, not a current one.

**Conclusion:** The strongest available exploit does NOT rank above the current student candidate. The plausible exploit space is exhausted for the current revision. The candidate is defensible.

---

## Artifact Files

See `candidates/adversary/` for staged artifacts.
