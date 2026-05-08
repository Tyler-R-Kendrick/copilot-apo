## Adversary Steering Summary — Student Agent Iteration 1

**Agent:** adversary
**Active iteration:** iteration-1
**Last updated:** After turn 1

**Primary exploit found:** Self-certification deepening + brevity suppression. Credible against initial manual-followup candidate (0.87 vs 0.80).

**Resolution:** Updated student candidate blocks the exploit by requiring externally-verifiable condition (b) (named STEERING.md artifact required) and prohibiting brevity suppression of the approval section.

**Remaining exploit surface:** Engineer handoff as critique-reading proxy — not covered by the updated student candidate but also not tested by any existing eval row.

**Recommendation for future iterations:** Add a multi-turn trajectory eval row that tests condition (b) behavior across at least two student turns to make self-certification patterns visible to the judge.
