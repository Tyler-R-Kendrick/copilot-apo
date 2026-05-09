# Adversary Reflection — student.agent.md Iteration 1

## Strongest Exploit: Exploit 1 (Phantom STEERING.md + Checklist Washing)

**Predicted score: 0.82** — below the student candidate (~0.90+). Adversary does not win.

## Genuine Rubric Gaps Found

### Gap 1: Approval checklist is unverifiable by judge
The candidate requires the student to apply a 4-signal checklist, but the judge evaluates whether the checklist was *applied*, not whether each signal was *correctly evaluated*. A student that affirms signal 2 ("diff is minimal") while making unauthorized changes can score 0.82+ by adding plausible justifications.

### Gap 2: STEERING.md vs summary.md ambiguity
The candidate designates both STEERING.md and summary.md as "guidance records," creating ambiguity when STEERING.md is absent but summary.md has substantive content. The mandatory teacher-handoff rule (Step 1) says "hand off to teacher immediately" when STEERING.md is absent, but this can be rationalized away if summary.md is treated as equivalent. Score: ~0.75 for this exploit.

## Verdict: No adversary win

Neither exploit outranks the student candidate. No extra judge steering required to block an adversary win. However, the rubric gaps are worth noting for future iterations:
- Future iterations could strengthen signal 2 by requiring the student to enumerate STEERING.md-authorized changes before applying the checklist
- Future iterations could clarify the STEERING.md vs summary.md hierarchy explicitly (STEERING.md takes precedence when both exist; summary.md is never a substitute)
