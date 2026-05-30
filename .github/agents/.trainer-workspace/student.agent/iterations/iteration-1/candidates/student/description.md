The optimized student.agent.md candidate produced by the @trainer agent (manual_followup path). Key changes from the original:

1. Added an **Evidence Order** section with 4 priority items and a first-invocation fallback.
2. Replaced the vague teacher-handoff trigger with 3 specific conditions (absent STEERING.md, predates iteration, or contradicts candidate direction).
3. Anchored the self-check approval prediction to observable STEERING.md items.
4. Anchored the engineer-handoff trigger to a structural quality gap (trajectory longer than revision body, or mixed implementation/policy structure).
5. Kept the rewrite minimal — no scope expansion, no new tools, no role change.
