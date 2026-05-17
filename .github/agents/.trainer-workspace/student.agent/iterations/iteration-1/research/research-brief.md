# Research Brief: student.agent.md Optimization

## Target and Task Summary

**Target file**: `.github/agents/student.agent.md`  
**Task**: Optimize the student agent for teacher-guided candidate revision in trainer-led prompt-optimization loops.  
**Scoring rule**: `llm_judge` — responses are evaluated against reference criteria for revision precision, reasoning trajectory completeness, loop-exit discipline, and teacher-approval prediction quality.

## Research Plan and Approval Bar

The student agent's primary behaviors are:
1. Absorb teacher critique and identify the revision target
2. Produce the smallest defensible candidate revision
3. Expose an explicit reasoning trajectory (plan, tradeoffs, uncertainty)
4. Predict teacher approval before finalizing

To build eval cases, we need scenarios that test:
- Correct revision targeting (narrowing to the stated critique item)
- Reasoning transparency (plan + tradeoffs vs. answer-only)
- Loop-exit discipline (stopping when evidence is exhausted)
- Teacher handoff timing (requesting guidance only when genuinely blocked)
- Approval prediction accuracy (spotting unaddressed critique items)

**Approval bar for source material**:
- Source must represent realistic prompt-optimization loop scenarios
- Must provide observable expected behaviors (not subjective quality assessments)
- Scenarios derived from the existing `student.agent.md` behavior contract and `teacher.agent.md` interaction patterns in this repository

## Approved Sources

1. **Repository student.agent.md + teacher.agent.md contracts** (this repo)  
   - Authority: first-party behavior contracts  
   - License: repo-owned  
   - Rationale: The agent's own contract defines the input/output interface; teacher critique scenarios can be derived from the stated constraints and approach steps.

2. **Existing researcher.agent.md eval cases** (this repo)  
   - Authority: first-party authored evals  
   - License: repo-owned  
   - Rationale: Structural reference for eval row shape, scoring fields, and assertion style; not reused as content.

3. **Prompt-optimization literature patterns** (general domain knowledge)  
   - Rationale: Scenarios for reasoning trajectory completeness and loop-exit behavior are well-characterized in iterative refinement literature. No specific public dataset required — test cases are synthetic but grounded in the agent's documented behavior constraints.

## Rejected Candidates

- **External NLP benchmark datasets (e.g., SuperGLUE, RAFT)**: Rejected — these test general NLP skill, not agentic revision loop behavior.
- **Generic coding or QA datasets**: Rejected — not relevant to teacher-guided prompt revision.

## Mapping Notes for Downstream Synthesis

Each eval row maps to a student agent invocation scenario:
- `input`: A realistic caller request containing current candidate text, teacher critique, and workspace context
- `reference`: A description of the compliant response (revision applied, reasoning exposed, approval predicted)
- `criteria`: Checklist of observable behaviors derived from the agent's Constraints + Approach sections
- `scoring`: `llm_judge` for all rows (revision quality requires semantic scoring)

Key field derivations:
- `input.current_candidate` → prompt text being revised
- `input.teacher_critique` → specific critique items from the teacher
- `input.steering_artifact` → active STEERING.md path or summary
- `reference` → describes what a correct minimal revision looks like
- `criteria` → observable checklist items matching the Constraints section

## Unresolved Gaps

None blocking synthesis. All required scenario types can be derived from the agent's own contract plus domain knowledge of iterative refinement loops. Proceeding to synthesis.
