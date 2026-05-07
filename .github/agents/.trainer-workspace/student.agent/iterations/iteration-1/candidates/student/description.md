# Student Candidate Description

## What Changed and Why

The optimized student candidate addresses six structural weaknesses identified in `engineer-prompt/review.md`:

### 1. Explicit Evidence Reading Order
**Before:** "Read the teacher goal, latest teacher critique, current teacher turn STEERING.md, the relevant per-agent summary.md files in the active iteration, and the current workspace evidence."
**After:** A numbered 5-step sequence (teacher goal → STEERING.md → summary.md → candidate → workspace evidence) with a conflict resolution rule.
**Why:** Removes ambiguity in artifact reading order, ensuring teacher goals are never overridden by lower-priority workspace evidence.

### 2. Concrete Handoff Conditions
**Before:** "Use teacher handoff whenever the critique is incomplete, contradictory, stale..." / "Use engineer handoff when the task needs specialized coaching..."
**After:** Checkable signals — teacher handoff when STEERING.md is absent/outdated/blocked; engineer handoff when trajectory exceeds three paragraphs or two technical concepts.
**Why:** Reduces false-positive handoffs triggered by mild ambiguity.

### 3. Named Convergence Stopping Conditions
**Before:** "at most one extra self-check; if approval still looks unlikely, justify..."
**After:** Explicit stopping conditions: "All BLOCKER flags resolved; loop complete" or "Teacher approval received; loop complete."
**Why:** Prevents indefinite looping and makes the teacher's job easier by naming the terminal state clearly.

### 4. Structured Artifact Contract
**Before:** Output format listed fields with no content requirements.
**After:** Reasoning trajectory steps must include step name, evidence consulted, conclusion, and uncertainty level. No-ops must quote specific BLOCKER flags. Revisions must include before/after diffs.
**Why:** Ensures artifact completeness and reduces shallow one-line outputs.

### 5. Trainer-Specific Focus Areas
**Before:** No mention of scoring modes, dataset shapes, or workspace contract.
**After:** Constraint added requiring checks on scoring mode awareness, dataset shape compliance, workspace staging contract, and authored-eval vs. synthesized-dataset distinction.
**Why:** Surfaces trainer-specific gaps that are the most common revision failure modes in this repo.

### 6. Minimum Revision Depth + No-op Threshold
**Before:** "Implement the smallest defensible candidate revision."
**After:** "A revision must change at least one instruction, constraint, or output requirement; otherwise submit a justified no-op citing a specific BLOCKER."
**Why:** Prevents trivial single-word changes from being accepted as complete revisions.
