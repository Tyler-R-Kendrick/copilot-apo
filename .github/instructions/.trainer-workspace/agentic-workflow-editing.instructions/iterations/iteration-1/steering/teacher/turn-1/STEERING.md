# Turn 1 — Teacher Steering

**Decision**: Continue — two targeted additions required. Do not restructure.

## Evidence Used

- Current instructions file (4 bullets, original)
- Engineer-prompt/review.md (6 failure modes documented)
- Training dataset (8 rows, llm_judge scoring)
- Validation dataset (4 rows, llm_judge scoring)

## Predicted Student Response

The student will add an explicit multi-workflow compile rule and explicit `git add` instruction. Likely the student will add these as new bullets rather than integrating into existing bullets. Predicted outcome: 6-bullet file with all failure modes covered.

## Requested Revision

**1. Multi-workflow rule (FM5 — highest priority)**
Add guidance that when editing multiple workflow sources, each must be compiled separately. Frame as a standing rule, not a conditional exception.

**2. Explicit `git add` in verification step (FM3)**
Make it clear the agent must explicitly stage the lockfile (`git add`) — "include in the change set" is too vague.

## What NOT to Change

- Bullets covering hook backstop and compile-failure stop — correct, leave untouched
- The "including minor formatting or comment changes" phrasing
- The frontmatter (description and applyTo)
- The explicit pre-PR compile mention

## Exit Criteria

Approved when:
- Multi-workflow compile stated as per-file rule
- `git add <lockfile>` explicit in at least one bullet
- Original hook-backstop and compile-failure bullets unchanged
- File stays ≤ 6 bullets
