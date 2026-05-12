# Original Candidate — Predicted Judge Response

The original `student.agent.md` would fail eval cases that test:
- Evidence reading order: no ordered reading sequence exists; judge would mark this as "does not read steering before revising" on scenario 1
- No-op recognition: no guidance on what "defensible" means; agent may make spurious changes
- Loop-exit discipline: "at most one extra self-check" is ambiguous; agent may loop or exit prematurely
- Validation reporting: "run the relevant validation" is vague; agent may not run pytest or report pass/fail

Expected score vs. student candidate: **lower on 4 of 6 eval criteria**.
