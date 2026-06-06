# AGENTS.md

This file documents key development conventions and best practices for contributors to this repository. Following these guidelines ensures consistent, maintainable, and high-quality contributions.

## Git Workflow

### File Operations
Always use `git mv` to rename, move, or reorganize files rather than manual delete-and-create operations. This preserves file history in Git, which is critical for tracking changes, understanding the evolution of code, and recovering from mistakes.

**Why this matters:** Using `git mv` maintains the link between the old and new file paths, preserving the complete commit history. Manual deletion loses this history.

## Agent Skills Setup

### Creating and Managing Agent Skills
Agent skills in this repository follow a structured process:

1. **Location**: Place new agent skills in the root `skills/` directory (e.g., `~/skills/my-skill/`).
2. **Symlinking**: Create symlinks from the agent runtime's skill directory (`~/.agents/skills/`) to your skill definitions in `~/skills/`.
3. **Creation Method**: Always use Anthropic's `skill-creator` skill to scaffold new skills:
   ```bash
   npx skills add https://github.com/anthropics/skills --skill skill-creator
   ```
   This ensures consistent structure, metadata, and integration with the agent runtime.

**Why this matters:** The `skill-creator` tool enforces skill contracts and naming conventions, reducing setup errors and making skills discoverable and maintainable.

## Code Quality and Testing

### Test-Driven Development (TDD)
All code contributions must follow test-driven development practices:

1. **Coverage**: Aim for 100% code coverage with unit tests.
2. **Tools**: Use `pytest` as the testing framework. Run tests with:
   ```bash
   python -m pytest -q
   ```
3. **Metrics**: Track coverage metrics to ensure all code paths are tested.

### Validation and Documentation
Beyond unit tests, validate your work visually and document the results:

1. **Browser Validation**: Use Playwright to validate your changes in the browser to ensure visual correctness and user experience quality.
2. **Screenshots**: Include screenshots of successful outcomes in your pull request description so reviewers can see the impact of your changes without running the code locally.

**Why this matters:** Comprehensive testing (unit + visual) catches both logic errors and UI regressions. Screenshots accelerate review and provide a record of intended behavior.

## Project Scaffolding

### Using Project-Specific CLI Tools
When creating new files or structures within this project, use the project-specific CLI tools and scaffolding mechanisms rather than manually creating files:

- **Node.js projects**: Use `npm` scaffolding commands
- **.NET projects**: Use `dotnet` CLI commands
- **Python projects**: Use `uv` for environment management and `pip` for dependencies
- **Other languages**: Use the canonical tool for that ecosystem

**Why this matters:** Project-specific tools understand the repository structure, conventions, and dependencies. They prevent manual errors and ensure consistency with existing patterns.

Examples of correct scaffolding:
- Creating a new Node.js feature: `npm init` followed by standard setup
- Adding Python dependencies: `uv` to manage virtual environments and `pip` to lock requirements
- Scaffolding a .NET project: `dotnet new` with the appropriate template

Avoid manual file creation for project structures; let the tools handle the boilerplate.

## Repository Standards

### Documentation Quality
- All public modules and functions should have clear documentation.
- Keep documentation co-located with code (docstrings, comments, README files).
- Update documentation whenever you update functionality.

### Code Style and Conventions
- Follow the language-specific style guide for any code you write (PEP 8 for Python, etc.).
- Use meaningful variable and function names.
- Keep functions focused and testable.

### Pull Request Expectations
- Ensure all tests pass before opening a pull request: `python -m pytest -q`
- Provide a clear description of changes and rationale.
- Include screenshots or demo links for user-facing changes.
- Reference related issues or discussions.
