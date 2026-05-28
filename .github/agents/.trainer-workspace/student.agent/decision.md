# Decision: student.agent.md Optimization — Iteration 1

## Result

**Candidate applied:** `iterations/iteration-1/candidates/student/candidate.md`
**Validation:** 856 tests passed, 0 failed

## Changes Applied

Six targeted structural improvements were made to `.github/agents/student.agent.md`:

1. **Numbered evidence reading order** — Approach step 1 now specifies five artifacts to read in priority order with an explicit "stop and plan" instruction.
2. **Teacher-approval prediction rubric** — Three observable criteria inline: (a) revision addresses specific critique without new scope; (b) predicted response aligns with request; (c) no new constraint introduced.
3. **Tightened engineer handoff condition** — Replaced vague "when the task needs prompt-engineering coaching" with two concrete triggers: domain-specific claims the student cannot justify, or explanation that needs structural shortening.
4. **Explicit validation step** — Approach step 7 now says to run `python -m pytest -q` when a tracked file is touched; report "validation skipped — draft candidate only" otherwise.
5. **Blocker report format** — After one failed teacher handoff, write a blocker note under the active STEERING.md and stop.
6. **Loop-escalation rule** — After two consecutive unresolved student turns, escalate to the trainer with a summary.

## Optimize Stage

Mode: `manual_followup` (agentlightning not installed in environment).
Report: `iterations/iteration-1/optimize/manual-followup-report.json`
Candidate: produced by `@trainer` agent inference step.

## Next Steps

A live automated rerun with model credentials is optional. The candidate is validated and applied.
