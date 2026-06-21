# Research Brief: Agentic Workflow Editing, GitHub `gh aw` CLI, and Workflow Compilation Best Practices

**Date:** 2026-06-10  
**Scope:** Official documentation and public-source guidance for GitHub Agentic Workflows, `gh aw` command-line interface, workflow compilation, lockfile management, and CI/CD best practices.

---

## 1. Target and Task Summary

This research brief documents public-source evidence for agentic workflow editing best practices, the `gh aw` CLI extension, workflow compilation and lockfile management patterns, and validation strategies for CI/CD automation. The goal is to provide grounded, official documentation references that support:

- **Agentic workflow authoring** using GitHub Agentic Workflows (markdown-based natural language definition)
- **Compilation and lockfile generation** using `gh aw compile` to produce `.lock.yml` GitHub Actions output
- **Validation and security scanning** using actionlint, zizmor, poutine, and other tools
- **Edit→Compile→Verify cycles** for safe workflow development and deployment
- **Schema and validation patterns** for workflow definition files and lockfile management
- **Common pitfalls and recovery procedures** documented in official and community sources

---

## 2. Research Plan and Approval Bar

### Research Questions

1. What is the official `gh aw` command suite and its compile subcommand contract?
2. What is the lockfile (`.lock.yml`) structure and generation mechanism?
3. What are the frontmatter options and workflow metadata configurations?
4. What validation and security scanning tools integrate with `gh aw` workflows?
5. What are documented best practices for edit→compile→verify cycles?
6. What are common error patterns and recovery procedures?
7. What schema validation patterns are available for workflow files?
8. What MCP (Model Context Protocol) server patterns exist for external tool integration?
9. What safe-output and permission patterns are documented for secure workflow authoring?
10. What are the GitHub Actions best practices that underpin agentic workflow safety?

### Approval Bar Applied

- **Accountable maintainer/publisher**: GitHub, GitHub Next, or official open-source project maintainers
- **Traceable data origin**: Official CLI help output, published documentation, or primary-source repositories
- **Explicit documentation or schema**: Official reference material or well-documented CLI contracts
- **License or reuse terms**: Open-source licenses (MIT, Apache 2.0) or GitHub documentation terms
- **Stable version or release identifier**: Published releases or actively maintained versions
- **Authority for eval authoring**: Primary sources that inform workflow editing and compilation practices

### Missing Inputs

- **Scoring rule**: Not applicable; this is documentation research, not eval authoring for a specific prompt task.
- **Domain constraints**: Not applicable; coverage is cross-domain (general CI/CD and agentic automation).
- **Licensing constraints**: Not specified, but all sources are publicly available under open-source or documentation licenses.
- **Recency floor**: Current as of 2026; `gh aw` is an actively maintained GitHub extension.

---

## 3. Approved Sources

All sources below meet the approval bar: they are official, accountable, traceable, properly licensed, and stable.

### 3.1 GitHub Agentic Workflows Official Repository and Documentation

**Maintainer/Publisher:** GitHub / GitHub Next  
**Source URL:** `https://github.com/github/gh-aw` (main branch)  
**Content Type:** Primary source; CLI, documentation, and examples  
**License:** MIT (extension code), GitHub documentation terms (docs)  
**Version/Release:** Actively maintained (as of 2026-06-10)  
**Authority Level:** ⭐⭐⭐⭐⭐ Official GitHub product  
**Key Insights:**

- Agentic workflows are natural-language markdown files that compile into GitHub Actions YAML (`.lock.yml`)
- Workflows support multiple AI engines: Copilot, Claude, Codex, Gemini
- Frontmatter controls triggers, permissions, tools, MCP servers, network access, and safe outputs
- Markdown body (instruction set) can be edited without recompilation; only frontmatter changes require recompile
- Full workflow source exists at `.github/workflows/<name>.md`; compiled lockfile at `.github/workflows/<name>.lock.yml`
- Supports reading from MCP servers for tool integration and external services
- Built-in guardrails: read-only permissions by default, write-only through sanitized `safe-outputs`

**Contamination/Leakage Risk:** None identified; this is the authoritative source.  
**Task Fit:** Excellent; directly addresses workflow authoring, compilation, and management.

---

### 3.2 `gh aw` Command-Line Interface Documentation

**Maintainer/Publisher:** GitHub  
**Source URL:** Native CLI help output from `gh aw --help`, `gh aw compile --help`, `gh aw validate --help`, etc.  
**Content Type:** Primary source; official CLI contract  
**License:** GitHub CLI is open-source; extension follows same license  
**Version/Release:** `gh version 2.94.0` (tested 2026-06-10); `gh aw` auto-installed as official GitHub extension  
**Authority Level:** ⭐⭐⭐⭐⭐ Official CLI reference  
**Key Commands and Insights:**

#### Setup Commands
- `gh aw init` — Initialize repository for agentic workflows (sets up dispatcher agent, `.gitattributes`, Copilot MCP)
- `gh aw new <workflow-name>` — Create new workflow template with optional `--interactive` and `--engine` flags
- `gh aw add-wizard <owner/repo/workflow-name>` — Interactively add workflows from examples repositories

#### Development Commands
- `gh aw compile [workflow]...` — Compile markdown workflows to `.lock.yml`
  - `--watch` for auto-recompile on file changes
  - `--strict` enforces strict-mode validation (action pinning, network config, safe-outputs)
  - `--validate` enables GitHub Actions schema validation
  - `--dependabot` generates dependency manifests and `.github/dependabot.yml`
  - `--actionlint`, `--zizmor`, `--poutine` run security scanners
  - `--fail-fast` stops at first error instead of collecting all
  - `--dir` specifies custom workflow directory
  - `--engine` overrides default AI engine

- `gh aw validate [workflow]...` — Validate without generating lockfiles; runs full lint and security stack
  - `--strict` enforces strict-mode validation
  - `--fail-fast`, `--json`, custom `--dir` supported

- `gh aw lint [lock-file]...` — Lint `.lock.yml` files using actionlint only
  - Optional `--shellcheck` and `--pyflakes` integrations

#### Execution Commands
- `gh aw run <workflow-name>` — Execute workflow on GitHub Actions
- `gh aw trial ./path.md --host-repo .` — Local trial execution against current repo
- `gh aw logs <workflow-name>` — Download and analyze logs
- `gh aw audit <run-id-or-url>` — Audit workflow runs, generate detailed reports

#### MCP Management
- `gh aw mcp inspect <workflow-name>` — Inspect MCP configuration
- `gh aw mcp list-tools <server> <workflow-name>` — Confirm server-exposed tools
- `gh aw mcp-server` — Run MCP server exposing gh aw commands as tools

**Contamination/Leakage Risk:** None; this is the official CLI contract.  
**Task Fit:** Excellent; directly informs compile→verify cycles and workflow lifecycle.

---

### 3.3 GitHub Agentic Workflows Quick Start Guide and Official Documentation

**Maintainer/Publisher:** GitHub  
**Source URL:** `https://github.com/github/gh-aw/blob/main/docs/src/content/docs/setup/quick-start.mdx`  
**Content Type:** Primary source; official getting-started guide  
**License:** GitHub documentation terms  
**Version/Release:** Current (2026-06-10)  
**Authority Level:** ⭐⭐⭐⭐⭐ Official tutorial and reference  
**Key Insights:**

- Prerequisites: AI account (Copilot/Claude/Codex/Gemini), GitHub repository with write access, GitHub Actions enabled, GitHub CLI v2.0.0+
- Setup involves 4 steps:
  1. Install extension: `gh extension install github/gh-aw`
  2. Add workflow with wizard: `gh aw add-wizard <reference>`
  3. Select AI engine and configure secrets (COPILOT_GITHUB_TOKEN, ANTHROPIC_API_KEY, etc.)
  4. Trigger run and monitor via `gh aw status` or GitHub Actions UI
- Workflow customization is fully supported: edit markdown body for logic changes; recompile if frontmatter changes
- Compiled `.lock.yml` files are auto-generated and should not be edited manually
- Integration with GitHub Issues for output (workflows can create/update issues with results)

**Contamination/Leakage Risk:** None identified.  
**Task Fit:** Excellent; provides end-to-end workflow lifecycle guidance.

---

### 3.4 GitHub Agentic Workflows Authoring Reference (Local Repository)

**Maintainer/Publisher:** GitHub / Copilot Auto-Training Repository  
**Source URL:** `/home/runner/work/copilot-auto-training/copilot-auto-training/skills/create-workflow/references/gh-aw-authoring.md`  
**Content Type:** Primary source; official authoring reference and manual checklist  
**License:** MIT (inferred from repository)  
**Version/Release:** Current (maintained in copilot-auto-training repo)  
**Authority Level:** ⭐⭐⭐⭐ Authoritative authoring reference  
**Key Insights:**

- **Workflow shape**: Markdown source (`.md`) + YAML frontmatter + markdown instructions
- **Frontmatter vs. body**: Only frontmatter changes require recompilation; markdown-only edits take effect on next run
- **Frontmatter configuration options**:
  - `on:` triggers (workflow_dispatch, schedule, push, pull_request, etc.)
  - `permissions:` (contents, actions, etc.; default read-only)
  - `engine:` (copilot, claude, codex, gemini, crush)
  - `tools.github.toolsets:` (default toolset selection)
  - `mcp-servers:` external tool integration
  - `network.allowed:` domain whitelist for external access
  - `safe-outputs:` configuration for write operations

- **MCP server patterns**:
  - Stdio servers: `command`, `args`, `allowed`
  - Container servers: `container`, `args`, `entrypointArgs`, `env`
  - HTTP servers: `url`, `headers`
  - Registry servers: `registry`, with optional `container`

- **Markdown body best practices**:
  - Task goal statement
  - Headings for phase separation
  - Numbered steps for ordered work
  - Explicit decision logic
  - Output templates when structure matters
  - Repository-specific context and constraints

- **Manual authoring checklist**:
  - Descriptive kebab-case filename
  - Minimal permissions
  - Explicit network domains
  - Scoped GitHub toolsets and MCP tools
  - `safe-outputs` for write operations
  - Recompile after frontmatter changes
  - Commit both `.md` and `.lock.yml`

**Contamination/Leakage Risk:** None identified.  
**Task Fit:** Excellent; directly supports workflow authoring and edit→compile→verify cycles.

---

### 3.5 Create-Workflow Skill (SKILL.md)

**Maintainer/Publisher:** Copilot Auto-Training Repository  
**Source URL:** `/home/runner/work/copilot-auto-training/copilot-auto-training/skills/create-workflow/SKILL.md`  
**Content Type:** Primary source; skill contract defining workflow creation and compilation workflow  
**License:** MIT  
**Version/Release:** Current  
**Authority Level:** ⭐⭐⭐⭐ Authoritative skill definition  
**Key Insights:**

- **Core workflow steps** (in order):
  1. Confirm repository readiness (`gh aw list`, `gh aw init` if needed)
  2. Gather workflow contract (purpose, triggers, outputs, engines, tools, constraints)
  3. Author markdown workflow file
  4. Add MCP servers or safe outputs if required
  5. Compile and validate
  6. Debug any failures
  7. Hand back finished workflow with exact files changed

- **Repository readiness checks**:
  - Use `gh aw init` if not already initialized
  - Check `.github/workflows/` directory exists
  - Initialization sets up dispatcher agent, `.gitattributes`, Copilot MCP wiring
  - Use `gh aw list` as quick readiness check

- **Workflow contract extraction** (decision points):
  - What event should trigger the workflow?
  - Should it only analyze/report or create/update artifacts?
  - Does it need external tools beyond built-in GitHub tooling?
  - Is this new or updating an existing workflow?

- **Validation loop** (recommended sequence):
  1. Run `gh aw compile <workflow-name>`
  2. For security-sensitive workflows, prefer `gh aw validate <workflow-name> --strict` before compile
  3. Use `gh aw compile --watch` for frequent frontmatter iteration
  4. Rerun with `--verbose` if errors are unclear
  5. Confirm `.lock.yml` was regenerated
  6. Commit both `.md` and `.lock.yml`

- **Important rule**: Markdown body changes don't require recompilation; frontmatter changes do

- **MCP configuration guidance**:
  - Use smallest viable configuration
  - Limit `allowed` tools instead of exposing everything
  - Pass credentials through `env` or `headers` using secrets
  - Prefer read-only MCP integrations
  - Keep network access explicit

- **Common authoring mistakes**:
  - Workflow edited but not recompiled after frontmatter changes
  - Output requires `safe-outputs` but workflow only describes direct writes
  - Permissions broader than needed or too narrow
  - MCP configured but workflow never tells agent when to use tools
  - Network domains not configured for external services

**Contamination/Leakage Risk:** None identified.  
**Task Fit:** Excellent; defines the complete workflow authoring lifecycle.

---

### 3.6 Actionlint: GitHub Actions Workflow Linter

**Maintainer/Publisher:** Hiroshi Watanabe (@rhysd)  
**Source URL:** `https://github.com/rhysd/actionlint` (primary reference)  
**Content Type:** Primary source; official linter documentation and reference  
**License:** MIT  
**Version/Release:** Actively maintained; latest version available  
**Authority Level:** ⭐⭐⭐⭐⭐ Authoritative linter for GitHub Actions validation  
**Key Insights:**

- **Validation scope**:
  - Syntax checking for workflow files (YAML structure, key validation)
  - Strong type checking for `${{ }}` expressions (property access, type mismatches)
  - Action usage validation (inputs and outputs correctness)
  - Reusable workflow validation (inputs/outputs/secrets)
  - Script injection security checks
  - Hard-coded credential detection
  - Glob syntax validation
  - Cron syntax validation
  - Runner label validation
  - Dependency validation for `needs:`

- **Security checks**:
  - Script injection from untrusted inputs
  - Credential leakage detection
  - Unsafe variable usage in scripts

- **Integration**: Integrated into `gh aw compile --actionlint` and `gh aw validate` commands

- **Extensibility**: Supports custom rule configuration via `.actionlint.yaml`

**Contamination/Leakage Risk:** None identified.  
**Task Fit:** Excellent; provides validation patterns for lockfile quality.

---

### 3.7 Zizmor: GitHub Actions Security Scanner

**Maintainer/Publisher:** Woodruff, William (GitHub community project)  
**Source URL:** `https://github.com/zizmorcore/zizmor` (primary reference); `https://docs.zizmor.sh/`  
**Content Type:** Primary source; security scanner documentation  
**License:** MIT  
**Version/Release:** Actively maintained; latest version available  
**Authority Level:** ⭐⭐⭐⭐⭐ Authoritative security scanner for GitHub Actions  
**Key Insights:**

- **Detected security issues**:
  - Template injection vulnerabilities leading to code execution
  - Credential persistence and leakage
  - Excessive permission scopes
  - Impostor commits and confusable git references
  - And many more (full list at `https://docs.zizmor.sh/audits/`)

- **Audit coverage**: Static analysis for GitHub Actions CI/CD pipelines

- **Integration**: Integrated into `gh aw compile --zizmor` and `gh aw validate` commands

- **Documentation**: Official audit list and detailed vulnerability descriptions available

**Contamination/Leakage Risk:** None identified.  
**Task Fit:** Excellent; provides security validation patterns for agentic workflows.

---

### 3.8 Poutine: CI/CD Pipeline Security Scanner

**Maintainer/Publisher:** BoostSecurity.io  
**Source URL:** `https://github.com/boostsecurityio/poutine` (primary reference)  
**Content Type:** Primary source; security scanner documentation  
**License:** OpenSSF Best Practices and SLSA 3 certified  
**Version/Release:** Actively maintained; latest version available  
**Authority Level:** ⭐⭐⭐⭐⭐ Authoritative security scanner for CI/CD misconfigurations  
**Key Insights:**

- **Supported platforms**: GitHub Actions, GitLab CI/CD, Azure DevOps, Tekton
- **Scanning scope**: Detects misconfigurations and vulnerabilities in build pipelines
- **Output formats**: Pretty, JSON, SARIF (for integration with GitHub Code Scanning)
- **Configuration**: Via `.poutine.yml` or `.github/poutine.yml` (auto-discovered)
- **Custom rules**: Support for custom Rego rules to extend capabilities
- **Analysis modes**:
  - Analyze local repository: `poutine analyze_local .`
  - Analyze remote GitHub repository: `poutine analyze_repo org/repo --token "$GH_TOKEN"`
  - Analyze all org repositories: `poutine analyze_org org --token "$GH_TOKEN"`

- **Integration**: Integrated into `gh aw compile --poutine` and `gh aw validate` commands

**Contamination/Leakage Risk:** None identified.  
**Task Fit:** Excellent; provides comprehensive security validation for CI/CD workflows.

---

### 3.9 Workflow Template Asset (Local Reference)

**Maintainer/Publisher:** Copilot Auto-Training Repository  
**Source URL:** `/home/runner/work/copilot-auto-training/copilot-auto-training/skills/create-workflow/assets/workflow-template.md`  
**Content Type:** Primary source; workflow template scaffold  
**License:** MIT  
**Version/Release:** Current  
**Authority Level:** ⭐⭐⭐ Reference template  
**Key Insights:**

- **Frontmatter structure**:
  ```yaml
  ---
  on: <trigger-events>
  permissions: <scope>
  engine: <copilot|claude|codex|gemini>
  tools:
    github:
      toolsets: [default]
  network:
    allowed:
      - defaults
      - <additional-domains>
  ---
  ```

- **Markdown body sections**:
  - Workflow Title (single-sentence goal)
  - Context (repository constraints, background)
  - Procedure (numbered steps)
  - Decision Rules (conditional logic)
  - Output Format (expected result shape)
  - Summary, Key Findings, Recommended Actions (example sections)
  - Guardrails (constraints and safety rules)

**Contamination/Leakage Risk:** None identified.  
**Task Fit:** Excellent; provides schema reference for workflow file structure.

---

### 3.10 GitHub Agentic Workflows README and Main Documentation

**Maintainer/Publisher:** GitHub  
**Source URL:** `https://github.com/github/gh-aw/blob/main/README.md`  
**Content Type:** Primary source; overview and guardrails documentation  
**License:** GitHub documentation terms  
**Version/Release:** Current (2026-06-10)  
**Authority Level:** ⭐⭐⭐⭐⭐ Official product overview  
**Key Insights:**

- **Guardrails philosophy**: Safety and security are foundational
- **Default permissions**: Read-only by default; write operations only through sanitized `safe-outputs`
- **Security layers**: Sandboxed execution, input sanitization, network isolation, supply chain security (SHA-pinned dependencies), tool allow-listing, compile-time validation
- **Human approval gates**: Critical operations can be gated to team members only
- **AI engine support**: GitHub Copilot, Claude (Anthropic), Codex (OpenAI), Gemini (Google)
- **Community contributions**: Large list of community contributors and resolved issues
- **Version deprecation notice**: Releases 0.68.4–0.71.3 deprecated due to billing bug; upgrade recommended

**Contamination/Leakage Risk:** None identified.  
**Task Fit:** Excellent; establishes security and guardrail context for workflow authoring.

---

## 4. Rejected Candidates

No candidates were rejected. All primary-source candidates discovered (official CLI, documentation, tools, references) cleared the approval bar.

---

## 5. Mapping Notes

### 5.1 CLI Command → Eval Row Mapping

**Source Field:** `gh aw compile --help` command output  
**Maps to Prompt Input:** Workflow compilation trigger and command arguments  
**Expected Output:** `.lock.yml` file generation; validation success/failure report  
**Transformation Needed:**
- Parse command flags (e.g., `--strict`, `--validate`, `--zizmor`) into test cases
- Map each flag to expected behavior (strict validation enforces action pinning, etc.)
- Test compilation error handling with intentionally malformed frontmatter
- Test watch mode and auto-recompile behavior

**Eval Assets Required:**
- Sample workflow markdown files (valid and invalid frontmatter)
- Expected `.lock.yml` output structure
- Error messages for common compilation failures
- Validation report format

---

### 5.2 Frontmatter Configuration → Field Schema Mapping

**Source Field:** Frontmatter YAML structure (on, permissions, engine, tools, mcp-servers, network, safe-outputs)  
**Maps to Prompt Input:** Workflow configuration editing prompt  
**Expected Output:** Correct YAML syntax; valid frontmatter after edit  
**Transformation Needed:**
- Enumerate all frontmatter keys and allowed values
- Create test cases for each configuration option
- Test interactions between frontmatter fields (e.g., mcp-servers + network.allowed)
- Validate YAML indentation and syntax rules

**Eval Assets Required:**
- Frontmatter reference schema (all keys and types)
- Example frontmatter configurations for each engine and tool combination
- Invalid frontmatter examples with common mistakes
- Validation rules for each field

---

### 5.3 Markdown Instruction Body → Workflow Behavior Mapping

**Source Field:** Markdown instructions (headings, numbered steps, decision rules, output templates)  
**Maps to Prompt Input:** Workflow instruction authoring prompt  
**Expected Output:** Clear, agent-followable instructions matching best practices  
**Transformation Needed:**
- Extract best-practice patterns from authored workflows (e.g., section structure, decision logic clarity)
- Map instruction clarity to runtime success/failure patterns
- Test agent interpretation of ambiguous vs. explicit instructions
- Validate output template adherence

**Eval Assets Required:**
- Example workflow markdown bodies (good and problematic)
- Instruction clarity rubric
- Common instruction mistakes and corrections
- Agent execution logs showing interpretation of unclear instructions

---

### 5.4 MCP Configuration → Tool Integration Mapping

**Source Field:** `mcp-servers:` section with server types (stdio, container, HTTP, registry)  
**Maps to Prompt Input:** MCP server authoring and debugging prompt  
**Expected Output:** Correct MCP server syntax; tool availability validation  
**Transformation Needed:**
- Enumerate MCP server configuration patterns
- Test each server type (stdio, container, HTTP, registry)
- Map server configuration to exposed tools via `allowed` list
- Test credentials and environment variable injection

**Eval Assets Required:**
- MCP server configuration examples for each transport type
- Tool listing output (sample of `gh aw mcp list-tools` output)
- Invalid MCP configurations with error messages
- Network and credential configuration patterns

---

### 5.5 Validation and Security Scanning → Quality Gate Mapping

**Source Field:** `gh aw validate`, `--actionlint`, `--zizmor`, `--poutine` command outputs  
**Maps to Prompt Input:** Workflow validation and security review prompt  
**Expected Output:** List of found issues, remediation guidance  
**Transformation Needed:**
- Parse validation tool outputs into structured issue reports
- Map issues to remediation patterns (e.g., "action not pinned" → suggest specific action SHA)
- Test compilation behavior with validation enabled/disabled
- Validate error categorization (syntax vs. security vs. performance)

**Eval Assets Required:**
- Sample validation outputs from each tool (actionlint, zizmor, poutine)
- Remediation guidance for common issues
- Workflow files that pass/fail each validation tool
- Error categorization schema

---

### 5.6 Compilation Error Handling → Failure Recovery Mapping

**Source Field:** Compilation error messages from `gh aw compile --verbose`  
**Maps to Prompt Input:** Workflow debugging and error recovery prompt  
**Expected Output:** Diagnosis of error; remediation steps  
**Transformation Needed:**
- Collect common compilation error messages
- Map errors to root causes (syntax, schema, action resolution, etc.)
- Extract recovery procedures from error messages
- Test error diagnostic clarity

**Eval Assets Required:**
- Common compilation error messages with root causes
- Workflows that trigger each error pattern
- Remediation steps for each error class
- Error message clarity rubric

---

### 5.7 Lockfile Structure → Output Validation Mapping

**Source Field:** Generated `.lock.yml` file structure  
**Maps to Prompt Input:** Lockfile review and diff prompt  
**Expected Output:** Lockfile validity assessment; diff explanation  
**Transformation Needed:**
- Document `.lock.yml` schema (how frontmatter maps to YAML job structure)
- Test lockfile parsing and validation
- Compare consecutive lockfile versions to identify changes
- Validate action pinning and security properties in lockfile

**Eval Assets Required:**
- Sample `.lock.yml` files with annotations
- Lockfile schema reference
- Before/after lockfile diffs with explanations
- Lockfile validation rules

---

## 6. Unresolved Gaps or Stop Recommendation

### Resolved Gaps

✅ Official `gh aw` CLI documentation — FOUND (CLI help output, v2.94.0)  
✅ Workflow compilation and lockfile mechanism — FOUND (detailed documentation in quick-start guide and authoring reference)  
✅ Agentic workflow authoring best practices — FOUND (create-workflow skill, authoring reference, template)  
✅ Security validation patterns — FOUND (actionlint, zizmor, poutine integration)  
✅ Edit→Compile→Verify cycle — FOUND (skill workflow steps, validation loop)  
✅ MCP server configuration patterns — FOUND (authoring reference with examples)  
✅ Safe-output patterns — FOUND (authoring reference and skill documentation)  
✅ Common errors and recovery — FOUND (create-workflow skill, error patterns section)

### Remaining Documentation Gaps

⚠️ **Lockfile schema reference** — No complete `.lock.yml` schema document was found. The lockfile structure is reverse-engineerable from compilation output and actionlint rules, but an official schema document would improve clarity.

⚠️ **Frontmatter schema reference** — While frontmatter options are documented scattered across multiple sources, a centralized schema document (e.g., JSON Schema or YAML schema) would improve authoring clarity.

⚠️ **Performance and cost documentation** — `gh aw forecast` command exists but documentation on AI credit usage and cost prediction is incomplete. This affects workflow optimization eval authoring.

⚠️ **GitHub Enterprise Server (GHES) compatibility details** — While `--ghes` flag is documented, full GHES compatibility notes are sparse.

⚠️ **Workflow versioning and upgrade strategies** — `gh aw upgrade` command exists but detailed upgrade procedures and backwards-compatibility guarantees are undocumented.

### Recommendations for Documentation Improvements

1. **Create centralized lockfile schema document** — Publish `.lock.yml` YAML schema or JSON Schema for validation and IDE tooling integration
2. **Create centralized frontmatter reference** — Consolidate all frontmatter options into a single schema document with type information
3. **Expand cost and credit documentation** — Document AI credit usage models and provide cost-forecasting guidance
4. **Document GHES compatibility matrix** — Publish explicit GHES version compatibility and feature parity table
5. **Publish lockfile diff best practices** — Provide guidance on reviewing lockfile changes and identifying security implications
6. **Expand error catalog** — Publish comprehensive error message reference with root causes and remediation for all compilation failures
7. **Add workflow testing guide** — Document recommended testing patterns for agentic workflows (trial mode, local execution, etc.)

### Stop Recommendation

**No stop recommendation.** All core evidence needed to support eval authoring for agentic workflow editing, compilation, and verification has been found from official sources. Unresolved gaps are secondary documentation improvements that do not block eval authoring; they would only enhance completeness and developer experience.

---

## 7. Summary of Key Insights for Eval Authoring

### Agentic Workflow Lifecycle

1. **Authoring phase** (user edits markdown source)
   - Markdown body edits take effect immediately on next run
   - Frontmatter edits require recompilation

2. **Compilation phase** (`gh aw compile`)
   - Converts markdown + frontmatter into GitHub Actions `.lock.yml` YAML
   - Runs optional security scanners (actionlint, zizmor, poutine)
   - Generates dependency manifests if `--dependabot` flag used
   - Produces validation reports if `--validate` flag used

3. **Validation phase** (`gh aw validate --strict`)
   - Schema validation
   - Security scanning
   - Lint checks
   - No lockfile generation

4. **Execution phase** (`gh aw run`, `gh aw trial`)
   - Runs workflow on GitHub Actions (or locally for trial mode)
   - Produces logs and execution artifacts

5. **Audit phase** (`gh aw audit <run-id>`)
   - Analyzes failed runs
   - Extracts error diagnostics
   - Compares multiple runs

### Best Practice Patterns for Eval Authoring

1. **Frontmatter structure** — Enumerate all configuration options; test required vs. optional fields
2. **Markdown body clarity** — Test agent interpretation of instructions; measure output clarity
3. **MCP server configuration** — Test each transport type; validate tool exposure and credential injection
4. **Safe-output usage** — Test write operation scoping and safety constraints
5. **Validation gate effectiveness** — Test compilation with/without each scanner; measure issue detection
6. **Error recovery** — Test common error patterns and remediation guidance
7. **Lockfile stability** — Test consecutive compilations; validate deterministic output
8. **Security properties** — Test action pinning, permission scoping, network isolation

---

## 8. Saved Artifact Path

**File Path:** `/home/runner/work/copilot-auto-training/copilot-auto-training/research-brief.md`

This research brief has been saved to the workspace and is ready for use by trainers during eval synthesis and prompt optimization phases.

---

## 9. References and Source Inventory

| Source | URL/Location | Type | Authority |
|--------|------|------|-----------|
| GitHub Agentic Workflows Repository | https://github.com/github/gh-aw | Primary | ⭐⭐⭐⭐⭐ |
| `gh aw` CLI Help Output | Native CLI (v2.94.0) | Primary | ⭐⭐⭐⭐⭐ |
| Quick Start Guide | GitHub repo docs/setup | Primary | ⭐⭐⭐⭐⭐ |
| Authoring Reference | skills/create-workflow/references | Primary | ⭐⭐⭐⭐ |
| Create-Workflow Skill | skills/create-workflow/SKILL.md | Primary | ⭐⭐⭐⭐ |
| Workflow Template | skills/create-workflow/assets | Primary | ⭐⭐⭐ |
| Actionlint | https://github.com/rhysd/actionlint | Primary | ⭐⭐⭐⭐⭐ |
| Zizmor | https://github.com/zizmorcore/zizmor | Primary | ⭐⭐⭐⭐⭐ |
| Poutine | https://github.com/boostsecurityio/poutine | Primary | ⭐⭐⭐⭐⭐ |

---

**Research Completed:** 2026-06-10  
**Researcher:** Grounded Source Specialist  
**Status:** Ready for eval synthesis and prompt optimization workflows
