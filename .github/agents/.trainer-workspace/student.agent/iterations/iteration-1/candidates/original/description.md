# Original Candidate Description

**Source**: `.github/agents/student.agent.md` (unmodified baseline)

This is the original student agent before any optimization. It defines the student role correctly but has six identified failure modes from the engineering review:

1. Reasoning trajectory format is underspecified (any format or none)
2. Teacher-approval prediction is binary and ungrounded (no rubric citation required)
3. No explicit convergence signal (when is the loop done?)
4. Engineer handoff trigger is overly broad ("specialized prompt or Trace-oriented coaching")
5. Validation step is underspecified ("run the relevant validation step")
6. Steering artifact navigation is passive (no iteration-scoping rule)
