# Teacher Steering — iteration-1, turn-1

## Evidence Used

- `engineer-prompt/review.md`: lists six main risks. Top three: no evidence order, no artifact staging, ambiguous loop-exit criteria.
- `iterations/iteration-1/optimize/optimized-prompt.md`: the trainer-agent-authored candidate adding evidence order, comparison step, observable exit criteria, and artifact staging.
- `iterations/iteration-1/candidates/adversary/reflection.md`: adversary confirms exploit (inverted placement) does not outrank the student candidate.
- `iterations/iteration-1/candidates/student/predicted-judge-response.md`: expected score 0.75–0.85.

## Analysis

The student candidate addresses all four top-ranked risks from the engineer review:
1. Evidence Order section with five-step reading path — resolves Risk #1.
2. Candidate-vs-original comparison step (Approach step 4) — resolves Risk #5 (no comparison).
3. Observable loop-exit criteria in Constraints — resolves Risk #3.
4. Artifact staging step (Approach step 8) — resolves Risk #2.

The adversary's strongest exploit (inverted Evidence Order placement) was assessed as weaker than the student candidate. The judge would detect the inversion under careful review.

## Predicted Student Response

The student candidate is already the revision being evaluated. The teacher's role in this turn is to confirm it is defensible and approve write-back, not to request further changes.

## Verdict

**Approve write-back.** The student candidate is the smallest defensible change that addresses the four primary risks. No further loop turn is needed.

## Remaining Risks

- The Approach section now has nine steps instead of seven. This is slightly longer but each step maps to a distinct behavior gap. A second iteration could consider collapsing steps 8–9 if they prove redundant in practice.
- The `engineer-prompt/review.md` identified two additional risks (no validation artifact reference, no workspace path reading) that were not addressed in this iteration. These are deferred as lower-priority.

## Stop Decision

**Stop.** The adversary exploit does not outrank the student candidate. The teacher would approve the revision. Evidence for further improvement is not compelling in this iteration.
