# Student Candidate Description

The optimized candidate for `student.agent.md`, produced by the `@trainer` agent answering the `manual_followup` `model_prompt` from `trainer-optimize`.

## Improvements Applied

1. **Explicit ordered evidence reading list** (Step 1) — reads in order: source → STEERING.md → summary.md → workspace evidence; blocks on missing STEERING.md
2. **Concrete approval prediction checklist** (Step 6) — 4 observable signals; requires ≥3 met before claiming ready
3. **Tightened engineer handoff trigger** — 2 specific conditions; "do not use for other reasons"
4. **Hard turn cap** — max 2 self-checks without a teacher turn (in Constraints and Step 6)
5. **Named validation command** — `python -m pytest -q`; tracked vs non-tracked cases specified
6. **Missing STEERING.md handling** — explicit "hand off to teacher immediately"

## Teacher Verdict

APPROVE (Turn 1) — all 6 failure modes addressed, minimal and non-regressive.
