# Optimized engineer-copilot-agent SKILL.md

## Key Improvements
1. **Reduced redundancy**: Combined core workflow and concern sections into a single discovery-first workflow order
2. **Principles-driven**: Workflow focuses on principles (discovery-first, concern separation) rather than numbered procedures
3. **Clearer handoff guidance**: Explicitly articulates when NOT to hand off, not just when to hand off
4. **Specific evals patterns**: Lists concrete failure modes that evals should catch (invented tools, stale names, MCP ordering, frontmatter mismatch, etc.)
5. **Conciseness**: Reduced overall length while preserving all essential guidance
6. **Better structure**: Clearer separation between workflow, concerns, minimization loop, and evals

## Changes Made
- Consolidated core workflow (steps 1-8) with workflow overview that emphasizes discovery-first
- Merged redundant minimization guidance
- Expanded evals section with specific failure-mode patterns
- Added concrete examples of what to check (frontmatter-body consistency, unnecessary handoffs, MCP ordering)
- Simplified language to be more actionable and less prescriptive
- Improved handoff section to emphasize ownership clarity and "do not hand off" boundaries

## Alignment with Eval Cases
- Addresses case 1: discovery-first and concern separation
- Addresses case 2: validation before changes and stale routing fixes
- Addresses case 3: recursive minimization with clear stop condition
- Addresses case 4: explicit ownership and handoff boundaries
- Addresses case 5: routing resilience through discovery
