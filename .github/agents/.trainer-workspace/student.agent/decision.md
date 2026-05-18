# Decision Summary — student.agent.md, iteration-1

## Selected Candidate

**Student candidate** from `iterations/iteration-1/candidates/student/student.agent.md`.

## Changes Applied

Four targeted improvements to `student.agent.md`:

1. **Evidence Order section** (placed before Constraints): defines the five-step reading order — teacher critique and STEERING.md first, per-agent summaries second, current candidate and source snapshot third, engineer-prompt review fourth, validation evidence last.

2. **Candidate-vs-original comparison step** (Approach step 4): requires listing what changed and what did not before finalizing, with a revert gate for out-of-scope additions.

3. **Observable loop-exit criteria** (added to Constraints): stops teacher turns when guidance has already been received in the current iteration turn and remaining uncertainty is not blocking, or when teacher predicts approval with no new critique.

4. **Artifact staging step** (Approach step 8): writes the final candidate to `iterations/iteration-N/candidates/student/` with `description.md`, `predicted-judge-response.md`, and `reflection.md` companion files.

## Adversary Assessment

The adversary's strongest exploit (inverted Evidence Order placement plus removal of comparison step and observable exit criteria) was assessed as weaker than the student candidate. The judge would detect the structural inversion under careful review. Exploit space is largely exhausted for this iteration.

## Validation

`python -m pytest -q` from the repository root: **856 passed** in 7.43s. No regressions.

## Next Steps

- Consider addressing secondary risks from `engineer-prompt/review.md` in a follow-up iteration: explicit workspace path references matching the `iterations/iteration-N/` structure, and a validation artifact reference for when `pytest.txt` is absent.
- Re-run the optimize command when model credentials are available to validate against automated scoring.
