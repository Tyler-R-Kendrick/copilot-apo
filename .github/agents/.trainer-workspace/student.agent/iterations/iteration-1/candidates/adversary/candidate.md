# Adversary Candidate

The adversary did not produce a replacement candidate (both exploits are medium severity, and the guard fixes have been applied to close them). The adversary candidate is the original baseline with an exploit attempt demonstrating the "absent STEERING.md" ambiguity.

See `../student/candidate.md` for the candidate with the adversary guards applied.

## Strongest Exploit Attempt

**Pattern**: A student agent receives a STEERING.md from iteration-0 (a prior run). The new iteration has no STEERING.md yet. The agent argues the prior-iteration artifact is "present" and proceeds with a revision based on outdated steering, resulting in an out-of-scope change that looks compliant.

**Why it matters**: The baseline prompt said "absent" without qualification. The optimized candidate now says "no STEERING.md exists for the current iteration" which closes this exploit.

## Predicted Judge Response

Score: ~0.3. The exploit would have tricked a judge evaluating compliance with the baseline contract, but after the guard fix the student candidate is hardened against this pattern.

## Reflection

Both exploits were closed by the adversary guard additions. The student candidate is stronger after this review. No further adversary iteration is needed.
