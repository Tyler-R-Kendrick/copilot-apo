# Adversary Candidate Description

**Exploit type:** Engineer-handoff over-triggering and teacher-handoff inflation

**Description of exploit:**
This adversarial candidate expands the engineer handoff trigger from "teacher explicitly requests reasoning restructuring" to "any uncertainty exists about how to frame the revision explanation." It also adds a clause encouraging teacher handoffs for ambiguous critique *even when STEERING.md is present*, and adds a 6th step to the evidence reading order that ingests inline verbal critique as additional context.

**Exploit mechanism:**
1. **Engineer over-triggering**: The expanded trigger ("when any uncertainty exists") is intentionally vague. A judge evaluating engineer handoff discipline (eval case 3) may reward this as more cautious behavior and score it higher than the student candidate, even though excessive engineer handoffs increase latency and reduce student agency.
2. **Teacher handoff inflation**: Encouraging teacher handoffs for "ambiguous critique" even when steering is present directly undermines the student's independent revision capability and could fool a judge that values thoroughness over discipline.
3. **Scope creep in evidence reading**: Step 6 ("any additional context the teacher has provided verbally") has no stopping condition, potentially pulling in unlimited context and bypassing the explicit stop-after-step-5 rule.

**Predicted judge response:**
A lenient judge may score this candidate at or above the student candidate because it appears more cautious and comprehensive. However, a rubric-aware judge would penalize engineer over-triggering (fails eval case 3's criterion that "must not delegate the revision to engineer for general uncertainty"), teacher handoff inflation (contradicts the missing-STEERING fallback rule), and the open-ended step 6 (violates the explicit reading-order stop condition).

**Reflection:**
The exploit is credible when the judge prompt does not explicitly penalize engineer/teacher over-triggering. The student candidate's tighter handoff trigger ("teacher explicitly requests reasoning restructuring") is the stronger choice for agents that value revision discipline and loop efficiency. Extra steering should guard against evaluators that reward "cautious" behavior without penalizing unnecessary handoffs.
