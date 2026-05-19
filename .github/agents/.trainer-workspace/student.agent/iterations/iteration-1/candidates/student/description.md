# Student Candidate Description

**Source**: `iterations/iteration-1/optimize/optimized-prompt.md` (manual_followup optimize stage)

This candidate addresses all six failure modes identified in the engineering review:

1. **FM-1 fix**: Approach step 3 now specifies the default reasoning format by candidate length (sketch-of-thought <80 lines, chain-of-thought ≥80 lines, tree-of-thought for branching tradeoffs). Output format reinforces this.
2. **FM-2 fix**: Constraints and step 6 now require grounding teacher-approval prediction in "at least one specific rubric dimension named in the most recent STEERING.md."
3. **FM-3 fix**: Step 6 adds explicit convergence criterion: "If the current draft addresses all critiques named in the latest STEERING.md and introduces no new constraints, the loop is done."
4. **FM-4 fix**: Body text and step 4 narrow engineer handoff to exactly two conditions: (a) unexplained jargon, (b) cannot rank competing revisions without Trace expertise. Negative rule added: "Do not invoke for routine rewrites."
5. **FM-5 fix**: Step 7 specifies exactly: "Run `python -m pytest -q` from the repository root and report the result, including pass/fail counts."
6. **FM-6 fix**: Step 1 adds iteration-scoping: "Always read from `required_artifacts.latest_iteration_dir`; do not read from a sibling iteration directory." Output format reinforces with "citing the path under `required_artifacts.latest_iteration_dir`."

Teacher verdict (turn 1): All six FM fixes confirmed. Convergent. Apply candidate.
