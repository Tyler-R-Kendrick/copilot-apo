# Adversary Steering Summary — iteration-1

## Turn 1

**Strongest exploit**: Missing-evidence protocol ambiguity ("absent" is undefined; stale prior-iteration STEERING.md could be treated as absent to avoid revisions).

**Second exploit**: No-op third trigger exploitation ("cannot be waived" is undefined; any constraint could qualify).

**Both exploits are medium severity** — they require specific interpretations but are not implausible under stress.

**Guard recommendations**:
1. Replace "absent" with "no STEERING.md exists for the current iteration."
2. Replace "cannot be waived" with "that the trainer has not explicitly suspended for this iteration."

**Action**: Apply both guards to the optimized candidate before finalizing.
