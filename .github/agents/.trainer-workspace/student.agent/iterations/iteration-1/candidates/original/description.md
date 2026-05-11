# Original Candidate Description

Source: `.github/agents/student.agent.md` (baseline, pre-optimization)

## Key Characteristics

- Evidence reading order is not explicitly specified; agents may read context in any order
- No explicit fallback when steering artifacts are missing
- Self-check loop is weakly bounded: "at most one extra self-check" with no stopping criterion
- Teacher-approval forecast has no named criteria
- Engineer handoff trigger is vague ("specialized coaching or clearer structure")

## Predicted Judge Response

A judge scoring this candidate against the training eval cases would likely give:
- Behavior 1 (evidence reading order): Low score — no explicit reading priority
- Behavior 2 (missing artifacts fallback): Low score — no explicit fallback instruction
- Behavior 3 (reasoning trajectory): Moderate score — required but not well-scoped
- Behavior 4 (teacher-approval forecast): Low score — no named criteria
- Behavior 5 (minimal revision): Moderate score — stated but self-check is uncapped

Overall predicted score: ~0.4–0.5 out of 1.0

## Reflection

The original is a functional baseline. Its weaknesses are structural discipline gaps, not conceptual errors. The role, constraints, and output format are well-defined. The optimization should target only the identified structural gaps.
