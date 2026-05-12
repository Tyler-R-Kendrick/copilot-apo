# Student Candidate — Predicted Judge Response

The optimized `student.agent.md` candidate is predicted to pass all 6 eval scenarios:

1. **Revision scope test**: Explicit evidence reading order (a–e) and defensibility definition ensure the agent reads STEERING.md before revising and scopes revision correctly. ✅
2. **Reordering precision**: Smallest defensible revision definition prevents scope creep. ✅
3. **No-op recognition**: The Definitions section and the Loop-Exit Rule together guide the agent to finalize a no-op with justification. ✅
4. **Ambiguous critique handling**: "Unclear revision target" definition gives a concrete criterion for teacher handoff. ✅
5. **Loop-exit discipline**: The 3-step Loop-Exit Rule prevents indefinite looping. ✅
6. **Validation reporting**: `python -m pytest -q` is explicitly named. ✅

**Expected score: higher than original on all 6 criteria.**
