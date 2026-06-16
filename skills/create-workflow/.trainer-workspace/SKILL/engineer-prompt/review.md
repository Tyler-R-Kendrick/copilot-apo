# Engineering Review: create-workflow SKILL.md

## Target Goal
Optimize the `create-workflow` skill prompt to improve clarity, user guidance, and agent behavior quality when creating GitHub Agentic Workflows.

## Current Task Shape
This skill teaches GitHub Copilot agents how to:
1. Create or update `.github/workflows/<name>.md` files
2. Configure frontmatter (triggers, permissions, MCP servers, safe outputs)
3. Author markdown instructions for workflow runtime behavior
4. Compile workflows with `gh aw compile`
5. Validate and debug compilation failures
6. Handle MCP server integration and safe output configuration

## Likely Failure Modes
- **Scope creep**: Users may ask for generic GitHub Actions YAML authoring (out of scope)
- **Repository readiness**: Users may skip initialization steps, causing silent failures
- **Frontmatter complexity**: MCP server configuration details can overwhelm or confuse
- **Workflow structure**: Unclear when to use frontmatter vs. markdown body guidance
- **Debugging isolation**: Users may conflate compilation errors, MCP failures, and runtime issues
- **Safe outputs scope**: Confusion about which write operations require safe-outputs

## Dataset Gaps
- No worked examples of frontmatter MCP configuration
- Limited examples of debugging different failure modes
- No examples of when MCP is overkill vs. necessary
- Missing guidance on workflow body instruction quality vs. agent behavior quality

## Validation Plan
1. Check that the skill accurately routes users away from generic GitHub Actions work
2. Verify repository readiness checks catch uninitialized repos early
3. Validate that workflow body guidance emphasizes clarity and specificity
4. Confirm that debugging section guides users toward the correct failure bucket
5. Test that safe-outputs scope is clear

## Next Optimization Hypothesis
The skill can improve by:
1. Adding a repository readiness checklist earlier in the workflow
2. Providing clearer MCP configuration patterns with more concrete examples
3. Separating workflow authoring concerns (frontmatter, markdown body, compilation) more explicitly
4. Expanding the debugging section with decision trees for common error patterns
5. Adding guidance on workflow body instruction quality (clarity, specificity, branching logic)

## Editing Strategy
Focus on instructional clarity, scope boundaries, and agent routing. Prioritize making repository readiness, MCP configuration, and debugging more accessible without expanding the skill into generic GitHub Actions territory.
