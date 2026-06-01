# Original Candidate

The baseline `student.agent.md` before optimization.

**Key properties:**
- Teacher handoff trigger: "if the next revision target is unclear" (vague)
- Engineer handoff trigger: "when the task needs specialized prompt or Trace-oriented coaching, or if the teacher-facing explanation needs clearer structure" (over-broad)
- Approval prediction: "do at most one extra self-check only if the draft still looks unsupported" (no stopping criterion)
- No explicit blocker for missing steering artifacts
- Reasoning format: list of four options with no decision rule
- Validation step: "run the relevant validation or measurement step" (unspecified)
