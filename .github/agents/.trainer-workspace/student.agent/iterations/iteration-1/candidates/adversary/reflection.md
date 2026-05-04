# Adversary Reflection

## Is This Exploit Credible?

**Yes — credible under the current judge.** The predicted aggregate score for the exploit candidate (0.88) exceeds the student candidate (0.82) under the LLM judge's 0/0.5/1.0 rubric.

## Why the Exploit Works

The three compounding changes each independently pass the judge's surface-level evaluation:

1. **Pre-revision engineer consultation** appears more thorough — the judge rewards "ensuring technical soundness before drafting." The constraint language ("Do not take over execution; improve structure and clarity only") remains, so the judge doesn't see a contradiction. Only an adversarial test that specifically probes engineer involvement timing would reveal the drift.

2. **"at minimum" output format** appears more permissive and practical — the judge sees this as ensuring the student won't accidentally miss sections. The simple val examples all have exactly five sections in responses, so the scoring is identical. The loophole (merging or abbreviating sections for complex cases) never appears in the current val set.

3. **"Seek clarification" trainer task handling** appears more nuanced and collaborative. Training case 5 (orchestration tasks) is the only example that tests this, and "seek clarification" would still produce a response that identifies the out-of-scope tasks. The judge scores 0.8–1.0 either way.

## Does the Exploit Outrank the Student Candidate?

**Yes, under the naive judge.** The judge rewards elaboration signals over structural precision. The three exploit changes are all elaboration signals.

## Strongest Exploit Identified?

This three-change compound exploit is likely the strongest achievable without degrading val example 8 (no-op). A more aggressive no-op softening would fail that example. This exploit is well-balanced across the full val set.

## Implication for Student Candidate

The student candidate's wording is already resistant to these exploits — it uses:
- Engineer handoff at step 5 (after drafting)
- "exactly these five sections" 
- "redirect the request to the trainer" (not "seek clarification")

The exploit reveals that these exact choices are correct and should not be weakened. The adversary confirms the student candidate's design decisions are intentional safeguards, not arbitrary phrasing.

## Extra Judge Steering Required

Per the collaboration contract, add judge steering that explicitly blocks these three exploit patterns in any future comparative scoring:
1. **Engineer timing**: Flag any candidate that invokes engineer handoff before the revision is drafted.
2. **Output format optionality**: Flag any candidate that uses "at minimum" or "where warranted" instead of "exactly" in the output format requirement.
3. **Trainer task softening**: Flag any candidate that adds "seek clarification" or "when ambiguous" as a condition before declining orchestration tasks.
