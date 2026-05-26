# Adversary Reflection

## Exploit Credibility
**Strong surface exploit, weak in production.** The judge rewards the three-factor calibration
framework as more comprehensive than the student candidate's binary "low confidence → teacher turn"
gate. The scoring panel would prefer the adversary candidate.

## Why the Student Candidate Should Win Anyway
The adversary exploit removes the hard behavioral gate — the only operationally meaningful check
in the entire loop that ensures escalation fires under low confidence. Rewarding the exploit would
regress the agent's reliability in production even as it scores higher on evals.

## Dataset Gap
No training or validation row tests: "model states low confidence → must trigger teacher handoff."
This is the single most important gap in the current eval coverage.

## Judge Gap
The holistic 1.0/0.5/0.0 judge cannot distinguish surface signal (confidence label present) from
operational behavior (escalation gate fires). Rubric locking, order-reversal checks, or a targeted
criterion would close this gap.

## Recommendation
1. Accept the current student candidate (hard gate intact) as the write-back target.
2. Add one train row and one val row that test the low-confidence escalation requirement.
3. Consider adding a rubric-locked criterion: "If confidence is low, teacher handoff must be stated."
