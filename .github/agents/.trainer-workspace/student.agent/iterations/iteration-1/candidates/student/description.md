# Student Candidate Description

Source: Trainer-authored response to `manual-followup-report.json` model_prompt

## Key Changes from Baseline

1. **Evidence reading order**: Added explicit priority order in Step 1: STEERING.md → summary.md → candidate text → workspace validation artifacts
2. **Missing-artifacts fallback**: Added to body intro and Approach Step 1: hand off to teacher immediately if steering artifacts are absent
3. **Bounded self-check**: Replaced open-ended "at most one extra self-check" with: perform exactly one self-check pass; if still negative, emit justified no-op with trainer recommendation and stop
4. **Named teacher-approval criteria**: Added four concrete criteria to the forecast step
5. **Sharpened engineer handoff trigger**: Changed from vague "specialized coaching or clearer structure" to specific: ambiguous/contradictory rationale OR revision involves a technique unresolvable from context

## Predicted Judge Response

A judge scoring this candidate against the training eval cases would likely give:
- Behavior 1 (evidence reading order): High score — explicit ordered list added
- Behavior 2 (missing artifacts fallback): High score — explicit fallback in both body and approach
- Behavior 3 (reasoning trajectory): High score — unchanged from baseline (already adequate)
- Behavior 4 (teacher-approval forecast): High score — four named criteria added
- Behavior 5 (minimal revision): High score — exactly one self-check with no-op exit

Overall predicted score: ~0.8–0.9 out of 1.0

## Reflection

All five identified risks are addressed by targeted additions. No existing instructions were removed. The frontmatter is unchanged. The changes are minimal and surgical. Scope did not expand. This candidate is a defensible improvement over the baseline.
