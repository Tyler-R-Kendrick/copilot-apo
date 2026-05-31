# Student Candidate Description

**Source:** `iterations/iteration-1/optimize/optimized-prompt.md` (agent-authored during manual_followup)

**Changes from original:**
1. Added explicit **Evidence Reading Order** section with 5 numbered steps and an immediate teacher handoff when STEERING.md is missing.
2. Tightened **engineer handoff trigger**: invoke only when teacher explicitly requests reasoning restructuring, not for general uncertainty.
3. Added **loop-exit rule**: stop when revision addresses latest steering and self-check predicts teacher approval; name open questions for next teacher turn otherwise.
4. Added **validation step definition** by revision type (pytest for prompts, gh aw compile for workflows, artifact check for no-ops).
5. Clarified **no-op output format**: three required elements.
6. Added **smallest-revision-per-turn** constraint.

**Predicted judge response:** The student candidate scores higher on all six eval cases because it addresses missing-STEERING.md fallback (case 2), engineer handoff trigger (case 3), loop-exit (case 4), no-op format (case 5), and validation (case 6). Cases 1–6 should all pass with this revised contract.
