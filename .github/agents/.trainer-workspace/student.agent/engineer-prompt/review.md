# Student Agent Prompt Review

## Target File
`./.github/agents/student.agent.md`

## Current Purpose
The student agent is a specialist in teacher-guided candidate revision within trainer-led optimization loops. It absorbs teacher critique, inspects workspace evidence, implements minimal defensible revisions to prompts/context/evaluations, and explains the reasoning behind choices.

## Key Responsibilities
1. Read teacher goals and critique before revising candidates
2. Hand off to teacher for clarification when revision targets are unclear
3. Draft minimal, defensible candidate revisions with explicit reasoning
4. Use engineer handoff for prompt-engineering or Trace expertise formatting
5. Validate revisions against teacher expectations
6. Report justified no-ops when evidence doesn't support better candidate

## Success Criteria for Optimization
- **Clarity of agent role**: The prompt clearly defines student as a *revision implementer*, not an orchestrator or judge
- **Constraint enforcement**: Prompts explicitly prohibit direct skill invocation (engineer-prompt, engineer-code) and ban orchestration takeover
- **Reasoning transparency**: Output format requires explicit reasoning trajectory, plan, tradeoffs, and uncertainty—not answer-only responses
- **Teacher collaboration**: Includes clear handoff triggers for teacher and engineer guidance before and during revision
- **Validation mindset**: Student must predict teacher approval and justify when another loop turn is needed

## Current Weaknesses (Likely Optimization Targets)
1. **Scope creep risk**: The "do not take over" constraint is stated but may not be strongly enough woven into the core task description
2. **Reasoning clarity**: While the output format requires explicit reasoning, the main prompt body could better model what "explicit reasoning trajectory" actually looks like
3. **Handoff triggers**: The conditions for when to hand off to teacher vs. engineer are present but could be more granular—e.g., what if the critique is incomplete vs. contradictory?
4. **No-op justification**: The requirement to "report justified no-ops" is present, but the prompt doesn't emphasize predicting when a revision would regress
5. **Turn-scoped steering**: References to `steering/<agent>/turn-N/STEERING.md` are present, but guidance on *how to interpret* those artifacts for revision decisions is implicit

## Validation Plan
- After optimization, run `python -m pytest -q` to ensure no test regressions
- Validate that the prompt maintains its role as a handoff agent within trainer loops (not an orchestrator)
- Confirm output format examples or guidance still match the "reasoning trajectory + plan + tradeoffs + uncertainty" requirement
- Check that handoff logic is clearly separated: teacher for guidance, engineer for formatting

## Next Optimization Hypothesis
Strengthen the prompt by:
1. Making scope constraints (no orchestration, no direct skill invocation) more prominent in the core task
2. Adding explicit examples of what "reasoning trajectory" and "turn-scoped steering" interpretation looks like in practice
3. Clarifying handoff conditions with concrete decision trees (e.g., "if critique is incomplete, hand off to teacher; if explanation needs structure, hand off to engineer")
4. Emphasizing regression prediction and no-op justification as primary safeguards against invalid revisions
5. Reorganizing the approach section to front-load the steering inspection and guidance interpretation logic

## Training Resources Needed
- Example training/validation datasets: Current trainer loop artifacts from existing student agent runs would serve as ground truth for what "good" output looks like
- Evaluation criteria: Judge should assess whether student maintains its scoped role, produces explicitly reasoned output, and appropriately delegates vs. self-implements
- Baseline: Compare optimized student prompt against current version on a dataset of teacher critiques and revision scenarios
