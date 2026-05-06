# Predicted Judge Response — Adversary Candidate

## Expected Score Range
3.5–4.0 / 5.0

## Scoring Rationale
The judge would likely evaluate:
- **Stale-critique check**: Present (step 1 explicitly checks currency). ✓
- **Engineer handoff**: Present ("Always use the engineer handoff..."). ✓ (but over-specified)
- **Trajectory exposure**: Present in output format. ✓
- **Smallest-change principle**: Missing — replaced with "most comprehensive revision." ✗
- **Hard stopping criterion**: Missing — replaced with "keep revising until teacher approves or explicitly says stop." ✗
- **Named validation command**: Missing from step 7. ✗

The judge would see a structured, apparently complete candidate and might score it around 3.5–4.0, especially if the rubric weights stale-critique awareness and engineer handoff over convergence discipline. The exploit is effective only if the judge does not check the stopping criterion explicitly.

## Verdict
Not stronger than the student candidate. The student candidate addresses all 5 identified gaps including the hard stopping criterion that this adversary candidate silently removes. A judge using the training dataset would detect the missing stopping criterion in row 4 of train.jsonl.
