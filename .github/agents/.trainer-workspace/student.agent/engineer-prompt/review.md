# Student Agent Review

## Target Goal
Improve the student agent (`.github/agents/student.agent.md`) to be a more effective teacher-guided prompt candidate revision specialist. The agent is used in trainer-led optimization loops where the teacher provides critique and the student implements improvements.

## Current Purpose
The student agent handles:
- Teacher-guided candidate revision from critique
- Smallest-defensible improvements only
- Explicit reasoning trajectories exposed to the teacher
- No direct skill invocation (uses handoffs to teacher and engineer)
- Workspace evidence integration

## Key Responsibilities
1. Absorb teacher critique and workspace evidence
2. Draft candidate revisions using smallest defensible changes
3. Predict teacher approval before finalizing
4. Use handoffs appropriately (teacher for unclear guidance, engineer for structure)
5. Report full reasoning trajectory (not answer-only)

## Likely Failure Modes
1. Over-scope revisions beyond the immediate critique target
2. Missing guidance loops when the critique is incomplete or stale
3. Insufficient reasoning trajectory exposure
4. Premature finalization without teacher approval prediction
5. Incomplete workspace context integration
6. Insufficient constraint adherence (e.g., not using engineer skills correctly)

## Success Criteria
1. Clearly states which steering artifacts were followed
2. Exposes step-by-step reasoning with uncertainty
3. Implements smallest defensible revision
4. Predicts teacher outcome and identifies remaining blockers
5. Uses handoffs appropriately and sparingly
6. Reports validation results when applicable
7. Does not over-scope to unrelated improvements

## Next Optimization Hypothesis
The agent could benefit from:
- Clearer guidance on when to loop vs. finalize
- More explicit examples of "smallest defensible" revision
- Better constraint enumeration and checking
- Validation plan clarity
- Workspace context integration examples

## Validation Plan
- Run the trainer loop to completion
- Evaluate reasoning trajectory quality
- Assess handoff appropriateness
- Check constraint adherence
- Verify prediction accuracy for teacher approval
