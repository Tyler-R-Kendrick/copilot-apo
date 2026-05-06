# Original Candidate: student.agent.md

## Description
The original `student.agent.md` prompt defines the student role for teacher-guided candidate revision. It has the correct role definition and basic structure but lacks: (1) a stale-critique gate at step 1, (2) an explicit smallest-change filter, (3) a concrete engineer handoff trigger, (4) a hard convergence stopping criterion, and (5) a named validation command.

## Predicted Judge Response
The judge would score the original as partially satisfactory. The agent has the right goal, constraints, and output format, but the approach section does not prevent the most common failure modes (unbounded loops, over-revision, answer-only output). Score: approximately 3/5.

## Reflection
The original is a reasonable starting point but clearly improvable. The 5 identified gaps are all grounded in the training data and research brief. The student candidate should score higher because it directly addresses each failure mode.
