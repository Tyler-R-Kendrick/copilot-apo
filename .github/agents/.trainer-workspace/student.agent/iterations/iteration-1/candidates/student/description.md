# Student Candidate Description

## Revision Summary

This candidate applies all 8 improvements identified in the engineer-prompt review for `student.agent.md`:

1. **Evidence reading order**: Added a numbered `## Evidence Reading Order` section with six steps and an explicit precedence rule (latest STEERING.md overrides rolling summary.md).
2. **Concrete teacher handoff trigger**: Replaced "if the next revision target is unclear" with three specific conditions: missing objective, contradictory objectives without resolvable precedence, or missing workspace evidence.
3. **Prediction gate with turn cap**: Replaced the single self-check with a stronger gate: predict approval → if disapproval, request one teacher turn → if still disapproved, write blocker and stop. Added a hard cap of 3 teacher handoffs per student turn.
4. **Engineer handoff scope**: Narrowed to "format the reasoning trajectory only" — explicitly excluded coaching on the revision itself.
5. **Updated argument-hint**: Now includes concrete workspace path patterns for the current candidate, latest STEERING.md, and per-agent summary.
6. **No-op artifact spec**: Added four required components: (a) evidence read, (b) revision considered, (c) why not defensible, (d) what teacher must supply next.
7. **Validation step**: Added `python -m pytest -q` with output path `iterations/iteration-N/validation/pytest.txt`.
8. **Engineer handoff prompt updated**: Updated the handoff prompt to say "reformat the reasoning trajectory" not "reformat the solution plan" to align with the tighter scope.

## Predicted Teacher Approval

High confidence. All changes are anchored to failure modes from the review. No scope was added beyond the identified improvements. The frontmatter, tool set, and agent role are unchanged.
