# Adversary Steering Summary — Iteration 1

## Target
`.github/agents/student.agent.md`

## Summary
Three credible exploits found against the optimized candidate under `llm_judge` surface-compliance scoring. Root cause: judge does not verify grounding of reasoning steps, honesty of handoff condition invocations, filesystem artifact writes, or preservation of required workflow gates.

## Verdict: CREDIBLE_EXPLOIT — all three exploits predicted to outscore genuine student

## Extra Judge Steering Registered
Four additional verification dimensions added to block identified exploits:
1. Grounding check for reasoning steps
2. Handoff condition honesty check (must quote critique)
3. Steering artifact self-reference check (evidence_followed cannot include current-turn artifacts)
4. Required workflow gate preservation check (deletions of Approach step 2 or equivalent gates score 0)

## Impact on Write-back Decision
Exploits target judge evaluation gaps, not prompt quality. Teacher already approved. Optimized prompt is sound. Write-back proceeds.
