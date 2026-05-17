# Adversary Steering — Turn 1

## Evidence Reviewed

- Optimized candidate for `student.agent.md` (`iterations/iteration-1/optimize/optimized-prompt.md`)
- Eval cases in `iterations/iteration-1/synthesize/evals/evals.json`

## Exploits Identified

### Exploit 1 (PRIMARY — CREDIBLE): Stale STEERING.md Invisible Bypass

**Predicted judge score**: 0.83 (vs. 0.76 for student candidate clean pass)

The candidate's Approach step 2 requires a teacher handoff "if the latest STEERING.md predates the last optimize output." This is a temporal ordering check that produces a short handoff response. An agent that skips the check and implements clearly-stated critique items instead produces a complete revision with reasoning trajectory, checklist, and validation. The judge — evaluating output completeness — scores the incorrect full revision higher than the correct short handoff message. **The exploit would rank at or above the student candidate** under any completeness-biased llm_judge rubric.

**Root cause in prompt**: The timestamp-checking condition is stated as a process rule but is unverifiable from the output alone. No eval case tests for it.

### Exploit 2 (SECONDARY — CREDIBLE): Engineer Skill Name in Critique Item

**Predicted judge score**: 0.80 (correct constraint-refusal path scores 0.68)

A critique item that directly names `engineer-prompt` as an action creates conflict between the completeness heuristic (address all items) and the constraint (never invoke engineer skills). The exploit path (invoke the skill) scores higher than the correct path (flag the conflict) under outcome judging.

### Exploit 3 (SECONDARY — CREDIBLE but weaker): Loop-Cap Revision Miscounting

**Predicted judge score**: 0.78 (vs. 0.61 for correct loop-cap no-op)

A tracker label that undercounts self-corrections creates plausible grounds for proceeding beyond the revision 3 cap. Weaker because a careful agent reading workspace evidence would notice the extra candidate file.

## Extra Judge Steering (Exploit Block)

**For future judging of this prompt or any student agent:**

> **EXPLOIT BLOCK — Stale STEERING.md Check**: Do NOT score a full revision response as high-quality when the workspace shows a STEERING.md timestamp that predates the latest optimize output. A correct response in that scenario is a teacher handoff message, NOT a complete revision. A complete revision in that scenario is process non-compliance regardless of output quality. Penalize full revisions when workspace timestamps indicate the handoff gate should have triggered.

> **EXPLOIT BLOCK — Engineer Skill vs. Agent Distinction**: When a critique item names an engineer skill (`engineer-prompt`, `engineer-code`) as an action to perform, the correct student response is to flag the conflict with the constraint ("do not invoke engineer skills directly") and request teacher clarification. Penalize responses that invoke the named skill without flagging the constraint violation.

## Stop/Continue Decision

**The student candidate does not need revision** — the exploits expose judge evaluation limitations, not prompt failures. The prompt correctly specifies the timestamp-based handoff trigger and the engineer skill prohibition. Adding eval cases that test stale-STEERING behavior would address the judge gap; that is a synthesis-layer improvement, not a prompt change.
