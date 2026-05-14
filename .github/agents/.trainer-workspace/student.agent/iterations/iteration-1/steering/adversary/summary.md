# Adversary Steering Summary — student.agent.md, iteration-1

## Turn 1 (2026-05-14)

Stress-tested intermediate candidate (pre-teacher-correction). Found 3 exploits:
- **Exploit 1 (credible):** Orchestration prohibition moved to preamble + stale trigger weakened. This exactly matches the teacher's blocking regression. Already fixed before write-back.
- **Exploit 2:** Execute scope bypass through verbose justification. Mitigated by current constraint but llm_judge may not catch semantic bypass.
- **Exploit 3:** Compound exploit combines Exploit 1 + Exploit 2 for ~0.88 judge score.

**Net result:** Current written-back candidate is defended against the primary exploit. Future judge rubric improvement recommended for trigger boundary correctness checking.
