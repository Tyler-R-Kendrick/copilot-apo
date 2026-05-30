# Teacher Steering — Turn 1

## Evidence Inspected
- `.github/agents/student.agent.md` — baseline prompt
- `engineer-prompt/review.md` — gap analysis with six identified weaknesses
- `iterations/iteration-1/research/research-brief.md` — source analysis confirming gaps
- `iterations/iteration-1/synthesize/datasets/train.jsonl` — 6 training rows covering evidence-reading, first-invocation, artifact-priority, self-check, engineer-handoff, and stopping behavior

## Steering Decision
**Continue with student revision.** The evidence supports a single targeted rewrite pass.

## Forecasted Student Mistakes
1. **Over-specifying the evidence order**: The student may add too many sub-bullets or conditions to the evidence order, turning it into a decision tree rather than a simple priority list.
2. **Expanding teacher-handoff criteria**: The student might add multiple edge cases to the teacher-handoff trigger rather than keeping it to three clear conditions.
3. **Inserting new sections rather than refining existing steps**: The student may create separate top-level sections for "Evidence Order" and "Self-Check Criteria" instead of weaving improvements into the existing structure minimally.

## Strongest Improvement Recommendation
Add an **Evidence Order** section (parallel to adversary.agent.md's evidence-reading pattern) and refine the existing Approach steps to:
1. Reference the evidence order explicitly in step 1 with a first-invocation fallback.
2. Replace the vague teacher-handoff trigger in the opening paragraph with three specific conditions: absent STEERING.md, stale STEERING.md (predates iteration), or STEERING.md explicitly contradicts candidate direction.
3. Anchor the self-check in step 6 to observable STEERING.md items rather than a subjective "would teacher approve" assessment.
4. Anchor the engineer handoff trigger to a structural quality gap (trajectory longer than revision body, or mixed structure).

## Key Evidence
- train.jsonl rows 1–3 test evidence-reading priority and first-invocation behavior.
- train.jsonl rows 4–6 test self-check bounds, engineer-handoff anchoring, and stopping behavior.
- The adversary.agent.md Evidence Order section provides a direct structural model.

## Uncertainty
Low. The gaps are clearly identified in the review.md and confirmed by train dataset construction. The rewrite is minimal and does not change the agent's role or tool contract.

## Stop or Continue
Continue to student revision. After revision, run pytest and check that all 6 train rows would be correctly addressed by the revised prompt behavior.

---
**Steering Note for STEERING.md:**
Revise student.agent.md by adding an Evidence Order section (4 items: current STEERING.md → per-agent summary.md → current candidate → workspace evidence, plus first-invocation fallback). In the opening paragraph, replace the vague teacher-handoff trigger with: invoke teacher only when STEERING.md is absent, predates the current iteration, or explicitly contradicts the candidate's direction. In Approach step 6, anchor the approval prediction to: all explicit STEERING.md items addressed and no new constraint violations introduced. In the engineer-handoff opening, anchor trigger to: trajectory longer than revision body, or implementation detail and policy rationale interleaved. Keep the rewrite minimal.
