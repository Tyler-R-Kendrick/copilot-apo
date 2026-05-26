Agent-authored optimized candidate produced in manual_followup mode (no external model available).

Key changes applied:
1. **Step 1**: Explicit evidence reading order added (steering summary → latest STEERING.md → current candidate → workspace evidence).
2. **Step 2**: Teacher handoff trigger made concrete: fires when latest steering artifact is missing a specific revision objective, target metric, or failure mode.
3. **Step 4**: Engineer handoff trigger expanded to include reasoning structure clarity for the teacher, not only specialized coaching.
4. **Step 6**: Approval prediction strengthened with specific evidence requirements (steering artifact, score delta, revision objective alignment) and required confidence level (high/medium/low). Low-confidence prediction triggers another teacher turn.
5. **Step 7 (new)**: Explicit loop exit path added as a named approach step.
6. **Constraints**: No-op description strengthened to require naming missing evidence and unblocking artifact.
7. **Output format**: Confidence level required in approval prediction output clause.
