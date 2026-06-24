# Engineer Prompt Review: student.agent.md

## Target
Optimize the `student` agent definition for better task clarity, tighter constraint enforcement, and improved handoff signaling to prevent scope creep during prompt optimization loops.

## Current State Analysis
The `student.agent.md` file defines an agent that applies teacher critique to candidate prompts during trainer-led optimization. The agent:
- Acts as a specialized prompt reviser guided by teacher feedback
- Implements the smallest defensible candidate revision
- Reports reasoning trajectory and tradeoffs
- Includes handoffs to `teacher` (for guidance) and `engineer` (for format cleanup)

## Success Criteria
A successful revision should:
1. **Clarify responsibility boundaries**: Make it explicit that the student does NOT orchestrate the trainer loop, judge candidates, or run trainer skills
2. **Tighten handoff conditions**: Specify precisely when to hand off to teacher vs. when to proceed independently
3. **Improve no-op detection**: Help the student recognize when supplied evidence does NOT support a better candidate, reducing wasteful turns
4. **Strengthen approval prediction**: Make the teacher-approval forecast more reliable so fewer false-positive revisions are submitted
5. **Reduce instruction bloat**: Consolidate redundant guidance that appears in both "Constraints" and "Approach" sections

## Likely Failure Modes
1. **Scope creep**: Student attempts to fix unrelated code or broaden the revision beyond what teacher asked for
2. **Infinite loops**: Student keeps requesting teacher feedback on the same evidence without attempting a revision
3. **False no-ops**: Student reports "no better candidate possible" when a small, defensible change exists
4. **Weak approval prediction**: Student submits revisions that teacher immediately rejects, indicating the forecast was wrong
5. **Over-engineering**: Student formats reasoning so elaborately that it obscures the actual revision

## Optimization Hypothesis
Rewrite the agent to:
- Use more deterministic language about when conditions warrant a handoff vs. direct action
- Add concrete examples of "defensible revision" so the boundary is clearer
- Simplify the reasoning format guidance to focus on *clarity* rather than exhaustive reasoning
- Explicitly warn against approval-prediction false positives by adding a pre-submission validation step
- Consolidate Constraints and Approach into a tighter, sequenced flow

## Validation Plan
1. Run `python -m pytest -q` from repo root to ensure no breaking changes to agent contracts or supporting tooling
2. Check that the rewritten agent still satisfies handoff signatures expected by the trainer contract
3. Manually inspect a small sample of next teacher-student turns to verify the student follows the new guidance

## Notes
- This is a core orchestration agent in the trainer loop; changes here affect all subsequent prompt optimization work
- The agent should remain lightweight and not duplicate trainer responsibilities
- Keep the handoff signatures and tool list unchanged unless the trainer loop explicitly needs to expand them
