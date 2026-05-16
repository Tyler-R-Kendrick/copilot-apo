## Teacher Steering — student.agent — Turn 1

**Iteration:** 1  
**Target file:** `.github/agents/student.agent.md`  
**Prior evidence:** engineer-prompt/review.md (7 risks), synthesize/evals/evals.json (5 evals), workflow-status.json

### Revision Objective
Produce the first optimization candidate addressing all 7 engineer-identified risks in a single minimal-rewrite pass.
Touch ONLY the seven listed locations; leave frontmatter, role sentence, job-description paragraph, "Treat turn-scoped" sentence, and all unaffected constraint bullets unchanged.

### Required Changes (in order)
1. **Approach Step 1 → add Step 1a** — staleness check: verify the critique references the current iteration; if not, hand off to teacher before drafting.
2. **Constraints line 28** — extend inline: append `(defensible = grounded in the current teacher critique, within scope of the optimization goal stated in the workspace, and verifiable by running repository validation)` to the existing bullet. No new bullet.
3. **Approach after Step 5 → add Step 5a** — STEERING.md creation: write `iterations/iteration-N/steering/student/turn-N/STEERING.md` (critique read, plan, revision, approval prediction); update rolling `summary.md`.
4. **Approach Step 6** — replace "at most one extra self-check" with a four-condition stop-or-continue decision table: stop on (a) predicted teacher approval, (b) explicit teacher "no further revision", (c) justified no-op, (d) iteration turn cap as defined by the active trainer run. Request teacher turn when draft still diverges after one self-check.
5. **Body paragraph 2 (engineer handoff)** — split into two named cases: Case A (technical expertise), Case B (explanation structure); note both may apply together.
6. **Approach Step 7** — add concrete example: `python -m pytest -q` or the active target's eval command.
7. **Output Format — final bullet** — add: state the STEERING.md artifact written (path + key content summary) and the summary.md section updated.

### What Teacher Approval Looks Like
- All 7 risks closed, no structural rearrangement of unrelated sections.
- "Defensible" is an inline parenthetical on its existing bullet — not a new bullet.
- Step 5a is a sub-step, not a new section.
- Loop-exit table has exactly four named conditions.
- Engineer handoff paragraph names exactly two cases (Case A, Case B).
- Step 7 cites `python -m pytest -q` verbatim.
- Output format gains one new bullet about STEERING.md/summary.md (path + key content summary).
- `python -m pytest -q` from repo root passes with no new failures.

### Forecasted Student Failure Modes to Avoid
- Do not rewrite the preamble or role sentence.
- Do not add a standalone "Defensible means:" bullet.
- Do not insert a new "## Workspace Artifacts" section.
- Do not add more than 4 rows to the loop-exit table.
- Do not write vague validation language ("run tests") — use the exact command.
- Do not forget the output-format bullet (priority 7) — it is last but required.
- Quote `iterations/iteration-N/steering/student/turn-N/STEERING.md` verbatim from the artifact_contract.
- Write "iteration turn cap as defined by the active trainer run" for condition (d) — do not hard-code a number.
