# AGENTS.md

## git usage

Always use `git mv` to rename/move files. This preserves file history and avoids merge conflicts caused by delete + create patterns.

## Agent Patterns

This repository includes custom agents for prompt/context optimization workflows. Use the `task` tool to invoke them with the `agent_type` parameter.

| Agent | Use When | Handoff Pattern |
|-------|----------|-----------------|
| **trainer** | Orchestrating a trainer-led optimization loop | Route `train` calls through agent-skills MCP server; coordinates research, student revision, judging, and adversarial review across iterations |
| **researcher** | Researching public datasets, benchmarks, documentation, or source provenance | Use before eval synthesis or when grounded discovery is needed; surfaces research briefs and licensing context |
| **teacher** | Reviewing optimization artifacts to explain how prompts/datasets/workflows should improve next | Provide critique without taking over orchestration; call when student/trainer needs guidance before revision |
| **student** | Drafting or revising prompt candidates from teacher guidance inside trainer-led loops | Operate inside trainer loop; explicit reasoning trajectory for teacher review |
| **judge** | Scoring prompt candidates or comparing revisions with concise summaries | Use for outcome-based evaluation; provides scoring matrices and candidate comparisons |
| **engineer** | Formatting reasoning, solution plans, or tracing code for teacher review and clarity | Call to improve explanation structure for human review; helps make implicit logic explicit |
| **conservator** | Reviewing prompt/dataset/evaluator changes for likely regressions after optimization | Use after optimization before promotion; surfaces potential breakage areas |
| **adversary** | Stress-testing prompts/datasets/evaluators by producing exploit artifacts to trick the judge | Use before finalization to find edge cases and failure modes |

See `docs/agent-handoff-patterns.md` and `.github/agents/*.agent.md` for full agent definitions and workflow examples.

## agent-skills

### Creating new skills

New agent-skills are built using Anthropic's [skill-creator](https://github.com/anthropics/skills) tool:

```bash
npx skills add https://github.com/anthropics/skills --skill skill-creator
```

Place new skills in the repository root `~/skills` directory.

### Using agent-skills

Agent-skills are registered as symlinks into `~/.agents/skills` to make them available to custom agents and the MCP server. The `skills-lock.json` file tracks skill dependencies. See `tools/agent-skills-mcp/` for the MCP server implementation and `skills/*/README.md` for skill documentation.

Current skills include: `trainer-train`, `trainer-optimize`, `researcher-research`, `engineer-prompt`, `judge-outcome`, `judge-rubric`, etc.

## Code

### Test coverage

All code must achieve **100% test coverage** using TDD. Code here means:
- `copilot_runtime/` — Core runtime behavior
- `skills/` — Python logic in agent-skills (not shell scaffolding)
- `tools/` — MCP servers and supporting utilities
- `tests/` — Must also be tested for test logic validity

Use `pytest` with coverage reporting (see `pytest.ini`). Aim for meaningful assertions, not line-counting.

### Browser validation

Use Playwright to visually validate browser-based agent work (e.g., UI agents, form interaction, rendering). After test runs:

1. Capture screenshots or recordings showing successful outcomes
2. Include them in your PR description so reviewers see the intended behavior visually
3. Example: agent UI validation, dashboard state after workflow, rendered output

This is most relevant when an agent task involves browser interaction or visual state changes.

## Scaffolding

Use project-specific CLI tools to scaffold instead of manually creating/editing files. This ensures consistency and avoids drift from templates.

- **Python** (`uv`): `uv init` for new packages, manage dependencies in `pyproject.toml`
- **Node.js** (`npm`): `npm init`, use package.json for scripts and dependencies
- **.NET** (`dotnet`): `dotnet new`, `dotnet add package` for dependencies
- **Workflow files** (`create-workflow` skill): Use the agent-skill for GitHub Actions scaffolding

See `examples/` for project scaffolding templates and `skills/create-workflow/` for workflow generation.
