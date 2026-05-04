# Decision Summary — student.agent.md — Iteration 1

## Target

- **File**: `.github/agents/student.agent.md`
- **Workspace**: `.github/agents/.trainer-workspace/student.agent/`
- **Goal**: Improve the student agent contract for tighter revision scope control, clearer loop termination, and reliable section-format compliance

## Winning Candidate

**`iterations/iteration-1/candidates/student/student.agent.md`** (also at `iterations/iteration-1/optimize/optimized-prompt.md`)

## Why This Candidate Was Selected

The optimized candidate addresses all 8 identified failure modes from the engineer-prompt review:

| # | Failure Mode | Fix Applied |
|---|-------------|-------------|
| 1 | Over-revision (adding content beyond critique) | "Revise only the elements the critique identified" + explicit minimum-change constraint |
| 2 | Hidden reasoning trajectory | Mandatory 5-section output format with explicit `REASONING TRAJECTORY` section |
| 3 | Stale guidance in iterative loop | "If revising in an iterative loop, read the latest steering turn first" |
| 4 | Loop avoidance (skipping teacher turn when uncertain) | "When uncertain whether teacher approval is forthcoming, always complete a turn" |
| 5 | Misrouted handoffs | Explicit table of signals mapped to actions (trainer→teacher, engineer→formatting) |
| 6 | Validation skip (outputting revised prompt without drafting reasoning) | Section 3 (`PREDICTED TEACHER RESPONSE`) blocks premature commitment |
| 7 | Orchestration task acceptance | Positive routing: "redirect the request to the trainer and explain the scope boundary" |
| 8 | Section format non-compliance | "Provide exactly these five sections in order" |

One teacher micro-improvement was applied: changed orchestration-decline constraint from prohibition to positive routing (step 7, approach section).

## Adversary Review Result

Three compounding scope-regression exploits were identified. All three were rejected. The student candidate's wording was already resistant to all exploits:
- Engineer handoff at step 5 (post-draft, not pre-draft)
- "exactly these five sections" (not "at minimum")
- "redirect to trainer" (unconditional, not "when ambiguous")

## Validation

`python -m pytest -q` — **856 tests passed, 0 failures**

See `iterations/iteration-1/validation/pytest.txt` for full output.

## Dataset Used

- `judge_mode`: `llm_judge`
- Train: `iterations/iteration-1/synthesize/datasets/train.jsonl` (8 rows)
- Val: `iterations/iteration-1/synthesize/datasets/val.jsonl` (4 rows)
- Evals: `iterations/iteration-1/synthesize/evals/evals.json` (8 cases)

## Optimize Stage

The optimizer ran in `manual_followup` mode (no `.env` model credentials in this runner environment). The `@trainer` agent completed the optimize stage by answering the `model_prompt` directly and saving the result as `optimized-prompt.md`. See `iterations/iteration-1/optimize/manual-followup-report.json` for the payload.

## Write-back Decision

**APPROVED**: Apply `iterations/iteration-1/optimize/optimized-prompt.md` to `.github/agents/student.agent.md`.
