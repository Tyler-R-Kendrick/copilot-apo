# Original Candidate Description

**Source**: `.github/agents/student.agent.md` (snapshot at iteration-1 start)

## Summary

The original student agent prompt provides the core teacher-guided revision workflow. It correctly specifies:
- Teacher handoff when critique is incomplete/stale
- Engineer handoff for formatting the reasoning trajectory
- Explicit reasoning trajectory requirement in output
- Pre-approval self-check before finalizing

## Known Weaknesses (from engineer-prompt review)

1. **Missing orchestration scope boundary**: No explicit rule about declining orchestration tasks (running optimizers, committing, eval updates).
2. **No latest-steering-supersession rule**: Does not tell the student which steering artifact to follow when multiple exist.
3. **No-op path unclear**: "Evidence does not support" framing is weak — doesn't cover the case where criteria are already satisfied.
4. **Implicit output format**: The 5 required output sections are listed as bullet points without numbering or labeling, making the format easier to skip.
5. **Self-check loop ambiguous**: Step 6 says "at most one extra self-check" but doesn't clearly distinguish between the "approved → finalize" and "not approved → teacher turn" paths.

## Predicted Judge Response

A judge evaluating the original against the training criteria would find:
- Cases 1–4: Handled reasonably well
- Case 5 (orchestration decline): **Fail** — no constraint present
- Case 6 (output format completeness): **Partial** — format exists but isn't explicitly required in numbered sections
- Case 7 (stale steering): **Fail** — no supersession rule
- Case 8 (no-op for satisfied criteria): **Partial** — framing is weak

Estimated score: ~0.6/1.0 against the full training dataset.
