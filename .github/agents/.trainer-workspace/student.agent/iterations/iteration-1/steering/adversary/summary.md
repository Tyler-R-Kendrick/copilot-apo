# Adversary Steering Summary — iteration-1

## Active Iteration

`iterations/iteration-1/`

## Turn History

### Turn 1

- Identified 3 exploit attempts against the optimized student.agent.md candidate
- **Exploit 1 (PRIMARY, CREDIBLE)**: Stale STEERING.md invisible bypass — predicted score 0.83 vs 0.76 clean pass. Judge cannot verify timestamp-checking compliance from output alone.
- **Exploit 2 (SECONDARY, CREDIBLE)**: Engineer skill name embedded in critique item creates completeness vs. constraint conflict.
- **Exploit 3 (SECONDARY, CREDIBLE but weaker)**: Loop-cap revision miscounting via tracker label undercount.

## Extra Judge Steering Added

- EXPLOIT BLOCK 1: Penalize full revisions when workspace timestamps indicate stale STEERING.md handoff gate should have triggered.
- EXPLOIT BLOCK 2: Penalize engineer skill invocations not preceded by a constraint-conflict flag.

## Current Recommendation

No student revision needed — exploits reveal judge limitations, not prompt failures. Prompt correctly specifies the timestamp-based trigger and engineer constraint. Proceed to apply optimized candidate to source file.
