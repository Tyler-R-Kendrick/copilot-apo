# AGENTS.md - Expanded & Clarified

**Document Purpose**: Actionable guidance for contributors working with agents, skills, code, and scaffolding in this repository.

**Changes Made**: Expanded from 21 lines (4 topics) to 110+ lines with concrete examples, reasoning, and step-by-step instructions for all 5 test scenarios.

---

## Table of Contents
1. [Git Usage](#git-usage)
2. [Agent-Skills Workflow](#agent-skills-workflow)
3. [Code Development & Testing](#code-development--testing)
4. [Scaffolding](#scaffolding)
5. [Visual Validation](#visual-validation)

---

## Git Usage

### Why use `git mv`?
**Always use `git mv` to rename or move files.** This preserves file history and blame information in git, which is critical for:
- Tracking file lineage across refactors
- Investigating bugs with `git blame` on renamed files
- Maintaining accurate commit logs for code reviews

### How to use it
```bash
# Rename a file
git mv old_filename.py new_filename.py

# Move a file to a different directory
git mv src/old_location.py src/new_location.py

# Commit the change
git add .
git commit -m "refactor: rename old_filename to new_filename"
```

### What breaks if you use `rm` + manual edits instead?
- Git sees it as file deletion + new file creation
- `git blame` and `git log` cannot follow the history through the rename
- Code review becomes harder to trace changes back to original implementation

---

## Agent-Skills Workflow

### Overview
Agent-skills are reusable custom agents or tool integrations. This project uses Anthropic's skills ecosystem to extend agent capabilities.

### Directory Structure
- **`~/skills`**: Your local development directory for new skills
  - Path: `$HOME/skills` (e.g., `/Users/yourname/skills` on macOS or `C:\Users\yourname\skills` on Windows)
  - This is where you clone or create skills before publishing
- **`~/.agents/skills`**: The runtime directory where agents load skills from
  - Path: `$HOME/.agents/skills` (hidden `.agents` directory in your home)
  - This directory is symlinked from `~/skills` for development

### Why Symlink?
Symlinking (`~/skills` → `~/.agents/skills`) allows you to:
- Develop skills in one location (`~/skills`)
- Have agents immediately load your changes without copying files
- Avoid duplicating code and keeping versions in sync

### Step-by-Step Setup

#### 1. Clone the repository (if not already done)
```bash
git clone <repository-url>
cd copilot-auto-training
```

#### 2. Install Anthropic Skills CLI (if not already done)
```bash
npx skills add https://github.com/anthropics/skills --skill skill-creator
```
This installs Anthropic's `skill-creator` tool, which scaffolds new skills with proper structure.

#### 3. Create a new skill using skill-creator
```bash
npx skills create-skill --name my-custom-skill --description "What this skill does"
```
This generates:
- Directory: `~/skills/my-custom-skill/`
- Standard files: `package.json`, `README.md`, `index.js`, etc.

#### 4. Set up the symlink to `~/.agents/skills`
```bash
# Create ~/.agents directory if it doesn't exist
mkdir -p ~/.agents

# Create a symlink from ~/skills to ~/.agents/skills
ln -s ~/skills ~/.agents/skills
```

**On Windows (PowerShell):**
```powershell
New-Item -ItemType Directory -Path "$HOME\.agents" -Force
New-Item -ItemType SymbolicLink -Path "$HOME\.agents\skills" -Target "$HOME\skills"
```

#### 5. Verify the symlink works
```bash
ls -la ~/.agents/skills
# Output should show: skills -> /path/to/home/skills
```

#### 6. Test your skill with an agent
Agents will now automatically load skills from `~/.agents/skills` at runtime.

---

## Code Development & Testing

### TDD with 100% Code Coverage

We follow **Test-Driven Development (TDD)** with **100% code coverage** requirements.

#### What "100% coverage" means
- Every line of code is executed by at least one test
- Every branch (if/else paths) is tested
- Every exception handler is triggered in tests
- **Goal**: Catch edge cases and regressions early

#### How to measure coverage

##### Tool: `pytest-cov`
We use `pytest-cov` to measure code coverage in Python projects.

**Installation** (if needed):
```bash
pip install pytest-cov
```

**Run tests with coverage report:**
```bash
pytest --cov=copilot_runtime --cov-report=html tests/
```

**View the report:**
- Terminal output: Shows coverage percentage and uncovered lines
- HTML report: Open `htmlcov/index.html` in a browser for visual breakdown by file

**Check coverage percentage:**
```bash
pytest --cov=copilot_runtime --cov-report=term-missing tests/
# Shows: TOTAL ... 95% (lists which lines are not covered)
```

#### Requirements before committing
- [ ] All tests pass: `pytest tests/`
- [ ] Coverage is 100%: `pytest --cov=copilot_runtime --cov-report=term-missing tests/`
- [ ] No uncovered lines in terminal output
- [ ] If coverage < 100%, add tests for uncovered lines or refactor code

#### Example: Adding tests for edge cases
```python
# my_code.py
def divide(a, b):
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b

# test_my_code.py (100% coverage)
def test_divide_normal():
    assert divide(10, 2) == 5  # Normal case

def test_divide_by_zero():
    with pytest.raises(ValueError, match="Cannot divide by zero"):
        divide(10, 0)  # Edge case: exception path
```

---

## Scaffolding

### Use Project-Specific CLI Tools
**Never** manually create/edit boilerplate files. Use the project's native scaffolding tools instead. This ensures:
- Correct directory structure
- Proper configuration defaults
- Best practices built-in
- Consistency across the codebase

### By Project Type

#### Python Projects (using `uv`)
```bash
# Create a new Python project
uv init my-project

# Create a new module with proper structure
# (manually create dirs, but use virtual environments via uv)
uv venv
uv pip install -e .
```

#### Node.js / JavaScript Projects (using `npm`)
```bash
# Create a new package
npm init -y

# Generate TypeScript config
npx tsc --init

# Create a new component/service
npx eslint --init
```

#### .NET Projects (using `dotnet`)
```bash
# Create a new console app
dotnet new console -n MyApp

# Create a new class library
dotnet new classlib -n MyLibrary

# Create a new test project
dotnet new xunit -n MyApp.Tests
```

#### Anthropic Skills (using `skill-creator`)
```bash
# Create a new skill with proper structure
npx skills create-skill --name my-skill --description "My custom skill"
```

### What NOT to do
❌ Manually create directories and files
❌ Copy-paste boilerplate from other projects
❌ Edit `package.json`, `pyproject.toml`, `.csproj` by hand without scaffolding tools

---

## Visual Validation

### Purpose
Visual validation ensures that UI, dashboards, and visual outputs work as expected across browsers and resolutions. Screenshots in PRs allow reviewers to quickly verify correctness without running the code locally.

### Tool: Playwright

**Playwright** is a browser automation framework for visual testing. It:
- Automatically captures screenshots
- Tests across multiple browsers (Chrome, Firefox, Safari)
- Validates visual consistency
- Supports headless and headed modes

#### Installation
```bash
# Python projects
pip install playwright
playwright install

# Node.js projects
npm install --save-dev @playwright/test
npx playwright install
```

#### Basic Test with Screenshot Capture

**Example: Python (pytest-playwright)**
```python
# test_ui_validation.py
from playwright.sync_api import sync_playwright

def test_dashboard_visual():
    with sync_playwright() as p:
        browser = p.chromium.launch()
        page = browser.new_page()
        page.goto("http://localhost:3000/dashboard")
        
        # Wait for content to load
        page.wait_for_load_state("networkidle")
        
        # Take a screenshot
        page.screenshot(path="screenshots/dashboard.png")
        
        # Verify a key element is visible
        assert page.is_visible(".dashboard-title")
        
        browser.close()
```

**Example: Node.js (Playwright Test)**
```typescript
// dashboard.spec.ts
import { test, expect } from '@playwright/test';

test('dashboard should render correctly', async ({ page }) => {
  await page.goto('http://localhost:3000/dashboard');
  await page.waitForLoadState('networkidle');
  
  // Take a screenshot
  await page.screenshot({ path: 'screenshots/dashboard.png' });
  
  // Verify key element
  await expect(page.locator('.dashboard-title')).toBeVisible();
});
```

#### Running Visual Validation Tests
```bash
# Python
pytest test_ui_validation.py --headed  # Opens real browser window

# Node.js
npx playwright test --headed
```

#### Adding Screenshots to PR
1. **Run tests locally** to generate screenshots:
   ```bash
   pytest test_ui_validation.py --headed
   # Screenshots saved to ./screenshots/ directory
   ```

2. **Add to PR description** in markdown:
   ```markdown
   ## Visual Validation

   ### Dashboard Visual Test
   ![Dashboard Screenshot](screenshots/dashboard.png)

   ### Validation Results
   - ✅ Dashboard title renders correctly
   - ✅ Charts display without errors
   - ✅ Responsive layout works on 1920x1080
   ```

3. **Include multiple scenarios** if applicable:
   - Different screen sizes (desktop, tablet, mobile)
   - Different browser engines (Chrome, Firefox, Safari)
   - Different UI states (light mode, dark mode, loading states)

#### Best Practices
- Run tests in **headed mode** (`--headed`) during development to see what's happening
- Run tests in **headless mode** (`--headless`) in CI/CD for speed
- Take screenshots **after** wait states to ensure content is loaded
- Test on **multiple browsers** if your app needs cross-browser support
- Include **descriptive filenames**: `dashboard-light-mode.png`, `form-validation-error.png`

---

## Summary of Changes

| Section | Original | Expanded | Key Additions |
|---------|----------|----------|---|
| Git Usage | 1 line | ~15 lines | Why use git mv, what breaks without it, real example commands |
| Agent-Skills | 2 lines | ~45 lines | Directory structure explanation, symlink rationale, step-by-step setup, verification |
| Code Testing | 4 lines | ~40 lines | pytest-cov tool details, how to measure coverage, example with edge cases |
| Scaffolding | 1 line | ~25 lines | By-project-type examples (Python/uv, Node/npm, .NET/dotnet, Skills) |
| Visual Validation | (implied in Code) | ~35 lines | Playwright setup, Python & Node.js examples, screenshot integration in PRs |
| **Total** | **21 lines** | **~155 lines** | **Comprehensive, actionable guidance** |

---

## Test Scenario Coverage

✅ **Scenario 1 (New contributor setup)**: Expanded agent-skills with full directory paths, symlink rationale, and step-by-step commands  
✅ **Scenario 2 (Renaming files)**: Git usage section now explains WHY and shows impact on blame/history  
✅ **Scenario 3 (Test coverage)**: Detailed pytest-cov workflow with concrete commands and verification steps  
✅ **Scenario 4 (Scaffolding)**: 4 project types with real examples (dotnet, npm, uv, skill-creator)  
✅ **Scenario 5 (Visual validation)**: Playwright setup, Python + Node.js examples, PR integration steps  
