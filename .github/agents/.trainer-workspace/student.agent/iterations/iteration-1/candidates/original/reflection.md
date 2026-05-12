# Original Candidate — Reflection

The original candidate is a clean, well-scoped agent contract. Its main limitation is the absence of structural rules that the trainer-loop context requires:
- No reading-order discipline
- No defensibility definition
- No loop-exit rule
- No format selection guidance

These are not fundamental design flaws — they are gaps that emerge when the agent needs to operate reliably in a bounded multi-turn loop with explicit workspace artifacts. The improvements in the student candidate add structure without changing the interface or expanding scope.
