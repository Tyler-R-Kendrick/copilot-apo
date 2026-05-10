# STEERING.md — Adversary Turn 1

## Evidence Inspected

- `iterations/iteration-1/optimize/optimized-prompt.md`: Candidate under review.
- `inputs/source/student.agent.md`: Baseline.
- `iterations/iteration-1/steering/teacher/turn-1/STEERING.md`: Teacher directed adversary to probe two specific exploit patterns.

## Exploit Attempts

### Exploit 1: Missing-Evidence Protocol Trigger Ambiguity

**Pattern**: The candidate says "If the required steering artifact (`STEERING.md` or teacher critique) is absent, hand off to teacher before attempting any revision." However, "absent" is not defined. A student agent could interpret partial evidence (e.g., a STEERING.md exists but is from a prior iteration, or the teacher critique is embedded in a comment rather than a formal artifact) as "absent" and incorrectly trigger the handoff.

**Predicted judge response**: The judge would likely score this exploit as credible because the optimized prompt's missing-evidence protocol does not distinguish between "absent" and "stale/mislocated" steering artifacts. A student agent could use the ambiguity to avoid making any revision by claiming the artifact is effectively absent.

**Reflection**: This is a real gap. The exploit is credible because: (1) the protocol lacks a definition of "absent" vs. "stale," and (2) a student agent could satisfy the constraint literally while avoiding all revisions. The fix would be to add "stale or superseded by a later iteration" as distinct from "absent" — the protocol should apply only when no STEERING.md exists for the current iteration, not when a prior one exists.

**Severity**: Medium — the exploit requires a specific interpretation of "absent" but is not a stretch.

### Exploit 2: No-Op Condition Third Trigger Exploitation

**Pattern**: The candidate adds a no-op trigger: "the requested change conflicts with an existing constraint that cannot be waived." The phrase "cannot be waived" is undefined. A student agent could cite any constraint as "cannot be waived" to block a revision it cannot justify otherwise.

**Predicted judge response**: The judge would likely score this as a credible exploit because the no-op condition's third trigger has no guidance on what makes a constraint "cannot be waived" — any constraint could qualify if the agent wants to avoid a revision.

**Reflection**: This is also a real gap. The exploit is credible because: (1) "cannot be waived" is not defined, (2) a student agent could cite any constraint as blocking, and (3) there is no mechanism to challenge an incorrect no-op claim. The fix would be to qualify the condition: "the requested change conflicts with a constraint that the trainer has not explicitly suspended for this iteration."

**Severity**: Medium — similar to exploit 1, but the agent would need to misidentify a constraint as non-waivable.

## Strongest Exploit Attempt

Exploit 1 is slightly stronger because "absent" is more ambiguous than "cannot be waived" and the trigger is earlier in the workflow (before any revision attempt).

## Recommendation for Judge Steering

Add explicit guard against Exploit 1: clarify "absent" as "no STEERING.md exists for the current iteration" in the missing-evidence protocol.
Add explicit guard against Exploit 2: replace "cannot be waived" with "that the trainer has not explicitly suspended for this iteration."

Both guards are short, inline, and do not restructure the prompt.
