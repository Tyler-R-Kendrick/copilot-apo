# Engineer-Prompt Review: student.agent.md

## Target Goal
Improve `student.agent.md` so the student agent more reliably produces defensible, teacher-approved candidate revisions within a bounded number of turns. The key quality axes are: triggering accuracy (description frontmatter precision), revision scope discipline (smallest defensible change), exit-criteria clarity (when to stop looping vs. escalate), and output conciseness (not producing answer-only or overly-verbose outputs).

## Current State Analysis

**Strengths**
- Clear role: teacher-guided candidate revision specialist with explicit reasoning trajectory requirement.
- Appropriate handoff gates: teacher when critique is unclear, engineer when explanation needs structure.
- Reasonable constraint list covering no-judging, no-orchestration, and no-answer-only outputs.
- Approach steps are logically ordered.

**Likely Failure Modes**

1. **Triggering ambiguity**: The description says "Use when drafting or revising prompt candidates from teacher guidance inside trainer-led optimization loops." The phrase "from teacher guidance" may cause agents to skip this when teacher guidance hasn't been received yet but is expected. It could also trigger when a student-like role is needed outside optimization loops.

2. **Revision scope drift**: "Implement the smallest defensible candidate revision" is the right directive but has no guidance on what "defensible" means quantitatively. Students may under-revise (zero-diff no-ops when small changes are actually warranted) or over-revise (rewriting sections unrelated to the critique).

3. **Approval-prediction loop ambiguity**: Step 6 says "do at most one extra self-check only if the draft still looks unsupported." The phrase "only if" creates an implicit loop that may cycle through multiple teacher turns without real convergence. The exit criteria (teacher predicts no further improvement, student predicts teacher approval) need to be more explicit in the agent body.

4. **No criteria for justified no-op vs. teacher escalation**: When the student reports a justified no-op it is unclear whether they should request a new teacher turn or let the trainer decide. This gap leads to either unnecessary looping or silent stalling.

5. **Output verbosity vs. conciseness tradeoff**: The output format asks for reasoning trajectory, revision, engineer-handoff summary, approval prediction, and validation result—but gives no guidance on relative length. Students often produce excessively long outputs that obscure the actual revision.

6. **Engineer handoff scope is vague**: "format your reasoning trajectory and solution plan into a clearer teacher-ready explanation" is ambiguous about what level of restructuring is acceptable. Students may either underuse this handoff (when it would help) or misuse it to offload the revision thinking itself.

## Dataset Gaps
No authored `evals/evals.json` or `train.jsonl`/`val.jsonl` exist for this skill. Need synthesis covering:
- Typical invocations: a teacher critique + current candidate → student produces minimal targeted revision with reasoning
- Edge cases: incomplete critique, contradictory steering, no-op scenarios, engineer handoff triggers
- Quality axes: revision scope correctness, reasoning transparency, approval prediction accuracy, exit condition signaling

## Validation Plan
1. Run `python -m pytest -q` from repo root after any file edits.
2. Evaluate synthesized val cases with `judge_mode=llm_judge` (rows use `reference` + `criteria`).

## Optimization Hypothesis
The highest-value improvements are:
1. **Tighten the description** to improve triggering accuracy.
2. **Add explicit exit criteria** into the Constraints section so the student knows when to stop looping vs. escalate to the trainer.
3. **Clarify "defensible" revision scope** with a concrete signal (e.g., whether the critique names a specific gap the revision addresses).
4. **Sharpen the engineer handoff boundary** to distinguish formatting help from revision delegation.
5. **Add output length guidance** to prevent verbose no-signal outputs.

These changes are small, targeted, and do not change the prompt's interface (frontmatter keys, handoff structure, or tool list).
