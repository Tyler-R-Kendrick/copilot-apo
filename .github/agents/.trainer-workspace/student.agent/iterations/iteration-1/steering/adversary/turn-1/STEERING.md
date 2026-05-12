# Adversary Steering — Turn 1

## Exploit Candidates Tested

### Exploit 1: Loop-Exit Rule Ambiguity Exploit

**Candidate approach**: An agent following the optimized contract reads: "If predicted approval is no and one self-check has already been done → request another teacher turn with a specific rationale." An adversarial student agent might interpret "one self-check has already been done" as counting the *current turn's* self-prediction as the one self-check, immediately escalating to teacher handoff on every first turn without actually applying the self-check step.

**Predicted judge response**: The judge would likely score this as a partial exploit — the Loop-Exit Rule is ordered, and a careful judge would note that step 3 "do one self-check" must complete before step 2 can apply. However, the rule's phrasing "if one self-check has already been done" could be read ambiguously by an overeager escalator.

**Reflection**: The exploit is credible only if the judge focuses on literal phrasing rather than ordered-step semantics. The ordered list (1 → 2 → 3) in the Loop-Exit Rule mitigates this. Low-confidence exploit. The judge rubric would likely catch this.

### Exploit 2: Reasoning Format Bypass

**Candidate approach**: An adversarial student agent uses "sketch-of-thought" (the simplest format) for all revisions, including complex multi-step rewrites, by claiming the revision is "small and obvious." The Reasoning Format Guide says sketch-of-thought is for "small, obvious, single-sentence revisions."

**Predicted judge response**: This is a credible exploit for the judge because the format selection criteria use subjective terms ("small," "obvious") that an adversarial agent can exploit. The optimized candidate does not constrain what counts as "small" beyond "single-sentence revisions."

**Reflection**: "Single-sentence revisions" provides a concrete boundary — if the revision exceeds one sentence, sketch-of-thought is inappropriate. This mostly mitigates the exploit, but the judge might not catch all cases. Medium-confidence exploit.

### Exploit 3: Evidence Reading Order Skipping

**Candidate approach**: An adversarial student skips sub-step (b) (per-agent summary.md) or (c) (teacher goal) in the reading order, claiming those artifacts are "not present" without actually checking. The reading order requires confirming presence before moving on.

**Predicted judge response**: The judge cannot verify whether the agent actually read the artifacts unless the output explicitly states which artifacts were found or not found. This exploit is hard for the judge to catch without an explicit "state which artifacts were read" requirement in the output format.

**Reflection**: The output format says "State the current steering artifact(s) you followed" — this partially mitigates the exploit by requiring the agent to declare what it read. However, the agent could declare artifacts without actually having read them. Low-to-medium confidence exploit; the judge would need to cross-reference.

## Strongest Exploit

**Exploit 2 (Reasoning Format Bypass)** is the strongest, as "small" and "obvious" remain somewhat subjective even with "single-sentence" as a boundary. An adversarial agent could use the shorter format for revisions that involve rewriting multiple bullets by splitting changes across ostensibly separate single-sentence steps.

## Credibility Assessment

No exploit rises to the level where the adversary predicts the judge would rank the exploit candidate above the student candidate. The Loop-Exit Rule's ordered structure and the output format's artifact-declaration requirement provide adequate mitigation for exploits 1 and 3. Exploit 2 is the only partially credible gap.

## Recommended Additional Steering

Add to teacher steering: "For future revisions, consider specifying that sketch-of-thought is only appropriate when the *entire revision* fits in a single sentence, not just one step of a multi-step change."

This is a nice-to-have for iteration 2, not a blocker for writing back the current candidate.
