# Engineer Copilot Agent - Optimization Review

## Target Goal
Improve the `engineer-copilot-agent` SKILL.md prompt to provide better guidance for engineers working on GitHub Copilot custom agents. Focus on clarity, conciseness, and actionable workflow guidance.

## Current State Assessment
The skill provides:
- Clear when-to-use guidance
- Structured core workflow with discovery, validation, and analysis
- Four concern separation framework (Frontmatter, Routing, Handoffs, Minimization)
- Recursive minimization loop with concrete steps
- Evals guidance and output contract

## Likely Failure Modes
1. **Overly prescriptive**: The numbered workflow steps (1-8) may feel rigid or implementation-focused rather than guidance-focused
2. **Redundancy**: Some guidance about references and scripts appears in multiple sections (core workflow, minimization loop, evals)
3. **Missing context**: Limited examples of what "stale routing" or "prompt bloat" look like in practice
4. **Incomplete handoff guidance**: The handoffs section is brief and could be more actionable for engineers uncertain about ownership boundaries

## Dataset Requirements
- Example `.agent.md` files with different issues (over-triggering, stale names, bloated prompts)
- Validation scenarios showing common agent contract failures
- Real-world routing problems and their fixes

## Validation Plan
1. Run existing tests to establish baseline
2. Run agent-skills MCP server to validate skill structure
3. Check skill triggering with representative user requests
4. Validate that references are correctly named and accessible

## Next Optimization Hypothesis
Focus on:
1. Making the workflow more principles-driven than procedure-driven
2. Reducing redundancy between sections
3. Adding concrete examples of common agent issues and fixes
4. Improving the evals guidance with more specific failure-mode patterns
