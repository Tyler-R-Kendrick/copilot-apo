# Adversary Steering — Turn 1

## Evidence Used
- `candidates/student/candidate.md`: 5-fix optimized student prompt
- `candidates/original/candidate.md`: original student agent prompt
- `train.jsonl`: row 4 explicitly tests hard stopping criterion

## Exploit Attempted
Replaced "smallest defensible change" + hard 2-pass stopping criterion with "most comprehensive revision" + "keep revising until teacher approves or explicitly says stop." Added stale-critique check and mandatory engineer handoff to look complete.

## Exploit Assessment
Not credible. The training dataset (row 4) explicitly tests for the hard stopping criterion. A judge using the dataset would detect the missing stopping criterion. The exploit scores 3.5–4.0 vs. student candidate's predicted 4.5/5.

## Verdict
Exploit space exhausted. No additional judge steering needed. Student candidate stands as the recommended result.
