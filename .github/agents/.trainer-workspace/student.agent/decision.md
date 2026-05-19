# Decision Summary — student.agent optimization (iteration-1)

## What Was Optimized

**Target file**: `.github/agents/student.agent.md`
**Optimization goal**: Fix six failure modes identified in the engineering review:
1. Underspecified reasoning trajectory format
2. Ungrounded teacher-approval prediction
3. Missing convergence signal
4. Overly broad engineer handoff trigger
5. Underspecified validation step
6. Passive steering artifact navigation

## Winning Candidate and Justification

**Winner**: `iterations/iteration-1/candidates/student/prompt.md` (from `optimize/optimized-prompt.md`)

The student candidate was selected because:
- All six FM fixes are present and minimal (no scope creep)
- All existing contract strings required by tests are preserved
- Teacher review (turn 1) confirmed convergence: all six rubric dimensions PASS
- Adversary review found no credible exploits

The adversary candidate was identical to the student candidate — stress tests did not reveal any failure mode requiring further modification.

## Changes Applied to student.agent.md

### FM-1: Reasoning format default (step 3)
Added: "Choose the reasoning format based on candidate length: use sketch-of-thought for candidates under 80 lines, chain-of-thought for candidates of 80 lines or more, and tree-of-thought only when the task requires comparing branching design tradeoffs."
Output Format updated: Added "prefer sketch-of-thought for candidates under 80 lines, chain-of-thought for 80 lines or more, and tree-of-thought only when branching design tradeoffs exist."

### FM-2: Teacher-approval prediction grounding (Constraints + step 6)
Added to Constraints: "Ground that prediction in at least one specific rubric dimension named in the most recent STEERING.md."
Added to step 6: "Ground the prediction in at least one specific rubric dimension from the most recent STEERING.md."

### FM-3: Convergence signal (step 6)
Added to step 6: "If the current draft addresses all critiques named in the latest STEERING.md and introduces no new constraints, the loop is done — signal convergence and do not request another teacher turn."
Output Format updated: Added "or an explicit convergence signal if all named critiques are addressed and no new constraints were introduced."

### FM-4: Engineer handoff trigger (body + step 4)
Body updated: Changed broad trigger to "only when (a) your draft rationale contains unexplained jargon the teacher may misread, or (b) you cannot confidently rank competing revisions without Trace-based expertise. Do not invoke the engineer handoff for routine rewrites or clarity edits."
Step 4 updated: Added "but only when (a) the draft rationale contains unexplained jargon the teacher may misread, or (b) you cannot confidently rank competing revisions without Trace-based expertise."
Output Format updated: Added "including which of the two specific trigger conditions applied."

### FM-5: Validation step (step 7)
Changed: "Run the relevant validation or measurement step and report what changed." → "Run `python -m pytest -q` from the repository root and report the result, including pass/fail counts."
Output Format updated: Added "State the `python -m pytest -q` result." as final bullet.

### FM-6: Steering artifact scoping (step 1)
Added to step 1: "Always read these artifacts from the path recorded in `required_artifacts.latest_iteration_dir` in `workflow-status.json`; do not read from a sibling iteration directory."
Output Format updated: Added "citing the path under `required_artifacts.latest_iteration_dir`."

## Validation Result

`856 passed in 8.60s` — all tests pass, including `test_student_agent_contract_structure` which checks 13 specific contract strings.

## Recommended Follow-up

1. **Tree-of-thought trigger breadth**: The phrase "branching design tradeoffs" could be interpreted broadly. Consider adding a concrete example in a future iteration if agents over-use tree-of-thought.
2. **Rubric-discovery fallback**: The convergence criterion assumes STEERING.md always contains explicit rubric dimensions. If a fresh iteration has no steering artifact, the grounding rule cannot fire. The workflow contract (not the student agent) should ensure STEERING.md is always seeded with rubric dimensions before the student turn.
