# Teacher Steering — Turn 1

## Artifacts Reviewed
- `engineer-prompt/review.md` (primary evidence)
- `iterations/iteration-1/optimize/optimized-prompt.md` (candidate under review)
- `iterations/iteration-1/research/research-brief.md` (dataset and failure mode context)
- Original `student.agent.md` (baseline)

## Does the Candidate Address All Six Failure Modes?

### FM-1: Reasoning Trajectory Format — ✅ ADDRESSED
The candidate adds a concrete length-based rule to Approach step 3:
> "use sketch-of-thought for candidates under 80 lines, chain-of-thought for candidates of 80 lines or more, and tree-of-thought only when the task requires comparing branching design tradeoffs"

This is the exact rewrite hypothesized in the engineering review. The Output Format section also reinforces it: "using the length-appropriate reasoning format." **No gap remaining.**

### FM-2: Teacher-Approval Prediction Grounding — ✅ ADDRESSED
The candidate updates Approach step 6 and the Constraints section to require grounding in "at least one specific rubric dimension named in the most recent STEERING.md." The Output Format section mirrors this: "grounded in at least one named STEERING.md rubric dimension." **No gap remaining.**

### FM-3: Convergence Signal — ✅ ADDRESSED
The candidate adds an explicit stopping criterion to step 6:
> "If the current draft addresses all critiques named in the latest STEERING.md and introduces no new constraints, the loop is done — signal convergence and do not request another teacher turn."

This is concrete and actionable. **No gap remaining.**

### FM-4: Engineer Handoff Trigger — ✅ ADDRESSED
The body text and step 4 now both specify exactly two trigger conditions:
1. Draft rationale contains unexplained jargon the teacher may misread
2. Cannot confidently rank competing revisions without Trace-based expertise

The body text adds the negative: "Do not invoke the engineer handoff for routine rewrites or clarity edits." This is the narrowing the engineering review requested. **No gap remaining.**

### FM-5: Validation Step — ✅ ADDRESSED
Step 7 now specifies exactly: "Run `python -m pytest -q` from the repository root and report the result, including pass/fail counts." The Output Format section also requires a pytest result section. **No gap remaining.**

### FM-6: Steering Artifact Navigation — ✅ ADDRESSED
Step 1 now adds: "Always read these artifacts from the path recorded in `required_artifacts.latest_iteration_dir` in `workflow-status.json`; do not read from a sibling iteration directory." The Output Format section reinforces: "citing the path under `required_artifacts.latest_iteration_dir`." **No gap remaining.**

## Forecasted Student Mistakes (for next loop awareness)

1. **Jargon in tree-of-thought trigger**: An agent might interpret "branching design tradeoffs" broadly and invoke tree-of-thought when chain-of-thought would suffice. The trigger is functional but could be sharpened with an example. This is a minor gap — not worth a further revision turn.

2. **Rubric dimension discovery**: An agent might struggle if the STEERING.md does not contain an explicit rubric section. The candidate assumes rubric dimensions are always named in STEERING.md, which is the expected convention but may occasionally fail in fresh iterations with no steering artifact. This is a workflow contract issue, not a student agent issue.

## Convergence Assessment

All six named critiques from the engineering review are addressed. The candidate makes no changes outside the six fix areas. No new constraints are introduced. The changes are minimal: targeted sentence additions and replacements in the body, step 4, step 6, step 7, and the Output Format section.

**Recommendation: Stop the loop. Apply the candidate.**

The student candidate does not require another revision turn. The two forecasted issues (jargon scope, rubric discovery) are minor and do not block the current improvement goal.

## Concise Steering Note (for STEERING.md copy)

> **Status**: All six FM fixes confirmed. Candidate is convergent.
> **Evidence**: engineer-prompt/review.md FM list vs. optimized-prompt.md diff — all six targeted changes present, no scope creep.
> **Rubric dimensions satisfied**: reasoning_transparency ✅, teacher_approval_prediction_quality ✅, convergence_discipline ✅, engineer_handoff_appropriateness ✅, validation_compliance ✅, steering_artifact_citation ✅
> **Recommendation**: Apply candidate to student.agent.md. No further student turn needed.
> **Minor watch items** (post-apply): tree-of-thought trigger breadth; rubric-discovery fallback if STEERING.md has no rubric section.
