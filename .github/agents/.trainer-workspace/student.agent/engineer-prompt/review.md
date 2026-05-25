# Engineer-Prompt Review — .github/agents/student.agent.md

**Review date:** 2026-05-25  
**Reviewer:** trainer agent  
**Target file:** `.github/agents/student.agent.md`

---

## Goal

Assess the current student agent as an optimization target for teacher-guided candidate revision work in prompt-optimization workflows. Emphasis on evidence ordering, revision discipline, reasoning trajectory clarity, and teacher-approval prediction.

The optimization target is operational reliability within the teacher-student loop. A strong student agent should absorb teacher critique precisely, implement the smallest defensible revision, expose its reasoning trajectory clearly, and predict teacher approval accurately before reporting done.

---

## Current Strengths

- Role scoping is sharp: revise candidates, expose reasoning, predict teacher approval.
- The constraints correctly prohibit judging, adversarial review, and loop orchestration.
- The approach has reasonable coverage: read critique → draft revision → predict approval.
- Output format requires explicit reasoning format exposure which supports teacher review.
- Engineer handoff is correctly scoped to formatting, not delegation of revision work.

---

## Likely Failure Modes

1. **No evidence reading order.** The approach says "read the teacher goal, latest teacher critique, current teacher turn STEERING.md…" but does not specify what to do when steering artifacts are missing or incomplete—leaving the agent to improvise whether to proceed or request refreshed guidance.

2. **Vague approval-prediction step.** Step 6 says "predict whether the teacher would approve" but gives no concrete criteria for when "approval looks unlikely" should trigger another teacher turn vs. a single self-check. This can lead to either infinite loops or premature claims of completion.

3. **Revision scope creep risk.** "Smallest defensible candidate revision" is stated as a constraint, but there is no explicit check for whether the revision expands the prompt interface, adds new tools, or introduces new handoffs beyond what the current critique supports.

4. **Engineer handoff confusion.** The constraints say "Do not use engineer-prompt, engineer-code, or any other engineer skills directly" but the approach says "hand off to engineer to help format the reasoning." This creates ambiguity about which `engineer` is meant: the `engineer` agent handoff listed in the frontmatter vs. an engineer skill.

5. **Missing context when workspace artifacts are absent.** The approach says to read workspace steering artifacts but gives no fallback when those artifacts do not yet exist—for example, on the first iteration before any steering has been written.

6. **No validation instruction.** Step 7 says "Run the relevant validation or measurement step" but gives no guidance on which validation is relevant for a candidate revision (e.g., pytest, eval check, diff review), leaving the agent to guess.

---

## Dataset Gaps

- No authored evals exist yet for this agent.
- Need train and validation rows covering: missing-steering fallback, approval-prediction logic, revision scope discipline, engineer-handoff disambiguation, and validation step clarity.
- `scoring: llm_judge` is appropriate since outputs are open-ended candidate revisions and reasoning trajectories.

---

## Validation Plan

1. Author `evals/evals.json` with representative scenarios.
2. Generate `train.jsonl` and `val.jsonl` as genuine holdout splits.
3. Run `trainer-optimize` with `judge_mode=llm_judge` (3+ iterations).
4. Review candidate reasoning trajectories for evidence ordering, approval-prediction quality, and scope discipline.
5. Re-run `python -m pytest -q` after applying any accepted revision.

---

## Next Optimization Hypothesis

The next revision should:

1. Add an explicit evidence reading order with a fallback when steering artifacts are absent.
2. Sharpen the approval-prediction step: name the exit criteria (teacher predicted to approve, no further critique provided, or turn cap reached) and make the single self-check rule explicit.
3. Clarify the engineer handoff: refer to the `engineer` agent handoff (listed in frontmatter), not engineer skills.
4. Add a scope-check constraint before finalizing: confirm the revision does not expand the prompt interface, add tools, or introduce handoffs beyond the current critique.
5. Add minimal validation guidance: recommend running `python -m pytest -q` or checking the active iteration validation log after applying a candidate revision.
