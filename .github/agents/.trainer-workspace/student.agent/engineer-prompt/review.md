# Student Agent Optimization Review

## Target File
`./.github/agents/student.agent.md` — a handoff agent that specializes in teacher-guided candidate revision during trainer-led optimization loops.

## Current Role
The student agent is responsible for:
- Absorbing teacher critique and workspace evidence
- Implementing the smallest defensible candidate revision
- Exposing reasoning trajectory, plan, tradeoffs, and uncertainty
- Pre-emptively predicting teacher approval before finalizing
- Requesting additional teacher guidance when critique is unclear
- Using engineer handoff to improve reasoning clarity and structure

## Optimization Goal
Improve the student agent's ability to:
1. **Extract actionable revision targets** from teacher critique by distinguishing clear improvement signals from incomplete or contradictory feedback
2. **Implement disciplined, bounded revisions** that stay tightly scoped to the identified problem without scope creep
3. **Expose uncertainty and tradeoffs** in the reasoning trajectory so the teacher can validate the plan before execution
4. **Predict teacher approval accurately** by modeling whether the revised candidate addresses the stated critique without introducing new issues
5. **Delegate specialist tasks appropriately** via engineer handoff when prompt-engineering or code-optimization expertise would improve clarity

## Current Behavior Assessment
Strengths:
- Clear framing of constraints and output format
- Proper delegation structure (teacher and engineer handoffs)
- Explicit guidance on no-op handling and self-check discipline
- Good separation of concerns between student, teacher, engineer, and judge roles

Potential Gaps:
- The "pre-emptively predict teacher approval" step could be more explicit about criteria (e.g., does the revision address the stated goal? Does it avoid new issues? Is the scope appropriate?)
- Limited guidance on how to handle contradictory or conflicting teacher feedback
- No explicit pattern for when to request a teacher turn instead of iterating further
- Reasoning trajectory format is suggested but not exemplified

## Success Criteria
A stronger student agent should:
- ✓ Clearly distinguish actionable feedback from vague guidance
- ✓ Implement revisions that address the specific critique without overreach
- ✓ Model teacher approval with explicit criteria (addresses goal? avoids new issues? scope appropriate?)
- ✓ Escalate to teacher when critique is incomplete or contradictory
- ✓ Use engineer handoff strategically when specialist guidance improves clarity
- ✓ Preserve reasoning chains so the teacher can follow the justification

## Likely Failure Modes
1. **Approval misprediction** — revising too broadly or too narrowly, missing the actual teacher intent
2. **Scope creep** — fixing adjacent issues that fall outside the stated critique
3. **Unclear reasoning** — exposing plan but not the uncertainty or tradeoffs that led to it
4. **Missed escalation** — applying a revision when teacher guidance is actually needed first
5. **Engineer handoff overuse** — formatting work that the student should handle directly

## Validation Plan
After optimization:
- Run `python -m pytest -q` to ensure no breaking changes to repository tests
- Verify that the agent contract is maintained (same tools, handoffs, argument hints)
- Confirm that the revised prompt text is more explicit about decision criteria and escalation patterns

## Next Optimization Hypothesis
The student agent would benefit from:
1. More explicit criteria for predicting teacher approval (e.g., a checklist)
2. Concrete examples of how to structure reasoning trajectories (chain-of-thought patterns)
3. Clearer rules for when to escalate to teacher vs. when to self-correct
4. More actionable guidance on scope discipline when revising

## Starting Point
Begin with a dataset of teacher critiques and student responses from existing optimization runs or synthesized examples to ground the revision in realistic scenarios.
