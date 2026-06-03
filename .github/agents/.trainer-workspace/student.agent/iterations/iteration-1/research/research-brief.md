# Research Brief: student.agent.md Optimization

## Target and Task Summary

**Target:** `.github/agents/student.agent.md`
**Task:** Identify behavioral criteria and evaluation patterns for the student agent's role in teacher-guided candidate revision loops.

**Optimization Goal:** Improve evidence reading discipline, revision scope control, teacher and engineer handoff trigger quality, explicit reasoning trajectories, teacher-approval prediction accuracy, and candidate write-back compliance.

## Research Plan

**Target layout:**
- Eval manifest: `iterations/iteration-1/synthesize/evals/evals.json`
- APO datasets: `iterations/iteration-1/synthesize/datasets/train.jsonl` and `val.jsonl`

**Approval bar:** All sources are repository-internal canonical contracts with traceable origin and stable versions.

## Source Evaluation

### Primary Sources

1. **`student.agent.md`** — defines expected behaviors, output format, handoff triggers, and revision constraints.
   - Authority: repository-owned canonical contract
   - Fit: exact match; forms ground truth

2. **`teacher.agent.md`** — defines what teacher critique and steering artifacts look like (the main inputs to the student).
   - Authority: repository-owned canonical contract
   - Fit: defines the input shape for all student revision tasks

3. **`trainer-train-agent/SKILL.md`** — defines handoff behavioral requirements and candidate write-back expectations.
   - Authority: repository-owned skill contract
   - Fit: defines what the orchestrator expects the student to produce

4. **Existing agent workspace examples** (researcher, conservator) — provide concrete eval row shapes, judge-mode selection, and workspace layout reference.
   - Authority: repository-internal prior runs
   - Fit: structural reference only

## Key Behavioral Dimensions

1. **Evidence reading order** — The student must read the active `STEERING.md` and per-agent summary before planning a revision.
2. **Revision scope discipline** — The student must not change prompt interface placeholders, eval shapes, or constraints unless the teacher steering explicitly authorizes that change.
3. **Teacher handoff trigger quality** — The student should hand off to teacher only when critique is genuinely insufficient, not as a reflexive first step.
4. **Engineer handoff trigger quality** — The engineer handoff should only reformat the reasoning trajectory artifact, not substitute for the candidate revision.
5. **Reasoning trajectory completeness** — Output must expose plan, tradeoffs, and uncertainty rather than producing answer-only output.
6. **Teacher approval prediction** — The student must predict whether the teacher would approve before finalizing, and either refine or request another teacher turn if approval is unlikely.
7. **Candidate write-back compliance** — The student must save the revised candidate to the correct `candidates/student/` path.

## Mapping Notes

- Judge mode: `llm_judge` (agent behavior quality is open-ended)
- Row fields: `input`, `reference`, `criteria`, `scoring`
- Train split: 6 rows covering the 7 key behavioral dimensions with variation
- Val split: 3 rows covering edge cases and adversarial inputs

## Unresolved Gaps

None — repository-owned contracts provide sufficient coverage for all behavioral dimensions.
