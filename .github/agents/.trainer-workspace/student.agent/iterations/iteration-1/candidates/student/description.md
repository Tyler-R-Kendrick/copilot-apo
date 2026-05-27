# Student (Optimized) Candidate Description

This is the iteration-1 optimized candidate produced via manual-followup APO by the `@trainer` agent.

**Improvements over baseline (all six engineer-prompt risks resolved):**
1. Added explicit evidence reading order (Approach step 1)
2. Added three operational teacher handoff conditions replacing vague language
3. Engineer handoff scoped to output-formatting only
4. Added loop exit rule with three concrete conditions
5. Added workspace output step (Approach step 7: write `steering/student/turn-N/STEERING.md`)
6. Added conflict resolution rule (latest STEERING.md is authoritative over summary.md)
7. Bonus: added unbundling rule (Approach step 5)

**Teacher verdict**: APPROVE for write-back (turn-1 STEERING.md)

**Predicted eval performance**: Covers all six training cases plus the unbundling case.
