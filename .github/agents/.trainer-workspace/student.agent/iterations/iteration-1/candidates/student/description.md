# Student Candidate (Optimized)

The optimized `student.agent.md` produced by the trainer manual-followup pass.

**Key improvements over original:**
1. Teacher handoff trigger is now concrete: three named conditions (no STEERING.md, contradictory critique, uninferrable goal) instead of vague "if unclear"
2. Engineer handoff restricted to formatting-only: fires only when plan is complete and explanation needs restructuring, not for domain advice
3. Approval prediction formalized as two-step rule: predict → if clear yes proceed; if uncertain, one self-check → if clear yes proceed; else teacher turn and stop
4. Reasoning format has a decision rule: sketch for small focused revisions, chain-of-thought for sequential, tree-of-thought for branching, chain-of-uncertainty for missing-info cases
5. Validation step specifies `python -m pytest -q` with exit code and summary line recording

**Preservation:**
- All frontmatter fields, handoff labels, and prompts unchanged
- Constraint list unchanged except removal of ambiguous "pre-emptively predict" phrasing (replaced with concrete rule in Approach)
- Output format section updated to reference the new validation step spec
