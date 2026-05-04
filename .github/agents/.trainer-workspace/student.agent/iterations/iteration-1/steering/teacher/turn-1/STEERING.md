# Teacher Steering — student.agent.md — Iteration 1 / Turn 1

## Status
Candidate is READY FOR WRITE-BACK. One optional micro-improvement identified but not a blocker.

## Summary of Review
The optimized candidate addresses all 8 training failure modes from the research brief and engineer-prompt review:
- Failure mode 1 (over-revision): Approach step 6 now explicitly forbids changes beyond the stated criterion.
- Failure mode 5 (orchestration tasks): New constraint with explicit decline and scope explanation.
- Failure mode 6 (output format): 5 sections are now explicitly numbered and labeled.
- Failure mode 7 (stale steering): Constraint + approach step 1 now require citing the most recent artifact and stating supersession explicitly.
- Failure mode 8 (no-op): Approach step 3 added as an explicit pre-draft no-op check.
- Failure modes 2, 3, 4: Preserved and modestly improved from original.

No bloat or scope creep. All original working behaviors preserved.

## Remaining Gap (Non-blocking)
The new orchestration-decline constraint uses "do not" prohibition language. Engineer-prompt review and criterion 4 prefer positive routing rules. One targeted fix:

**Replace** (constraints section):
> "Do not accept or execute orchestration tasks (running optimizers, committing files, updating eval manifests, re-running validation suites) that belong to the trainer agent; decline them explicitly and explain the scope boundary."

**With**:
> "When asked to perform orchestration tasks that belong to the trainer agent (running optimizers, committing files, updating eval manifests, re-running validation suites), redirect the request to the trainer and explain the scope boundary."

## Scope Constraint for Student (if another turn is triggered)
- Change exactly this one sentence in the Constraints section.
- Do not modify any other constraint, approach step, output format section, or YAML front matter.
- Predict teacher approval after the swap; if the swap is made correctly, finalize immediately.

## Forecasted Student Mistake
Student may over-revise: seeing one sentence to fix, they are likely to rewrite adjacent prohibition bullets for "consistency" or add new output-format notes. The steering must explicitly state single-sentence scope.

## Missing Evidence
- No scored validation results (model not available; manual_followup mode)
- No judge scores or adversary stress-test results
- These are expected for this run mode and do not block write-back of the structural candidate

## Decision
Write-back is justified now. If the trainer prefers the positive-routing fix first, trigger one tightly scoped student turn using the replacement sentence above.
