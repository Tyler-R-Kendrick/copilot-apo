# Decision — student.agent.md

**Target:** `.github/agents/student.agent.md`
**Workspace:** `.github/agents/.trainer-workspace/student.agent/`
**Iteration:** iteration-1
**Date:** 2026-05-16

## Decision: Apply Student Candidate

The optimized student candidate from iteration-1 was applied to `.github/agents/student.agent.md`.

### Changes Applied

Seven surgical in-place edits addressed all engineer-identified gaps plus one adversary-identified hardening:

1. **Step 1a added** — staleness check before drafting; hands off to teacher if critique pre-dates the latest steering artifact.
2. **"Defensible" defined inline** — `(defensible = grounded in the current teacher critique, within scope of the optimization goal stated in the workspace, and verifiable by running repository validation)`.
3. **Step 5a added** — STEERING.md creation guidance: write `iterations/iteration-N/steering/student/turn-N/STEERING.md` and update rolling `summary.md`.
4. **Loop-exit tightened** — four-condition decision table replacing the open-ended "at most one self-check" rule.
5. **Engineer handoff split** — Case A (technical expertise) and Case B (explanation structure) named explicitly; both may apply.
6. **Concrete validation command** — `python -m pytest -q` cited in step 7.
7. **Output format updated** — STEERING.md artifact bullet added (path + key content summary).
8. **Validation floor** *(adversary hardening)* — `no tests ran` is not a passing result for a substantive revision.

### Validation Result

`python -m pytest -q`: **856 passed** in ~8s. No regressions.

### Why This Candidate Was Chosen

- All 7 original gaps closed with minimal structural changes.
- Frontmatter, role sentence, job-description paragraph, and all unaffected constraint bullets unchanged.
- Adversary found 3 credible exploits; exploit 1 and 2 (validation theater) addressed. Exploit 3 (turn cap self-declaration) partially mitigated by teacher steering.
- Teacher steering confirmed all 7 changes are necessary and correctly scoped.

### Test File Update

`tests/test_customizations.py` line 268 updated to match the new engineer handoff wording (`'Use the `engineer` handoff in one of two named cases'`).
