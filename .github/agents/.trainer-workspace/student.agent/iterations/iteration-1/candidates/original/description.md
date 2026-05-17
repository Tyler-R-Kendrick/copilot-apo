## Original Candidate

**Source**: `.github/agents/student.agent.md` (unmodified)

**Description**: The baseline student agent before this optimization iteration. Contains the six weaknesses identified in the engineer-prompt review: no evidence reading order, weak approval prediction, imprecise teacher handoff trigger, unclear engineer distinction, verbose reasoning format list, and no loop-exit stopping rule.

**Expected judge assessment**: Scores lower on revision precision (no "change only the targeted section" constraint), loop-exit compliance (no cap), and teacher handoff trigger observability (vague "if unclear" condition).
