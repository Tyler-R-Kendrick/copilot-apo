# decision.md

## Selected Target

**File**: `.github/agents/student.agent.md`
**Workspace**: `.github/agents/.trainer-workspace/student.agent/`
**Selection reason**: First `.agent.md` file (alphabetically by repo-relative path) without an existing trainer workspace.

## Optimization Summary

**Iteration**: iteration-1
**Optimize mode**: `manual_followup` (no model credentials available; @trainer agent answered model_prompt)

### Changes Applied

1. **Evidence reading order**: Approach step 1 now specifies a numbered sequence: STEERING.md for current iteration → current candidate → teacher critique → other workspace evidence.
2. **Concrete scope rule**: Replaced vague "smallest defensible revision" constraint with "Change only what the current critique explicitly names; leave all other prompt structure and constraints unchanged."
3. **Binary loop exit criteria**: Step 6 now specifies two explicit conditions: stop if approval predicted with one concrete reason; add one extra self-check only when approval looks unlikely and one targeted improvement remains.
4. **Missing-evidence protocol**: Opening paragraph and Step 1 now specify: if no STEERING.md exists for the current iteration, hand off to teacher before revising.
5. **Engineer handoff clarification**: Now specifies "use engineer when the reasoning trajectory itself needs restructuring for clarity so the teacher can follow it; use teacher instead when the revision logic itself is unclear."
6. **Concrete validation step**: Step 7 now specifies `python -m pytest -q` from repo root with pass/fail count reporting.
7. **Sharpened no-op condition**: Three explicit triggers with adversary guard: "that the trainer has not explicitly suspended for this iteration."

### Adversary Findings

Two medium-severity exploits were found and closed:
1. "Absent" STEERING.md ambiguity → guard added: "no STEERING.md exists for the current iteration."
2. No-op third trigger ("cannot be waived") → guard added: "that the trainer has not explicitly suspended for this iteration."

## Validation Result

`python -m pytest -q`: **856 passed** (0 failed)

## Artifacts

- `engineer-prompt/review.md`: Engineering review
- `iterations/iteration-1/research/research-brief.md`: Research brief
- `iterations/iteration-1/synthesize/datasets/train.jsonl`: 6 training rows
- `iterations/iteration-1/synthesize/datasets/val.jsonl`: 3 validation rows
- `iterations/iteration-1/synthesize/evals/evals.json`: 5 eval cases
- `iterations/iteration-1/optimize/manual-followup-report.json`: Optimize artifact
- `iterations/iteration-1/optimize/optimized-prompt.md`: Candidate (with adversary guards)
- `iterations/iteration-1/optimize/operator-followup.md`: Handoff summary
- `iterations/iteration-1/steering/teacher/turn-1/STEERING.md`: Teacher turn-1 steering
- `iterations/iteration-1/steering/adversary/turn-1/STEERING.md`: Adversary turn-1 findings
- `iterations/iteration-1/validation/pytest.txt`: Validation log
- `iterations/iteration-1/candidates/candidates.json`: Candidate manifest
