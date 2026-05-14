# Original Candidate: student.agent.md

**Source:** `.github/agents/student.agent.md` (baseline, no changes)

## Description

The original agent contract as checked into the repository. This is the pre-optimization baseline used for comparison.

## Known issues (from engineer-prompt/review.md)

1. `agent/runSubagent` in tool list — redundant with named handoffs
2. Teacher handoff trigger too broad — four conditions, not just "undefined target"
3. `execute` tool has no scope guidance — could be used to invoke skills
4. Redundant prohibition: "Do not take over judging, adversarial review, or trainer-loop orchestration"
5. Engineer handoff trigger too vague
6. Reasoning trajectory requirement does not require considered-and-rejected alternatives
