# Research Brief: student.agent.md Optimization

## 1. What Task the Student Agent Performs

The `student` agent operates inside a trainer-led prompt optimization loop. Its core task is:
- Absorb teacher critique from `steering/<agent>/turn-N/STEERING.md` artifacts
- Inspect workspace evidence (current candidate, prior runs, eval artifacts)
- Implement the **smallest defensible revision** to the candidate prompt that advances the iteration goal
- Expose an explicit **reasoning trajectory** so the teacher can evaluate not just the output but the reasoning that produced it
- **Predict teacher approval** before declaring the loop done
- Hand off to `teacher` when critique is incomplete/stale, and to `engineer` when the reasoning needs better structure

The agent is explicitly scoped to revision and reasoning — not judging, orchestrating, or running the trainer loop.

## 2. What Distinguishes a Good vs. Weak Student Revision

### Good Student Revision
- **Addresses all named critiques** from the latest STEERING.md with traceable changes
- **Uses an appropriate reasoning format**: sketch-of-thought for short/simple edits, chain-of-thought for moderate complexity, tree-of-thought for branching design tradeoffs
- **Grounds teacher-approval prediction** in at least one specific rubric dimension (e.g., "constraint compliance," "reasoning transparency," "output format coverage")
- **Makes the smallest defensible change**: no scope creep, no gratuitous additions
- **Reports validation**: runs `python -m pytest -q` and includes results
- **Reads from the right iteration**: cites `required_artifacts.latest_iteration_dir` for steering, not stale past-iteration paths

### Weak Student Revision
- Produces output without explaining reasoning, or with vague "I considered X" placeholders
- Predicts "teacher will approve" without grounding in rubric evidence
- Rewrites the entire candidate when one sentence needed changing
- Invokes the `engineer` handoff for routine formatting tasks
- Skips the validation step or reports "tests likely pass"
- Reads from a past iteration's steering when a newer one exists

## 3. Key Failure Modes to Test For

### FM-1: Reasoning Trajectory Underspecification
**Trigger**: Revision output section is present but no explicit reasoning format is used.
**What to test**: Give the student a mid-length candidate (~50 lines) with a specific critique. Expect chain-of-thought (not sketch, not tree).
**Indicator of failure**: Prose rationale with no labeled steps or branching analysis.

### FM-2: Ungrounded Teacher-Approval Prediction
**Trigger**: Student says "teacher would likely approve" but cites no specific criterion.
**What to test**: Supply a revision that partially addresses the critique. Expect the student to identify which rubric dimension still has a gap before predicting approval.
**Indicator of failure**: Optimistic approval prediction with no supporting rubric citation.

### FM-3: Missing Convergence Signal
**Trigger**: Critique is fully addressed but student requests another teacher turn unnecessarily.
**What to test**: Give the student a scenario where all named critiques are resolved and no new constraints introduced.
**Indicator of failure**: Student requests a teacher turn despite convergence criteria being met.

### FM-4: Overly Broad Engineer Handoff
**Trigger**: Student invokes `engineer` handoff for a routine formatting or clarity task.
**What to test**: Give the student a simple text-rewrite critique. Expect no engineer invocation.
**Indicator of failure**: Engineer handoff is mentioned or invoked without jargon/ranking justification.

### FM-5: Validation Noncompliance
**Trigger**: Student completes revision but does not run or report `python -m pytest -q`.
**What to test**: Check that every valid completion includes a pytest result.
**Indicator of failure**: No validation section, or "skipped" validation claim.

### FM-6: Stale Steering Read
**Trigger**: Multiple iteration directories exist; student reads from wrong iteration.
**What to test**: Prompt scenario where two steering paths exist. Expect citation of `required_artifacts.latest_iteration_dir`.
**Indicator of failure**: Student cites a non-latest iteration path or makes no iteration-dir citation.

## 4. Dataset Schema Guidance

### Judge Mode
Use `judge_mode=llm_judge` because evaluation of student revisions is **qualitative**:
- Did the reasoning trajectory appear?
- Was the revision defensible and minimal?
- Was the teacher-approval prediction grounded?

These require LLM-based assessment, not exact-match scoring.

### Row Format
```json
{
  "prompt": "<task scenario and critique for student>",
  "context": "<workspace evidence, current candidate excerpt, latest STEERING.md content>",
  "reference": "<what an ideal student response looks like>",
  "criteria": "<rubric dimensions to evaluate against>",
  "scoring": "llm_judge"
}
```

### Rubric Dimensions to Include in `criteria`
1. **Reasoning transparency**: Does the response use an explicit reasoning format (sketch/chain/tree-of-thought) appropriate to task length?
2. **Revision minimality**: Is the change the smallest defensible edit, or is there scope creep?
3. **Teacher-approval prediction quality**: Is the prediction grounded in at least one named rubric dimension from the latest STEERING.md?
4. **Validation compliance**: Is a `python -m pytest -q` result included?
5. **Steering artifact citation**: Does the response cite the correct iteration's steering path?
6. **Convergence discipline**: Does the student correctly signal done vs. needs-more-turns?
7. **Engineer handoff appropriateness**: Is the engineer handoff invoked only for jargon-heavy or ranking-uncertain scenarios?

### Dataset Split
- **Train**: 8 rows covering FM-1 through FM-6, with some normal/pass cases
- **Val**: 4 rows covering cross-cutting scenarios that blend multiple failure modes

### Coverage Matrix for Datasets
| Row | Primary FM | Secondary FM | Type |
|-----|-----------|--------------|------|
| T1  | FM-1      | —            | Train |
| T2  | FM-2      | —            | Train |
| T3  | FM-3      | FM-2         | Train |
| T4  | FM-4      | —            | Train |
| T5  | FM-5      | —            | Train |
| T6  | FM-6      | —            | Train |
| T7  | —         | —            | Train (pass case) |
| T8  | FM-1+FM-2 | FM-5         | Train (hard) |
| V1  | FM-2+FM-3 | FM-5         | Val |
| V2  | FM-1+FM-6 | —            | Val |
| V3  | FM-4+FM-5 | —            | Val |
| V4  | —         | —            | Val (ideal case) |

## 5. Sources Consulted
- `.github/agents/student.agent.md` (current target)
- `.github/agents/.trainer-workspace/student.agent/engineer-prompt/review.md`
- `.github/agents/teacher.agent.md` (teacher collaboration context)
- Trainer loop contract principles from `skills/trainer-train/`
