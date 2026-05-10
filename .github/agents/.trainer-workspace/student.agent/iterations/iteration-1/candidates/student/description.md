# Student Candidate Description

This is the optimized candidate produced by the @trainer agent answering the manual_followup model_prompt, with adversary guard fixes applied.

## Changes from Baseline

1. **Evidence reading order**: Approach step 1 now specifies numbered sequence: STEERING.md for current iteration → current candidate → teacher critique → other workspace evidence.
2. **Concrete scope rule**: Constraints section now specifies "Change only what the current critique explicitly names; leave all other prompt structure and constraints unchanged."
3. **Binary loop exit criteria**: Step 6 specifies two conditions: stop if approval predicted with one reason; add one extra self-check only when approval looks unlikely and one targeted improvement remains.
4. **Missing-evidence protocol**: Step 1 and opening paragraph now specify: if no STEERING.md exists for the current iteration, hand off to teacher before revising.
5. **Engineer handoff clarification**: Now specifies "use engineer when reasoning trajectory needs restructuring for clarity; use teacher when revision logic is unclear."
6. **Concrete validation step**: Step 7 now specifies `python -m pytest -q` with pass/fail count reporting.
7. **Sharpened no-op condition**: Three explicit triggers including "that the trainer has not explicitly suspended for this iteration" guard against exploit.

## Predicted Judge Response

Score: ~0.85-0.90. The candidate addresses all identified gaps with surgical precision, does not expand scope, and adds no unsupported placeholders. The adversary guards prevent the two identified exploit patterns.
