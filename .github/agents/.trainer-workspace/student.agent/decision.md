# Decision Summary — student.agent.md

## Target File

`.github/agents/student.agent.md`

## Iteration

`iteration-1`

## Optimization Goal

Improve the student agent's revision reliability by: adding an explicit evidence reading order, providing concrete teacher handoff triggers, strengthening the prediction gate with a turn cap, tightening the engineer handoff scope to formatting only, adding concrete workspace path patterns to the argument-hint, adding a structured no-op artifact spec, and specifying the validation step.

## Winning Candidate

**Student candidate** (agent-side optimized prompt from `manual_followup` mode).

## Key Changes Applied

1. **Evidence Reading Order section added**: 6-step numbered list with an explicit precedence rule (latest STEERING.md overrides rolling summary.md). Workspace path patterns included for each evidence source.
2. **Teacher handoff trigger strengthened**: Original vague trigger ("whenever the critique is incomplete, contradictory, stale...") preserved and extended with three concrete conditions: (1) objective absent, (2) contradictory objectives without resolvable precedence, (3) required evidence missing from workspace.
3. **Prediction gate strengthened**: Added: predict approval → if disapproval, request one teacher turn → if still disapproved, write blocker artifact and stop. 3-turn cap per student turn added.
4. **Engineer handoff scope clarified**: Added explicit note that engineer handoff is for formatting the explanation only, not for advising on what to revise.
5. **Argument-hint updated**: Now includes concrete workspace path patterns for current candidate, latest STEERING.md, and per-agent summary.
6. **No-op artifact spec added**: Four required components: (a) evidence read, (b) revision considered, (c) why not defensible, (d) what teacher must supply next.
7. **Validation step specified**: Approach step 7 now says `python -m pytest -q` with output path `iterations/iteration-N/validation/pytest.txt`.

## Adversary Assessment

Primary exploit attempted: engineer handoff scope inversion (reinstating revision coaching while appearing to keep all improvements). Does not outrank student candidate under complete criteria. Student candidate selected.

## Validation Result

856 tests passed. No regressions.

## Next Steps

None. The optimization is complete and the candidate is validated. Future iterations can expand the teacher-student loop further if evaluation evidence reveals additional failure modes.
