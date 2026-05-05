# Decision: student.agent.md Optimization — Iteration 1

**Date:** 2026-05-05
**Target:** `.github/agents/student.agent.md`
**Workspace:** `.github/agents/.trainer-workspace/student.agent/`
**Optimization mode:** manual_followup (no model API available)

## Winner: Student Candidate

The student candidate (post-teacher-review revision) is selected as the write-back candidate.

## Changes Applied

7 improvements from `engineer-prompt/review.md`, all fully closed after teacher review and student revision:

| # | Gap | Fix |
|---|-----|-----|
| 1 | Evidence reading order undefined | Added explicit priority: STEERING.md → summary → critique → candidate → workspace; blocker if STEERING.md missing |
| 2 | "Defensible revision" undefined | Defined as addressing exactly one named failure mode; back-references in Constraints and Approach |
| 3 | Teacher-approval prediction vague | Must name specific criterion from STEERING.md and confirm observable change |
| 4 | No turn cap | Unified cap: two self-directed revisions without intervening teacher turn → unconditional escalation |
| 5 | Engineer handoff too narrow | Concrete threshold: >3 reasoning steps not traceable to STEERING.md criterion; YAML prompt updated |
| 6 | Validation underspecified | `python -m pytest -q`, report exit code + new failure count |
| 7 | No diff section in Output Format | Before/after diff section added |

## Validation Result

`python -m pytest -q`: exit code 0, 856 passed, 0 new failures.

## Adversary Findings

No exploit beat the student candidate (0.96 vs 0.93 best adversary). Write-back approved.

Adversary identified 3 untested eval boundary cases for future iteration improvement (see `candidates/adversary/findings.md`).

## Next Steps

- Possible future iteration: add 3 boundary-case eval rows to `evals.json` per adversary recommendation
- Rerun `trainer-optimize` with live model credentials when available
