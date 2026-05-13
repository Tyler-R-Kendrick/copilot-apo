# Teacher Steering — Turn 1

## Evidence Inspected

- Source: `.github/agents/student.agent.md`
- Engineer-prompt review: `.github/agents/.trainer-workspace/student.agent/engineer-prompt/review.md`
- Train dataset (6 rows), Val dataset (2 rows) — `judge_mode=llm_judge`

## Assessment

The current `student.agent.md` is a well-scoped agent contract with clear role boundaries. The main structural gaps identified in the review are:

1. **No evidence reading order** — the approach lists what to read but not in what order or when to stop.
2. **Vague stopping condition** — "at most one extra self-check if the draft still looks unsupported" gives no concrete criteria for "unsupported."
3. **Undefined "smallest defensible revision"** — the constraint appears twice but has no operational definition.
4. **No conflicting-critique resolution rule** — consecutive contradictory teacher turns have no resolution protocol.
5. **Broad output format options** — five reasoning styles with no guidance on which to prefer.
6. **No missing-evidence blocker path** — missing STEERING.md leads to silent no-op or guessing.
7. **Unclear engineer handoff criterion** — the current description covers "any formatting" which is too broad.

## Predicted Mistakes to Avoid

- Do not add new constraints that are not in the review — scope creep is the most common student mistake.
- Do not change the agent's role, tools, handoff labels, or frontmatter — these are interface elements.
- Do not fold all seven issues into a single dense paragraph — structure each improvement as a distinct addition.
- Do not over-specify the evidence reading order with sub-steps or exceptions at first pass — a simple numbered list is sufficient.

## Revision Guidance

Focus the first candidate on items 1–4 and 6–7 (the six structural gaps). Item 5 (output format) is a minor polish concern and can be addressed alongside items 1–4 in the same pass without expanding scope. The priority order is:
1. Add Evidence Order section (numbered list, stop condition at step 5).
2. Add Stopping Condition section (three explicit criteria).
3. Restate "smallest defensible revision" with an operational definition in Constraints.
4. Add conflicting-critique resolution rule to Constraints.
5. Add missing-evidence blocker path to Constraints and Approach.
6. Clarify engineer handoff criterion in the intro paragraph.
7. Narrow output format default to chain-of-thought in the Output Format section.

## Stop-or-Continue Decision

Continue — the review is strong and the revision hypothesis is clear. A single student turn should produce a defensible candidate for adversary review.

## Judge Notes

Use `judge_mode=llm_judge`. Score on: evidence reading order compliance, stopping condition clarity, scope-exactness of revision, reasoning trajectory visibility, and teacher approval prediction quality.
