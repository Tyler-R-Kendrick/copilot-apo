# AGENTS.md

## Git Usage

Always use `git mv` to rename/move files. This preserves Git history and prevents confusion when files are relocated.

## Build and Test

Set up the Python virtual environment and install dependencies:
```
python3.12 -m venv .venv && source .venv/bin/activate && python -m pip install -r requirements.txt
```

Run tests to validate changes:
```
python -m pytest -q
```

Run these commands before committing to catch issues early.

## Code and TDD

Always use TDD with code coverage metrics to ensure 100% coverage. Structure code with clear separation:
- **Spec**: Prompts and instruction files that define expected behavior
- **Evals**: Evaluation/test code that validates correctness
- **Runtime**: Production code that implements the behavior

Use Playwright to visually validate your work in the browser afterwards. Take screenshots of successful outcomes and include them in your PR description so reviewers can see the results.

## Agent-Skills

If you create agent-skills, put them in the root skill directory (`~/skills`) and symlink them into the `~/.agents/skills` directory. This separation allows you to develop skills independently while the symlink makes them available to the agent runtime.

Always create them using Anthropic's "skill-creator" skill:
```
npx skills add https://github.com/anthropics/skills --skill skill-creator
```

## Prompt and Instruction File Editing

Prompt-like files include specification documents, instruction files, and configuration that define expected behavior. When editing these files, reference the detailed guidance in `.github/instructions/prompt-optimization.instructions.md` rather than duplicating instruction content.

Example: Before expanding a prompt significantly, check if you're addressing an identified gap or if the current instruction file already covers the guidance more comprehensively.

## Agentic Workflow Editing

When modifying agentic workflows (YAML files in `.github/workflows/`), compile the workflow to validate the syntax:
```
gh aw compile <workflow-name>
```

After compilation, ensure the lockfile (`skills-lock.json`) is synchronized. This prevents runtime failures and keeps the skills registry consistent across environments.

## Scaffolding

Use project-specific CLI tools to scaffold instead of manually creating or editing files (dotnet, uv, npm, etc.). This ensures consistency with the project's conventions and reduces manual errors.
