# Decision: student.agent.md Optimization — Iteration 1

**Date:** 2026-05-11  
**Target:** `.github/agents/student.agent.md`  
**Workspace:** `.github/agents/.trainer-workspace/student.agent/`  
**Iteration:** iteration-1

## Selected Candidate

**Student candidate** (trainer-authored `manual_followup` + teacher turn + adversary hardening)

## Changes from Baseline

The optimized candidate addresses all 6 risks identified in `engineer-prompt/review.md` plus one adversary-identified exploit:

| Risk | Change Applied |
|------|----------------|
| Evidence reading order underspecified | Step 1 now reads artifacts in explicit priority: STEERING.md → summary.md → candidate → validation |
| No missing-artifacts fallback | Body intro + Step 1: hand off to teacher immediately if any required artifact is absent or critique is ambiguous |
| Self-check weakly bounded | Exactly one self-check pass; if still negative, emit justified no-op and stop — for any reason |
| Teacher-approval forecast uncriteria'd | Five named criteria added to Constraints and Approach step 6 |
| Engineer handoff trigger vague | Narrowed to: ambiguous/contradictory rationale OR technique unresolvable from context |
| No explicit stopping rule | Justified no-op + trainer recommendation replaces open-ended looping |
| Adversary exploit: conditional stop escape | Added explicit language: "Do not use critique ambiguity as a reason to re-enter the loop after the self-check; ambiguous critique must be resolved before drafting, not after." |

## Validation Result

`python -m pytest -q`: **844 passed, 12 failed** (up from 843 pre-change).  
All 12 failures are pre-existing infrastructure failures (`/.venv/bin/python` absent, shell script environment gaps) unrelated to the student agent optimization.  
The `test_student_agent_contract_structure` test was updated to reflect the improved contract and now passes.

## Optimize Stage

Mode: `manual_followup` (no model credentials in environment).  
Artifact: `iterations/iteration-1/optimize/manual-followup-report.json`  
Trainer-authored candidate: `iterations/iteration-1/optimize/optimized-prompt.md`

## Loop Summary

- **Research**: No public benchmark found; synthesized 7 eval cases + 6 train / 2 val rows from agent contract
- **Optimize**: manual_followup (model unavailable); trainer authored candidate from model_prompt
- **Teacher turn 1**: approved with one refinement — add 5th approval criterion for Output Format completeness
- **Adversary**: identified conditional stop escape exploit (score 0.88 predicted vs 0.82 student); countermeasure applied
- **Final validation**: 844 passed, no new failures

## Write-back

Written to `.github/agents/student.agent.md` ✅
