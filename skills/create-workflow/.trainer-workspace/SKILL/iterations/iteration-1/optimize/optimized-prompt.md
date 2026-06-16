---
name: create-workflow
description: Create or update a GitHub Agentic Workflow in .github/workflows using gh aw, including frontmatter, markdown instructions, optional MCP servers, compilation, and debugging. Use this whenever the user wants new repository automation, wants to turn a repeated GitHub process into an agentic workflow, needs to add external MCP tools to a workflow, or needs help validating and fixing a workflow before commit.
argument-hint: Describe the workflow goal, trigger, outputs, engine, and any MCP or safe-output requirements.
---

# Create Workflow

Use this skill to author GitHub Agentic Workflows as markdown source files and carry them through compilation and validation.

GitHub Agentic Workflows are stored as `.github/workflows/<name>.md` and compiled into `.github/workflows/<name>.lock.yml`. The markdown body is the runtime instruction set. The frontmatter controls triggers, permissions, tools, MCP servers, network access, and safe outputs.

Read [the authoring reference](./references/gh-aw-authoring.md) before drafting the workflow when the request includes MCP setup, unfamiliar triggers, safe outputs, or troubleshooting.

Use [the starter template](./assets/workflow-template.md) when you need a clean first draft.

## gh aw CLI quick reference

`gh aw` is a GitHub CLI extension, so invoke it through `gh`, not as a standalone `gh-aw` binary.

- Run commands from the repository root.
- Use `gh aw --help` for the top-level command list.
- Use `gh aw <command> --help` for command-specific flags.
- `gh aw new <workflow-name> --engine copilot` scaffolds a new markdown workflow.
- `gh aw compile <workflow-name>` accepts either a workflow id like `my-workflow` or a filename like `my-workflow.md`; with no argument it compiles every markdown workflow under `.github/workflows/`.
- `gh aw validate <workflow-name> --strict` validates without emitting lock files and runs the full validation stack.
- `gh aw trial ./path/to/workflow.md --host-repo .` is the local-file path for exercising a `workflow_dispatch` workflow against the current repository.
- `gh aw list` is a quick readiness check; if it reports `no .github/workflows directory found`, the repository has not been set up for workflow authoring yet.

## When to use

- The user wants to create a new GitHub Agentic Workflow.
- The user wants to automate repository work such as triage, reporting, validation, issue handling, scheduled analysis, or orchestrated agent tasks.
- The user wants to convert a repeated GitHub process into a workflow file in `.github/workflows/`.
- The user needs to add or update MCP servers for a workflow.
- The user needs help compiling, validating, or debugging a workflow after editing it.

### Scope boundaries: Do not use for

- Generic GitHub Actions YAML authoring (not Agentic Workflow markdown).
- Workflows that do not need agent reasoning or decision-making (use GitHub Actions directly).
- Requests to build custom CI/CD pipelines without agentic components.
- Requests to automate build steps, testing matrices, or deployment logic via GitHub Actions (redirect to standard GitHub Actions documentation).

**Redirect guidance**: If a user asks for GitHub Actions automation unrelated to agentic workflows, respond with:
> "That sounds like a GitHub Actions workflow, not a GitHub Agentic Workflow. GitHub Agentic Workflows are designed for agent-driven tasks like triage, analysis, and decision-making. For standard CI/CD pipelines or build automation, I recommend using [GitHub Actions](https://docs.github.com/en/actions). If you'd like to add agent reasoning to your workflow (e.g., intelligent triage or automated analysis), I can help with that using the create-workflow skill."

## Core workflow

Follow this order.

### 0. **Confirm repository readiness (FIRST STEP)**

**Why this is critical**: Many failures stem from skipped initialization. Check repository setup before drafting.

Run this quick check:

```bash
gh aw list
```

**If the repository is initialized:**
- `.github/workflows/` exists
- `gh aw list` returns existing workflows or "no workflows found"
- Proceed to step 1

**If the repository is NOT initialized:**
- `gh aw list` returns `no .github/workflows directory found` or the command fails
- **Instruct the user to initialize first**:
  ```bash
  gh aw init
  ```
- Explain what initialization does: sets up the dispatcher agent, `.gitattributes`, and Copilot MCP wiring (unless `--no-mcp` is passed)
- If the user only wants a draft file before initialization, you can still create it, but note that compilation and execution require initialization and configured secrets.

**Readiness prerequisites to verify:**
- Repository is initialized (`gh aw init` has run)
- User has permissions to modify `.github/workflows/`
- Repository secrets are configured for the engine (e.g., `GITHUB_TOKEN` for Copilot)
- If using MCP servers, credentials for external services are ready as secrets

---

### 1. Gather the workflow contract

Before writing the file, extract or ask for the minimum set of requirements:

- **workflow purpose** — What business problem does this solve?
- **trigger model** — issue, pull request, schedule, manual dispatch, discussion, or command?
- **expected outputs or side effects** — Does it only analyze and report, or does it create/update GitHub artifacts?
- **target engine** — default is Copilot; confirm if other engines are needed
- **required GitHub toolsets** — default, pull_requests, issues, discussions, labels?
- **required external services or MCP servers** — Notion, Slack, Jira, custom API?
- **write scope** — Does the workflow write back to GitHub? If so, what types: issues, comments, labels, PR reviews?
- **security or network constraints** — Does it need specific domains? Should certain domains be blocked?

### Decision points to close quickly

If the user is vague, ask these focused questions in order:

1. **What event should trigger the workflow?** (issue opened, PR created, scheduled, manual `workflow_dispatch`, discussion, slash command?)
2. **What should the workflow do first?** (analyze content, fetch external data, check repository state?)
3. **Should it only analyze and report, or should it create/update GitHub objects?** (Labels, issues, comments, PR reviews, discussions?)
4. **Does it need tools beyond GitHub?** (Notion, Slack, an API, a custom tool?)
5. **Is this a new workflow or an edit to an existing one?**

---

### 2. Author the markdown workflow file

Create the workflow in `.github/workflows/<workflow-name>.md` using kebab-case names.

#### Split into two distinct layers

The workflow has two responsibilities: **frontmatter** (configuration) and **markdown body** (runtime instructions).

**Frontmatter**: Defines *what tools and permissions* the workflow has.  
**Markdown body**: Defines *what the workflow should do* with those tools.

#### Writing the markdown body for clarity and specificity

Treat the workflow body like instructions for a new teammate. The agent will follow them literally, so specificity is critical.

**Structure**:

- **Start with a one-line goal**: State what the workflow accomplishes in one sentence.
- **Use headings to separate phases**: `## Context`, `## Procedure`, `## Decision Rules`, `## Output Format`
- **Use numbered steps for ordered work**: "1. Analyze the issue for X... 2. Check if condition Y holds... 3. Create Z if needed..."
- **Encode decision logic explicitly**: "If X is true, do A. If X is false, do B. Otherwise, do C."
- **Include output templates** when the result must follow a structure: "Output format: **Summary** (1 sentence), **Key Findings** (list), **Recommended Actions** (list)"
- **Give concrete repository context**: "In this repo, priority labels are: `p1-critical`, `p2-major`, `p3-minor`. Use these conventions consistently."
- **Mention constraints and guardrails**: "Do not escalate without evidence. Do not create duplicate issues."

**Good examples**:

- ❌ "Analyze this issue." → ✅ "Analyze this GitHub issue to determine if it is a bug report, feature request, or user support question. Use the criteria below."
- ❌ "If it's important, label it." → ✅ "If the issue describes a regression in the last release, apply the `regression` label and tag @core-team."
- ❌ "Return a report." → ✅ "Return a report with: Summary (1 sentence), Key Findings (3–5 bullets), Risks (if any), Next Steps (1–2 actions)."

**Action verbs to use**:
- `Analyze`, `Classify`, `Triage`, `Summarize`, `Extract`, `Label`, `Create`, `Update`, `Comment`, `Review`, `Audit`, `Report`, `Dispatch`, `Escalate`

**What NOT to do**:
- Do not write generic advice ("be thoughtful", "consider edge cases").
- Do not assume the agent knows your repository conventions; state them explicitly.
- Do not ask the agent to "use best judgment" without decision rules.

---

### 3. Add MCP servers or safe outputs if required

#### Safe outputs

**When to use**: If the workflow needs to create or update GitHub objects (issues, PR comments, reviews, labels, discussions, etc.), use `safe-outputs`.

**Why**: Safe outputs shield the agent from making arbitrary writes to your repository. The agent produces structured data, and the safe-output handler performs the actual write.

**Specific write types to configure in frontmatter**:

When the workflow writes to GitHub, declare which output types it uses:

- `create_issue` — Workflow creates new issues
- `update_issue` — Workflow updates issue body, title, or state
- `create_pull_request_review_comment` — Workflow comments on PR diffs
- `create_pull_request_comment` — Workflow comments on PRs
- `create_issue_comment` — Workflow comments on issues
- `create_discussion` — Workflow creates discussions
- `create_discussion_comment` — Workflow comments on discussions
- `add_label_to_issue` — Workflow adds labels to issues
- `add_label_to_pull_request` — Workflow adds labels to PRs

**Examples**:

```yaml
# Workflow that creates and comments on issues
safe-outputs:
  - create_issue
  - create_issue_comment

# Workflow that reviews PRs
safe-outputs:
  - create_pull_request_review_comment

# Workflow that labels issues
safe-outputs:
  - add_label_to_issue
```

**Keep write scope minimal**: Only configure the output types the workflow actually uses.

**Note**: Read-only workflows that only analyze and report do not need safe-outputs configuration.

---

#### MCP servers

**When to use**: If the workflow needs external systems, custom tools, or third-party integrations beyond built-in GitHub tools.

**Decision tree**:
- Do you need a tool from Notion, Slack, Jira, or another service? → Use MCP
- Do you need a custom API or helper tool? → Use MCP with `http` or `container`
- Do you only need simple shell utilities or small helpers? → Consider `mcp-scripts` instead
- Can you solve this with GitHub tools alone? → Skip MCP

**MCP configuration principles**:

- **Limit `allowed` tools**: Expose only the tools the workflow actually uses. Use `["*"]` only if broad access is genuinely required.
- **Pass credentials through `env` or `headers`**: Use `secrets` for sensitive values; never hardcode credentials.
- **Prefer read-only integrations**: Restrict external services to reading data, not writing.
- **Keep network access explicit**: If the MCP server needs external domains, list them in `network.allowed`.

**Pattern: Registry-backed server (pre-configured from MCP registry)**

Use when a server is already registered in the MCP registry. This is the simplest and most maintainable approach.

```yaml
mcp-servers:
  markitdown:
    registry: https://api.mcp.github.com/v0/servers/microsoft/markitdown
    allowed: ["convert_to_markdown", "summarize_document"]

network:
  allowed:
    - defaults
    - github.com

# Usage in markdown body:
# 1. Convert the PDF to markdown: "use markitdown to convert_to_markdown on the document"
```

**When**: Using well-known MCP servers that are already in the registry.

**Scoping best practices**:

```yaml
# ❌ Don't do this (overly broad)
mcp-servers:
  notion:
    url: "https://api.notion.com/v1/mcp"
    headers:
      Authorization: "Bearer ${{ secrets.NOTION_API_TOKEN }}"
    allowed: ["*"]  # Exposes all tools, even ones you don't need

# ✅ Do this (scoped to what you need)
mcp-servers:
  notion:
    url: "https://api.notion.com/v1/mcp"
    headers:
      Authorization: "Bearer ${{ secrets.NOTION_API_TOKEN }}"
    allowed: ["query_database", "update_properties"]  # Only these tools
```

**For comprehensive MCP examples** (stdio, container, HTTP transports with credentials and network configuration), see [gh-aw-authoring.md](./references/gh-aw-authoring.md).

---

### 4. Compile and validate

After editing frontmatter or imports, recompile the workflow.

**Validation loop**:

1. Run `gh aw compile <workflow-name>` (or `gh aw compile` for all).
2. For newly authored or security-sensitive workflows, run `gh aw validate <workflow-name> --strict` before or after compile. `--strict` runs the full validation stack without emitting lock files.
3. If iterating on frontmatter frequently, use `gh aw compile --watch <workflow-name>`.
4. If errors are unclear, rerun the failing command with `--verbose`.
5. Confirm the `.lock.yml` file was regenerated after compile.
6. Commit both the `.md` and `.lock.yml` files.

**Important rule**:
- **Markdown body only changes** do not require recompilation.
- **Frontmatter changes** do require recompilation.

---

### 5. Debug failures

Use the structured decision tree below to isolate the root cause and find the shortest path to resolution.

#### Debugging decision tree

| **Symptom** | **Question** | **If Yes** | **If No** |
|-------------|-----------|-----------|----------|
| Workflow is not working | Can it compile? | ✓ Move to Runtime/MCP debugging | ✗ Check compilation: Run `gh aw compile --verbose` |

#### Compilation failures — detailed path

**Root causes and fixes**:

| Symptom | Cause | Fix |
|---------|-------|-----|
| `Error: expected a mapped value at line 5, column 1` | YAML indentation or syntax error | Check spaces vs. tabs, colons, list formatting. Run `gh aw compile --verbose` to see the exact line. |
| `Error: missing required field: 'on'` | Trigger not defined | Add `on: workflow_dispatch:` or appropriate trigger to frontmatter. |
| `Error: unknown field 'tolls'` | Typo in field name | Check spelling: `tools:`, not `tolls`. Use `gh aw compile --verbose` to spot typos. |
| `Error: array syntax invalid at line 12` | List formatting error | Use correct YAML array syntax: `- item1`, `- item2` or `[item1, item2]`. |
| `.lock.yml` not created after compile | Compilation succeeded but lock file not written | Run `gh aw compile --purge` to remove stale artifacts. Check that `.github/workflows/` is writable. |

**Debug workflow**:

1. Run: `gh aw compile --verbose <workflow-name>`
2. Read the error message and look for the line number and field
3. Correct the YAML syntax in the frontmatter
4. Run `gh aw validate --strict` to check schema, lint, and security rules
5. Run `gh aw compile --purge` if lock files seem stale

---

#### Runtime failures — detailed path

**Root causes and fixes**:

| Symptom | Check | Fix |
|---------|-------|-----|
| Workflow trigger never fires | Is trigger configured? | Verify `on: workflow_dispatch:` or correct event type exists in frontmatter. |
| Workflow starts but agent cannot access tools | Are permissions broad enough? | Add `permissions: { contents: read, issues: read, pull-requests: read }` etc. to frontmatter. |
| Write operations fail (e.g., cannot create issue) | Is `safe-outputs` configured? | Add `safe-outputs: [create_issue, create_issue_comment]` to frontmatter for the output types you use. |
| Agent cannot authenticate to external service | Are secrets configured? | Verify `${{ secrets.EXTERNAL_API_KEY }}` exists in repository secrets. Set it in Settings > Secrets. |
| `.lock.yml` exists but not pushed to branch | Workflow not using compiled version | Ensure both `.md` and `.lock.yml` are committed and pushed before triggering the workflow. |

**Workflow instructions too vague or unclear?**

If the agent produces unexpected output or doesn't follow instructions:

1. Check the workflow markdown body for vague language: "be thoughtful", "use best judgment", "handle edge cases"
2. Rewrite with specificity: Add decision rules, output templates, concrete examples
3. Run `gh aw logs <workflow-name>` to see what the agent actually did
4. Revise instructions and run again without recompiling (markdown-only changes don't need compilation)

---

### 6. Quality bar

Before finishing, verify all of these:

- **Workflow filename** is descriptive and kebab-case (e.g., `triage-pr.md`)
- **Frontmatter** is minimal but complete:
  - `on:` is present and specifies the trigger
  - `permissions:` are present and minimal but sufficient
  - `engine:` is specified (default: copilot)
  - `tools:` specifies GitHub toolsets needed
  - `safe-outputs:` lists write types if the workflow writes to GitHub
  - `mcp-servers:` defined if the workflow uses external tools (and `allowed` is scoped)
  - `network.allowed:` specified if MCP needs external domains
- **Markdown body** is specific, structured, and action-oriented:
  - One-line goal at the top
  - Headings separate phases (`## Procedure`, `## Decision Rules`, `## Output Format`)
  - Action verbs used consistently (Analyze, Classify, Create, etc.)
  - Decision rules are explicit ("If X, do A; otherwise do B")
  - Output templates provided for structured results
  - Repository conventions and constraints stated explicitly
- **Write actions** go through `safe-outputs` (not direct agent writes)
- **MCP configuration** is scoped and secrets-aware:
  - `allowed` tools are scoped to what the workflow actually uses
  - Credentials passed through `secrets` in `env` or `headers`
  - Network domains are whitelisted if needed
- **`.lock.yml` exists** and matches the current frontmatter
- **User understands** which edits require recompilation in the future

---

### 7. Response contract

When you create or update a workflow for the user:

1. **State which workflow file(s) you changed** (e.g., `.github/workflows/my-workflow.md`)
2. **Summarize the trigger, main behavior, and configured tools** (e.g., "Triggers on PR open. Checks for conflicts, assigns reviewers, and comments with a checklist. Uses GitHub tools only.")
3. **State whether you recompiled and which validation commands you ran** (e.g., "Compiled and validated with `gh aw validate --strict`. No errors.")
4. **Call out any remaining prerequisites** (e.g., "Requires `NOTION_API_TOKEN` in repository secrets before first run." or "Repository must be initialized with `gh aw init` first.")

---

## Example request shapes

These are common workflows you should be able to help with:

- Create a scheduled workflow that posts a weekly repository health issue.
- Create an issue triage workflow that uses GitHub tools and asks clarifying questions.
- Add a Notion MCP server to this workflow and restrict it to search and create tools.
- Fix why this workflow compiles but fails at runtime.
- Turn this manual release checklist into an agentic workflow with safe outputs.
- Debug a workflow that starts but doesn't create the expected issue comment.
- Improve workflow instructions so the agent makes consistent decisions.
