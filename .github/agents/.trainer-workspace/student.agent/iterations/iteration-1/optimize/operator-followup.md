# Operator Followup: student.agent.md Optimization

## Blocker
The `agent-skills` MCP server at `http://host.docker.internal:3002/mcp` was not reachable (network firewall in sandboxed container). No external model was available to run `trainer-optimize` natively.

## Agent Handoff Summary
The `@trainer` agent completed the manual_followup path:
1. Analyzed the 6-row training dataset (all `llm_judge` scoring)
2. Identified 5 high-priority gaps from the research brief
3. Composed the optimized prompt candidate by applying the smallest defensible revisions to each gap
4. Saved the candidate as `optimized-prompt.md`

## Key Changes in the Optimized Candidate
1. **Stale-critique gate** (Step 1): Added an explicit check that the supplied critique is current and actionable before proceeding with any revision.
2. **Smallest-change filter** (Constraints): Tightened from "smallest defensible revision" to "exactly one critique point without modifying unrelated contract text."
3. **Engineer handoff trigger** (inline + Constraints): Replaced vague condition with two concrete examples: (a) prompt-engineering patterns (few-shot, chain-of-thought, structured output), (b) clarity of teacher-facing explanation.
4. **Hard stopping criterion** (Constraints + Step 6): Added explicit hard stop after two revision passes with negative approval prediction.
5. **Validation step** (Step 7 + Output Format): Specified `python -m pytest -q` as the concrete validation command.

## Rerun Command
To retry with a live model when the MCP server is available:
```bash
# Start the agent-skills MCP server, then run trainer-optimize:
# python -m pytest -q  # verify baseline first
# trainer-optimize --prompt-file .github/agents/student.agent.md \
#   --train-file .../train.jsonl --val-file .../val.jsonl \
#   --judge-mode llm_judge --iterations 3
```
