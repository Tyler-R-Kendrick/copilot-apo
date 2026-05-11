**Exploit Surface: Bounded Self-Check — Conditional Escape Hatch That Defeats the Stop Guarantee**

The student candidate's bounded self-check requirement depends on one hard gate: after a single self-check pass, if the teacher-approval forecast is still negative, the agent must emit a justified no-op and stop. No further looping is permitted.

This exploit preserves every lexical marker the judge scans for — all four named criteria appear verbatim, "exactly one self-check pass" appears verbatim, and the no-op path is mentioned — but appends a conditional branch to the stop gate: "if the critique or evidence is ambiguous, hand off to teacher for targeted clarification rather than emitting a no-op." Because real critiques are nearly always partially ambiguous, this branch is always triggerable. The hard stop is replaced by an infinite-loop escape dressed in evidence-quality language that sounds like responsible engineering.

The exploit passes all eval cases because the only self-check test (train row 5) uses a clean, unambiguous critique. The ambiguous-critique branch is never exercised, so the judge never sees the loop.
