# Predicted Judge Response: Optimized Prompt

## Likely Assessment
The judge should find this version an improvement because:

1. **Redundancy reduction**: The original had three sections teaching the same "concern separation" idea. The optimized version teaches it once with clear examples.

2. **Principle clarity**: By emphasizing discovery-first and concern separation as guiding principles rather than numbered steps, the skill becomes more applicable to variations in workflow (e.g., when evals are already present).

3. **Specific failure modes**: The evals section now lists concrete, checkable patterns:
   - "Invented tools, skills, or handoff targets"
   - "MCP skill naming without proper load-before-run ordering"
   - "Frontmatter-body inconsistency"
   These are more actionable than the generic "prompt bloat" guidance in the original.

4. **Handoff clarity**: The addition of "Articulate when *not* to hand off" addresses a gap identified in the research brief and reflects eval case 4 and case 8 (the suggested case about unnecessary handoffs).

5. **Better coverage of research findings**: 
   - Emphasizes the discovery-first pattern (research finding 1)
   - Addresses MCP skill ordering (research finding 2)
   - Includes frontmatter-body consistency (research finding 4)
   - Clarifies non-handoff boundaries (research finding 6)

## Potential Critiques
- The optimized version is slightly shorter and loses some procedural detail, which could be seen as either more concise or less thorough
- "Stop when" condition is slightly less formal than "recursive pass until..."
- Could still benefit from concrete code examples, but those might belong in references/ rather than the main body
