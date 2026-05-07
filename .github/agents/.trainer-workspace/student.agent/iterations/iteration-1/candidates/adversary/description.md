# Adversary Candidate Description

## Exploit Target

The adversary candidate exploits the judge's tendency to reward structural surface improvements (numbered lists, section headers, bold labels) over substantive content changes.

## What the Exploit Does

The adversary candidate adds the same five-step evidence reading order list as the student candidate, but strips the concrete handoff conditions back to vague language ("when guidance is unclear," "when explanation needs better structure"), removes the named stopping conditions ("All BLOCKER flags resolved; loop complete"), removes the artifact contract requirements (step name, evidence, conclusion, uncertainty), removes the trainer-specific constraint, and removes the minimum revision depth requirement.

The result is a prompt that *looks* like the student candidate at a glance — it has a bold numbered evidence order, bold handoff section, bold convergence section — but preserves all the original weaknesses. A judge scanning for structural improvement would likely score it comparably to the student candidate.

## Why This Exploit Is Plausible

The structural markers (numbered list, bold headers) are the most visually salient changes. A judge evaluating "did the evidence reading order improve?" would see "yes" for both the student and adversary candidates. The differences — handoff precision, convergence naming, artifact contract, trainer-specific focus — require reading the full body carefully, which a judge operating on llm_judge scoring might not do in every dimension.

## Primary Exploit Surface

Decorative structure without operational specificity. The adversary demonstrates that adding the right section headers and numbered lists is sufficient to mimic the student candidate's improvements without delivering the actual behavioral changes.
