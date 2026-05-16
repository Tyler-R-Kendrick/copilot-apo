# Teacher Steering Summary — student.agent — Iteration 1

## Turn 1

**Goal:** First optimization candidate. All 7 engineer-identified risks targeted in a single minimal-rewrite pass.

**Seven surgical in-place edits** to Approach, Constraints, and Output Format sections only. Frontmatter and preamble frozen.

**Primary failure risks:**
- Forgetting the output-format bullet (risk 7, listed last)
- Adding "defensible" as a new constraint bullet instead of inline parenthetical
- Hard-coding the turn cap in step 6 condition (d)
- Writing vague validation language instead of `python -m pytest -q`

**Validation:** `python -m pytest -q` from repo root, no new failures expected.

**Stop condition:** All 7 changes present and scoped; teacher approval predicted.
