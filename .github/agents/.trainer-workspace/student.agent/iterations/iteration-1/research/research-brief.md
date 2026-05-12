# Research Brief: student.agent.md

## Target and Task Summary

Target file: `.github/agents/student.agent.md`  
Task description: Evaluate whether the student agent correctly absorbs teacher critique, reads workspace steering artifacts, implements the smallest defensible revision, exposes the reasoning trajectory, predicts teacher approval accurately, and knows when to escalate vs. finalize.  
Scoring rule: LLM judge with reference + criteria; the judge evaluates whether the agent output demonstrates correct revision scope, reasoning transparency, loop-exit discipline, and validation reporting.  
Domain: Iterative prompt optimization — teacher-guided candidate revision in trainer loops. No specific locale or jurisdiction constraints apply.

## Research Plan and Approval Bar

**Primary-source-first search plan:**  
No direct public benchmark exists for teacher-guided iterative prompt revision agents. The nearest analogous public work includes:
- Iterative prompt optimization datasets (APO, OPRO, TextGrad papers)
- Teacher-student learning benchmarks in NLP
- Code review and revision benchmarks (CodeReviewer, APPS refinement)

**Approval bar criteria applied:**
- Accountable maintainer or standards body
- Traceable data origin and label definitions
- Explicit license
- Stable version or date
- Acceptable contamination/leakage risk for eval authoring

## Approved Sources

No external public dataset directly models the teacher-guided candidate revision task as implemented in this repository. The task is repository-specific: it requires reading workspace STEERING.md artifacts, applying trainer-loop conventions, and predicting teacher approval within a bounded loop — behaviors with no direct public benchmark equivalent.

**Decision:** Proceed with grounded synthetic examples derived from the agent contract, the trainer-loop convention files, and representative failure modes identified in the engineer-prompt review. Apply the verifier-backed synthetic data standard: draft candidate rows, independently verify them against the agent contract and trainer loop references, then keep only high-confidence rows.

## Rejected Candidates

| Source | Reason Rejected |
|---|---|
| APO/OPRO papers | Optimization loop is automated, not teacher-student; no criterion for "smallest defensible revision" |
| Code review benchmarks | Different task domain; revision quality criteria differ significantly |
| Teacher-student distillation datasets | Knowledge distillation, not iterative prompt revision; no steering artifact concept |
| General QA datasets | No model of critique absorption or revision minimality |

## Mapping Notes

Since no external dataset cleared the approval bar, mapping notes describe how the agent contract fields map to synthetic eval rows:

- **Input**: A scenario prompt describing a teacher critique (as a STEERING.md excerpt), the current candidate, workspace context, and revision objective — phrasing as a realistic user request to the student agent.
- **Reference**: A description of a compliant response that reads the active STEERING.md, implements exactly the targeted revision, exposes the reasoning trajectory with plan/tradeoffs/uncertainty, and predicts teacher approval.
- **Criteria**: Observable behavioral predicates (e.g., "Response reads steering artifact before revising", "Revision addresses exactly the cited critique", "Reasoning trajectory includes tradeoffs").
- **Scoring**: `llm_judge`

Representative eval scenarios:
1. Clear single-focus critique → agent should implement minimal targeted revision
2. Ambiguous critique without named target → agent should hand off to teacher, not guess
3. Revision scope creep risk → agent should resist expanding beyond the cited constraint
4. Loop-exit scenario → agent should finalize when predicted approval is "yes"
5. Validation reporting → agent should run pytest and report results

## Unresolved Gaps

None that block synthesis. The agent contract provides sufficient specification for grounded synthetic evals. The verifier role is the agent contract itself: each synthetic eval row is checked against the explicit constraints and approach steps in `student.agent.md`.
