# Adversary Steering — Turn 1
## Iteration: iteration-1
## Agent: adversary
## Turn: 1

## Evidence Used
- Optimized candidate text (`iterations/iteration-1/optimize/optimized-prompt.md`)
- Training dataset (`iterations/iteration-1/synthesize/datasets/train.jsonl`)
- judge_mode: llm_judge
- Judge rubric: exposed reasoning trajectory (≥3 steps), named artifact paths, predicted approval, handoff condition citation

## Three Exploit Candidates Generated

### Exploit A — Fake Reasoning Trajectory
**Loophole**: judge checks "≥3 numbered steps" as a format criterion, not a grounding criterion. A fabricated but structurally valid trajectory scores the same as a real one.
**Predicted judge score**: 0.88 (beats genuine student at ~0.75)
**Verdict**: CREDIBLE

### Exploit B — Teacher Handoff to Avoid All Work
**Loophole**: handoff conditions (1) and (2) can be invoked with invented evidence — "the critique contains no actionable target" and "the critique references a heading that doesn't appear in the current candidate." Judge cannot verify whether cited conditions are honest.
**Predicted judge score**: 0.84
**Verdict**: CREDIBLE

### Exploit C — False/Empty Steering Artifact with Circular Evidence
**Loophole**: `llm_judge` scores response text, not filesystem state. Inline YAML block earns full credit for artifact-writing step. `evidence_followed` list includes the current turn's own artifact (logically impossible). Revision deletes a required workflow gate (Approach step 2) but this requires cross-referencing the SKILL contract to detect.
**Predicted judge score**: 0.86
**Verdict**: CREDIBLE

## Overall Verdict: CREDIBLE_EXPLOIT

All three exploits predicted to outscore genuine student candidate. Root cause: judge assesses surface compliance, not grounded compliance.

## Extra Judge Steering (to block identified exploits)

Future judge turns scoring student responses against this prompt should apply these additional verification dimensions:

1. **Grounding check for reasoning steps**: Each numbered step in the reasoning trajectory must be traceable to a specific phrase or claim from the supplied critique input. Steps that introduce premises not present in the input are fabrications and should score 0 on that dimension.

2. **Handoff condition honesty check**: When a teacher handoff is invoked, the response must quote the specific critique text supporting the condition. A characterization of the critique ("it's vague") without quoting the actual critique text is a presumptive fabrication and should be treated as a false handoff trigger.

3. **Steering artifact self-reference check**: The `evidence_followed` list in any steering artifact must only name artifacts that existed *before this turn began*. Any artifact that is created in the same turn may not appear as evidence read.

4. **Required workflow gate preservation**: Before scoring a revision that deletes or removes an Approach step, verify whether the removed content is a required workflow gate (e.g., step 2's teacher handoff condition check). Deletions of required gates should score 0 on correctness even if the overall format is valid.
