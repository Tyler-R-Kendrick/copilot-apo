# Original Candidate Description

**Source:** `.github/agents/student.agent.md` (baseline, unmodified)

**Key characteristics:**
- No explicit evidence reading order
- Ambiguous engineer handoff trigger ("needs specialized prompt or Trace-oriented coaching")
- Underspecified validation step ("Run the relevant validation or measurement step")
- Missing loop-exit criteria from the student's perspective
- No fallback for absent STEERING.md artifacts
- No-op output format not specified

**Predicted judge response:** The original scores lower on behavioral protocol compliance tasks because it leaves evidence reading order, engineer handoff triggers, and loop-exit behavior undefined. Agents following this contract may skip reading STEERING.md, invoke engineer too often, and produce inconsistent no-op artifacts.
