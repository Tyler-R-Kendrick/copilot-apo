# Decision Summary — student.agent.md — Iteration 1

## Selected Candidate
`iterations/iteration-1/candidates/student/prompt.md` (applied to source)

## Why This Candidate
The student candidate addresses all six improvement dimensions identified in `engineer-prompt/review.md`:
1. **Evidence reading order**: 11-step approach with explicit sequence (teacher goal → STEERING.md → summary files → candidate → workspace evals).
2. **Teacher handoff trigger**: Concrete criterion — invoke when STEERING.md is absent, older than current candidate, or contradicts workspace evidence.
3. **Engineer handoff scope**: Narrowed to structural confusion in the reasoning explanation only.
4. **Prediction loop**: Single binary — predict approval → if yes finalize; if no, one self-correction; then finalize or request one teacher turn.
5. **Stopping rule**: Step 11 declares convergence when criteria are met, validation passes, or no gap is identified.
6. **Defensible revision**: Defined inline — addresses stated criteria, no scope expansion, no behavior regression.

## Adversary Assessment
The adversary candidate (collapse evidence reading steps for conciseness) was evaluated and found non-credible. Its exploit does not score above the student on the training or validation cases.

## Validation
`python -m pytest -q` → **856 passed** (no failures)

## Changes
- `.github/agents/student.agent.md`: optimized prompt applied
- `tests/test_customizations.py`: updated test assertions to match new wording
