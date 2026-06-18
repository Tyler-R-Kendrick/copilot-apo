---
name: engineer-copilot-agent
description: Improve GitHub Copilot custom agents by validating agent contracts, tightening tool and MCP skill routing, and minimizing prompt bloat while keeping handoffs bounded to real workspace agents. Use this whenever the user wants to create, debug, or refine a custom agent.
argument-hint: Describe the target custom agent, whether the concern is triggering, routing, handoffs, structure, evals, or all, and any observed failures such as stale tool names, bad handoffs, or bloated instructions.
license: MIT
compatibility: Python 3.11+. Works in repositories that store GitHub Copilot custom agents as `.agent.md` files alongside reusable skills.
metadata:
  author: Tyler Kendrick
  version: "0.2.0"
---

# Engineer Copilot Agent

Use this skill to create or improve GitHub Copilot custom agents. Separate triggering, routing, handoffs, and prompt size as independent concerns. Keep the top-level agent contract lean.

Read `references/copilot-agent-standard.md` for the baseline contract and `references/context-minimization-loop.md` before editing.

## When to use this skill

Use it when the user wants to:

- Create a new Copilot custom agent from scratch
- Fix an existing agent that under-triggers, over-triggers, or has stale tool names
- Audit tool-calling or MCP skill routing against the live workspace inventory
- Clarify subagent handoff ownership and boundaries
- Trim a bloated agent prompt by moving standards and checks into separate artifacts
- Add eval coverage that catches routing regressions or invented tools

Do not use this skill for prompt-only or skill-only work unless the target artifact is a Copilot custom agent or you are explicitly asked to design one.

## Required inputs

- Target `.agent.md` file path, or a description of the agent to create
- Repo root when runtime surface discovery is needed
- Improvement focus: triggering, routing, handoffs, structure, evals, or a combination
- Known failure modes such as under-triggering, stale names, or bad handoffs

## Workflow overview

Follow this discovery-first order:

1. **Discover**: Run `python scripts/discover_runtime_surface.py --repo-root <repo-root> --json`. Live session inventory overrides repo snapshot.
2. **Validate**: Run `python scripts/validate_agent.py <agent-path> --repo-root <repo-root> --json`.
3. **Analyze**: Run `python scripts/analyze_agent_body.py <agent-path> --repo-root <repo-root> --json`.
4. **Fix errors**: Correct validation failures before editing for style.
5. **Sync routing**: Update tool, skill, and agent names to match discovered inventory.
6. **Trim bloat**: Move standards to `references/` and checks to `scripts/`. Stop when only routing and ownership remain.
7. **Re-validate**: After each significant revision, re-run discovery and validation.

## Four concern separation

Address these concerns independently in order:

### Frontmatter

Optimize discoverability and fit:

- Keep `name`, `description`, and hints specific and tool-focused
- Expose only tools and handoff targets that actually exist
- Avoid execution detail in frontmatter

### Routing

Name only tools, skills, and agents that exist in your workspace:

- Discover the live inventory before naming any helper
- When names drift, live inventory always overrides repo snapshot
- Never invent MCP routing, tool names, or handoff targets without verification

### Handoffs

Make ownership clear:

- State what the agent should do directly
- State what should be handed off and why
- Avoid ownership overlap between orchestrator and helper agents
- Articulate when *not* to hand off—do not only list positive handoff rules

### Minimization

Keep `.agent.md` as a routing layer only:

- Move standards, examples, and extended guidance into `references/`
- Move deterministic checks and repeated logic into `scripts/`
- Delete prose that duplicates deeper assets
- Stop when each remaining section is necessary for triggering, routing, or bounded execution

## Recursive minimization loop

Trim in multiple passes until the prompt is only as large as needed:

1. Draft or repair the routing layer.
2. Extract standards, examples, and guidance into focused reference documents.
3. Move mechanical checks and inventory logic into scripts.
4. Delete any prose that duplicates those external assets.
5. Re-read the remaining body and trim again.

**Stop when**: Each remaining section serves routing, ownership clarity, or bounded handoff orchestration. If a section could move to `references/` or `scripts/`, move it.

## Evals and regression prevention

When asked for evals, focus on failure modes that automated checks can catch:

- Invented tools, skills, or handoff targets (validate against discovered inventory)
- Stale helper names (validate against current repo state)
- Missing discovery before concrete tool naming
- MCP skill naming without proper load-before-run ordering
- Frontmatter-body inconsistency (declared tools that are never used, or routed tools not declared)
- Unnecessary handoffs (simple work that should use direct tools, not subagents)
- Prompt bloat that belongs in references or scripts

Use `validate_agent.py` and `analyze_agent_body.py` to back up eval assertions with deterministic checks.

## Output contract

When improving a custom agent, deliver:

1. Runtime surface summary from discovery
2. Validation results and error list
3. Analysis of body structure and concerns
4. Improvement plan and changes made
5. Re-validation status and any remaining risks
