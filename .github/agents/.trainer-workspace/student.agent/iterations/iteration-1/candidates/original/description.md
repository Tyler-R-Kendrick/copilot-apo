# Original Candidate Description

This is the baseline `student.agent.md` before optimization.

**Key weaknesses identified by engineer-prompt review:**
- No explicit evidence reading order
- Vague teacher handoff conditions ("incomplete, contradictory, stale")
- Ambiguous engineer handoff scope
- No workspace output responsibility (no steering artifact written per turn)
- Loop exit condition weakly specified ("at most one extra self-check")
- No conflict resolution rule for contradicting steering turns

**Predicted eval performance**: Struggles on cases 2 (vague critique), 3 (conflict resolution), 4 (loop exit), 5 (engineer handoff scope), 6 (unbundling).
