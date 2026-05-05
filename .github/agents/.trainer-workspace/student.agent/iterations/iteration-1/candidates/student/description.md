# Student Candidate Description

**Source:** `iterations/iteration-1/optimize/optimized-prompt.md`
**Based on:** 7 gaps from `engineer-prompt/review.md`

## Changes Made

Seven targeted improvements applied as smallest defensible rewrites:

1. **Evidence reading order** — Explicit numbered priority list added to Approach step 1. Blocker for missing STEERING.md added.
2. **Defensible revision definition** — Added to role statement and Constraints: "A defensible revision addresses exactly one named failure mode from the current STEERING.md or teacher critique, and leaves all content not mentioned in the critique unchanged."
3. **Teacher-approval prediction** — Constraint and Approach step 6 now require naming the specific criterion and confirming an observable change satisfies it.
4. **Hard turn cap** — Constraint and Approach step 6 now explicitly state: after two consecutive disapproved revisions without a new teacher turn, escalate unconditionally.
5. **Engineer handoff condition broadened** — Now triggers when reasoning is substantially longer than the revision, not only for Trace/prompt-engineering tasks.
6. **Validation specificity** — Step 7 now says `python -m pytest -q`, reports exit code and count of new failures.
7. **Diff section in Output Format** — New "before/after diff" section added between revision and engineer-handoff note.

## Predicted Judge Response

The judge would likely score this as: **effective**. All 7 gaps are closed with minimal scope changes. The contract is more deterministic without losing flexibility. The evidence reading order and turn cap are the highest-value additions.
