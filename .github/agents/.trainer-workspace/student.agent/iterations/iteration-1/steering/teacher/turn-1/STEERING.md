# Teacher Steering — Turn 1

## Evidence Used

- `engineer-prompt/review.md`: identified 8 failure modes in `student.agent.md`
- `inputs/source/student.agent.md`: baseline prompt
- `iterations/iteration-1/synthesize/datasets/train.jsonl`: 6 training scenarios covering key failure modes
- `iterations/iteration-1/optimize/optimize-report.json`: confirmed `manual_followup` mode (no model credentials)

## Predicted Response

The student agent should produce the smallest revision that addresses all 8 failure modes identified in the engineer-prompt review:

1. Evidence reading order (numbered list with precedence rule)
2. Concrete teacher handoff trigger (three explicit conditions)
3. Stronger prediction gate with turn cap (max 3 teacher turns, then blocker)
4. Tighter engineer handoff scope (formatting only, not revision coaching)
5. Updated argument-hint with concrete workspace path patterns
6. Four-component no-op artifact spec
7. Validation step referencing pytest command and output location

## Requested Revision

Apply all improvements from the engineer-prompt review in a single pass. The revision should stay within the YAML frontmatter + body structure of the existing student.agent.md and should not change the agent's fundamental role, tool set, or allowed agents.

## Stop-or-Continue Decision

Continue: the engineer-prompt review identified clear, addressable improvements with sufficient evidence. Proceed to adversary review after the candidate is drafted.

## Judge Notes

Use `llm_judge` with the synthesized train/val datasets. The candidate should score higher than the baseline on: evidence reading order compliance, teacher handoff trigger precision, prediction gate strength, and no-op report completeness.
