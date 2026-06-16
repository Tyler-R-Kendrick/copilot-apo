# Research Brief: Create-Workflow Skill Training Dataset Requirements

## Target and Task Summary

The `create-workflow` skill teaches GitHub Copilot agents how to create and validate GitHub Agentic Workflows (`.github/workflows/*.md` files). The skill covers repository readiness checks, workflow contract gathering, markdown authoring (frontmatter + body), MCP server integration, safe-output configuration, compilation, and validation using `gh aw` CLI. The task is to identify existing eval cases, benchmark workflow examples, failure patterns, and reference documentation that can support training dataset synthesis. Success is measured by whether the agent can take a user request and produce a well-formed, compilable, and validated `.md` workflow file with correct frontmatter, clear instructions, scoped permissions, and safe-output guards where needed. Scope: GitHub Agentic Workflows only; generic GitHub Actions YAML is out of scope.

---

## Research Plan and Approval Bar

### Primary Sources Identified
1. **In-repository skill contract and references** — SKILL.md (canonical), gh-aw-authoring.md (reference), workflow-template.md (template), existing workflows in `.github/workflows/`
2. **Trainer workspace checkpoint** — engineer-prompt review, workflow-status.json, existing training context
3. **Live workflow examples** — train-prompt.md, update-docs.md (compiled and lock-file paired)
4. **Related documentation** — docs/getting-started.md, docs/troubleshooting.md, agentic-workflow-editing.instructions.md
5. **Comparable eval manifests** — engineer-copilot-agent/evals.json, trainer-train-prompt/evals.json (same repository for structural guidance)

### Approval Bar Applied
- **Accountable maintainer**: All sources are repository-owned; no third-party external sources required.
- **Traceable data origin**: Skill contract, references, and templates are all tracked in git with clear versioning.
- **Evaluation rules**: SKILL.md defines success criteria (compilation, validation, schema compliance). Engineer review captured existing failure modes. Trainer loop contract defines workspace and scoring rules.
- **Explicit license**: Repository LICENSE applies (inherited); all materials are internal development assets.
- **Stable version/date**: All sources as of repository head; agentic-workflow-editing.instructions.md enforces lockfile sync post-compilation.
- **Contamination/leakage risk**: Low; examples are production workflows from this repository. No private data or secrets exposed in markdown or examples.

### Missing Constraints (Noted, Non-Blocking)
- **Recency**: No explicit version floor stated; using current repository state.
- **Domain-specific taxonomy**: MCP server types, safe-output types, and GitHub trigger types are documented inline in references but not indexed separately.
- **Scoring precision**: Skill requires "compilable" and "validated" but does not specify exact JSONL row structure or rubric beyond "passes `gh aw validate --strict`"; synthesis stage will need to define rubric details.

---

## Approved Sources

### 1. SKILL.md (Canonical Skill Contract)
**Publisher**: Repository (Tyler-R-Kendrick/copilot-auto-training)  
**Origin**: `skills/create-workflow/SKILL.md`  
**Frontmatter**: N/A (markdown document, not data)  
**License**: Repository LICENSE (MIT-like)  
**Version/Date**: Repository head (June 2024)  
**Task Fit**: Defines when to use the skill, core 7-step workflow, quality bar, response contract, and example request shapes. Authoritative scope boundary (excludes generic GitHub Actions YAML). Failure modes documented.  
**Risk**: None identified. Well-maintained, actively used by other skills and workflows.

**Field Mapping for Eval Synthesis**:
- Section "When to use" → positive case definitions (new workflow, convert process, add MCP, debug)
- Section "Do not use..." → negative cases (generic GitHub Actions YAML)
- Section "Core workflow" steps 1–7 → success criteria sequence
- Section "Quality bar" (lines 184–194) → rubric checklist (filename, frontmatter, markdown body, safe-outputs, MCP config, lock.yml, recompile guidance)
- Section "Example request shapes" → eval prompt templates
- Section "Common authoring mistakes" (lines 176–182) → failure patterns for rubric

### 2. gh-aw-authoring.md (Authoring Reference)
**Publisher**: Repository (Tyler-R-Kendrick/copilot-auto-training)  
**Origin**: `skills/create-workflow/references/gh-aw-authoring.md`  
**Frontmatter**: N/A (markdown reference document)  
**License**: Repository LICENSE  
**Version/Date**: Repository head  
**Task Fit**: Detailed authoring patterns for frontmatter (trigger, permissions, MCP servers, safe outputs, network config), markdown body best practices, MCP configuration syntax (stdio, container, HTTP, registry), safe-output reminder, common failure patterns, and final handoff checklist. Provides concrete syntax examples.  
**Risk**: None identified. Read-only reference; clearly separates frontmatter from markdown body concerns.

**Field Mapping for Eval Synthesis**:
- Section "Workflow shape" → structure validation (source .md, compiled .lock.yml, frontmatter marker, markdown body)
- Section "MCP configuration summary" → MCP setup patterns (stdio, container, HTTP, registry); examples for eval synthesis
- Section "Safe outputs reminder" → safe-output requirement detection (issue, comment, label, PR updates)
- Section "Common failure patterns" → rubric checks (YAML indentation, frontmatter fields, lock file generation, MCP connection, tool discovery, write actions)
- Sections on manual authoring checklist → validation requirements

### 3. workflow-template.md (Starter Template)
**Publisher**: Repository (Tyler-R-Kendrick/copilot-auto-training)  
**Origin**: `skills/create-workflow/assets/workflow-template.md`  
**Frontmatter**: YAML frontmatter with `on:`, `permissions:`, `engine:`, `tools:`, `network:`; markdown body with headings (Workflow Title, Context, Procedure, Decision Rules, Output Format, Guardrails)  
**License**: Repository LICENSE  
**Version/Date**: Repository head  
**Task Fit**: Minimal but complete starter template for new workflows. Defines canonical frontmatter structure and markdown section order. Good reference for eval prompts that test "create a new workflow from scratch."  
**Risk**: Low. Template is intentionally minimal; production workflows expand these sections as needed.

**Field Mapping for Eval Synthesis**:
- Frontmatter structure → expected minimal valid frontmatter
- Markdown section headings → expected markdown body structure
- Template comments and guardrails section → expected tone and safety guidance

### 4. Live Workflow Examples (train-prompt.md, update-docs.md)
**Publisher**: Repository (Tyler-R-Kendrick/copilot-auto-training)  
**Origin**: `.github/workflows/train-prompt.md` and `.github/workflows/update-docs.md` (source and compiled `.lock.yml` pairs)  
**Frontmatter**: Full examples: triggers (schedule, workflow_dispatch, pull_request_target), imports, permissions, descriptions, labels, engine, tools, MCP (optional), safe-outputs, timeouts, steps  
**License**: Repository LICENSE  
**Version/Date**: Repository head (compiled with gh-aw v0.68.1)  
**Task Fit**: Production workflows with complexity: train-prompt.md includes imports, schedule triggers, MCP bootstrap validation, safe-output config (create-pull-request, noop), and multi-step procedures. update-docs.md demonstrates pull_request_target trigger, conditional logic, and safe-output noop. Both have compiled `.lock.yml` pairs showing successful compilation state.  
**Risk**: Low. Both workflows are actively maintained and tested. Lockfile timestamps show recent compilation. Safe to use as success-criteria examples.

**Field Mapping for Eval Synthesis**:
- train-prompt.md frontmatter lines 2–23 → complex trigger/permission/import pattern
- update-docs.md frontmatter lines 2–41 → pull_request_target + safe-outputs example
- Both lock.yml files → expected compilation output (metadata header, manifest, secret list)
- Safe-outputs sections → detection of write-back requirements; pattern for noop when no writes needed

### 5. Engineer-Prompt Review (create-workflow workspace checkpoint)
**Publisher**: Repository trainer workspace (created as training input)  
**Origin**: `skills/create-workflow/.trainer-workspace/SKILL/engineer-prompt/review.md`  
**Frontmatter**: N/A (markdown artifact, not data)  
**License**: Repository LICENSE  
**Version/Date**: Generated during trainer initialization  
**Task Fit**: Identified current failure modes (scope creep, repo readiness, frontmatter complexity, workflow structure clarity, debugging isolation, safe-output scope), dataset gaps (MCP config examples, debugging examples, when MCP is overkill, workflow body quality), and optimization hypothesis (readiness checklist, MCP patterns, debugging decision trees, body instruction quality).  
**Risk**: None identified. This is a working artifact from the current skill training cycle; it captures known weaknesses.

**Field Mapping for Eval Synthesis**:
- "Likely Failure Modes" (6 bullet points) → negative test cases for rubric
- "Dataset Gaps" → synthesis guidance: add MCP worked examples, debugging examples, overkill-vs.-necessary decision rules
- "Validation Plan" (5 items) → eval objectives
- "Next Optimization Hypothesis" → guidance for eval rubric focus areas

### 6. Trainer Loop Contract (shared workflow guidance)
**Publisher**: Repository (Tyler-R-Kendrick/copilot-auto-training)  
**Origin**: `.github/workflows/shared/trainer-loop-contract.md`  
**Frontmatter**: N/A (procedural contract, not data)  
**License**: Repository LICENSE  
**Version/Date**: Repository head  
**Task Fit**: Defines workspace layout, skill execution contract, dataset rules, judge mode rules (llm_judge vs. deterministic vs. custom), and optimize output contract. Provides context for how eval rows will be consumed (which judge_mode applies to workflow creation tasks, how to structure expected outputs, artifact checkpoint requirements).  
**Risk**: None identified. Read-only reference; defines trainer infrastructure expectations.

**Field Mapping for Eval Synthesis**:
- "Dataset And Judge Mode Rules" → eval row structure guidance (judge_mode selection, scoring field rules, reference + criteria for open-ended tasks)
- "Optimize Output Contract" → expected output structure (optimized-prompt.md + report.json, artifact naming)

### 7. Agentic Workflow Editing Instructions
**Publisher**: Repository (GitHub.com copilot-auto-training)  
**Origin**: `.github/instructions/agentic-workflow-editing.instructions.md`  
**Frontmatter**: Front matter with `description`, `applyTo` (pattern: `.github/workflows/*.md`)  
**License**: Repository LICENSE  
**Version/Date**: Repository head  
**Task Fit**: Post-edit enforcement: compilation requirement, lockfile sync, and hook-backed validation. Clarifies that compilation must happen after every edit and before PR. Useful for eval prompts that test "edit an existing workflow" and "check if compilation is required."  
**Risk**: Low. This is a contributor-facing instruction; helps define when recompilation is necessary.

**Field Mapping for Eval Synthesis**:
- "After editing any .github/workflows/*.md file, run `gh aw compile`" → recompilation requirement rule
- Hook enforcement → validation gate after edits
- Guidance on `.md` and `.lock.yml` drift → warning about stale pairs

### 8. Comparable Eval Manifests (engineer-copilot-agent, trainer-train-prompt)
**Publisher**: Repository skills (same repo)  
**Origin**: `skills/engineer-copilot-agent/evals/evals.json` (5 rows), `skills/trainer-train-prompt/evals/evals.json` (4 rows)  
**Frontmatter**: Standard evals.json structure with `skill_name`, `evals` array, row fields: `id`, `prompt`, `expected_output`, `assertions`, `scoring`, `criteria`  
**License**: Repository LICENSE  
**Version/Date**: Repository head  
**Task Fit**: Structural reference for how eval rows are authored in this repository. Demonstrates scoring modes (`llm_judge`), assertion-based checking, and criteria fields for open-ended output validation. Not domain-specific to create-workflow but shows the data format and rubric style expected downstream.  
**Risk**: None identified. Eval format is standardized across the repository.

**Field Mapping for Eval Synthesis**:
- Row structure (id, prompt, expected_output, assertions, scoring, criteria) → template for new eval rows
- `scoring: llm_judge` with `criteria` field → format for open-ended workflow validation tasks
- Assertion structure (bullet-list, specific validation checks) → rubric pattern for synthesis

### 9. Workflow-Status.json (Training State Checkpoint)
**Publisher**: Repository trainer workspace  
**Origin**: `skills/create-workflow/.trainer-workspace/SKILL/workflow-status.json`  
**Frontmatter**: N/A (JSON data)  
**License**: Repository LICENSE  
**Version/Date**: Generated during trainer initialization (June 2024)  
**Task Fit**: Shows trainer workspace state, required_artifacts expectations, and links to engineer review artifact. Confirms that create-workflow skill is a training target with no existing eval_manifest; synthesis will start from scratch.  
**Risk**: None identified. This is a data artifact, not mutable config.

**Field Mapping for Eval Synthesis**:
- `eval_manifest: null` → confirms no pre-existing evals.json; synthesis should create one under `skills/create-workflow/evals/evals.json`
- `required_artifacts` fields → expected output checkpoint structure after synthesis

---

## Rejected Candidates

**None identified.**  
All primary in-repository sources cleared the approval bar. No external third-party datasets or benchmarks were evaluated; research scope was limited to internal documentation and examples.

---

## Mapping Notes

### Eval Row Synthesis Guidance

The following table maps approved source content to eval row fields. These mappings inform how the synthesis stage will author new eval rows under `skills/create-workflow/evals/evals.json`.

| Source Section | → | Eval Row Field | Mapping Notes |
|---|---|---|---|
| SKILL.md "When to use" | → | `prompt` | Use request shapes from lines 32–36 and example request shapes from lines 205–211 as positive test cases. Extract domain (new workflow, MCP setup, debugging, conversion) to build scenario variants. |
| SKILL.md "Do not use..." | → | `prompt` | Negative case: user asks for generic GitHub Actions YAML. Eval should test that the skill boundary is enforced. |
| SKILL.md "Core workflow" steps 1–7 | → | `expected_output`, `criteria` | Expected output should demonstrate completion of all 7 steps: readiness check output, workflow contract collection, markdown file creation, compilation success, validation pass, debug artifacts (if applicable), and response contract (summarize trigger, behavior, tools, validation command). |
| SKILL.md "Quality bar" (lines 184–194) | → | `assertions` | Each assertion should check one bar item: filename kebab-case, frontmatter minimal but complete, markdown specific/structured, safe-outputs used for writes, MCP scoped, lock.yml present and synced, recompile guidance clear. |
| SKILL.md "Common authoring mistakes" (lines 176–182) | → | Negative cases / `expected_output` | Test that the skill avoids these mistakes: not recompiling after frontmatter changes, forgetting safe-outputs for writes, over-broad permissions, MCP config present but unused in instructions, missing network domains. |
| gh-aw-authoring.md "MCP configuration summary" | → | `prompt` | Author eval cases for each MCP transport: stdio server, container server, HTTP server, registry-backed server. Test that agent chooses the right transport pattern for the user request. |
| gh-aw-authoring.md "Safe outputs reminder" | → | `prompt` | Test that agent detects write-back requirements (issue creation, PR updates, label changes) and configures safe-outputs type accordingly. |
| gh-aw-authoring.md "Common failure patterns" | → | `assertions` | Check that workflow compilation succeeds, lock file is generated, frontmatter fields are present and spelled correctly, MCP server connection succeeds, tools are exposed as expected, write actions use safe-outputs. |
| workflow-template.md structure | → | `expected_output` | Expected markdown body should follow template structure: title, context, procedure, decision rules, output format, guardrails. Frontmatter should include required `on:`, `permissions:`, `engine:`, `tools:`, `network:`. |
| train-prompt.md, update-docs.md | → | `expected_output` | Example compiled workflows with lock.yml pairs. Use as reference for what "correct" frontmatter + compilation looks like. Test cases that require complex triggers (schedule, workflow_dispatch, pull_request_target), imports, or safe-outputs should reference these as success patterns. |
| Engineer review "Failure Modes" | → | Negative cases / rubric sensitivity | Incorporate each failure mode into at least one eval case: scope creep (test rejection of generic Actions), repo readiness (test detection of uninitialized repo), frontmatter complexity (test MCP setup guidance), workflow structure clarity (test body vs. frontmatter separation), debugging isolation (test categorization of error types), safe-output scope (test detection of write requirements). |
| Engineer review "Dataset Gaps" | → | Synthesis guidance | Plan to author cases for: (1) MCP configuration worked examples (at least one per transport type); (2) debugging examples (at least one per common error category); (3) when MCP is overkill vs. necessary; (4) workflow body instruction quality (specificity, clarity, branching logic). |
| Trainer loop contract "Judge Mode Rules" | → | `scoring`, `criteria` | Use `judge_mode=llm_judge` with `reference` + `criteria` for open-ended workflow authoring tasks. Use `judge_mode=deterministic` only for exact-match compilation checks (e.g., "compiled without error"). |
| Comparable evals (engineer-copilot-agent, trainer-train-prompt) | → | Row structure | Follow JSONL row template: `id`, `prompt`, `expected_output`, `assertions` (list), `scoring` ("llm_judge" for open-ended), `criteria` (description of what makes output acceptable). |

### Assets Required for Evals Workflow

- **`skills/create-workflow/evals/evals.json`** — New file to be created during synthesis. Template: standard evals.json with `skill_name: "create-workflow"` and `evals: [...]` array of row objects.
- **`skills/create-workflow/assets/workflow-template.md`** — Already exists; referenced by synthesis for expected markdown structure.
- **`skills/create-workflow/references/gh-aw-authoring.md`** — Already exists; referenced by synthesis for MCP and safe-output pattern examples.
- **Repository workflows (`.github/workflows/train-prompt.md`, `.update-docs.md`)** — Already exist; can be linked in eval row descriptions as "success examples" or linked in expected output for reference.

### Rubric Focus Areas (from Engineer Review)

The synthesis stage should emphasize evals that test:

1. **Repo readiness detection** — Does the skill check `gh aw list` and detect uninitialized repos? Does it route users to `gh aw init` or `install.md` when needed?
2. **Workflow contract gathering** — Does the skill ask focused questions to extract trigger, permissions, outputs, engine, tools, MCP, and write requirements before drafting?
3. **Frontmatter vs. markdown body clarity** — Does the skill explain when frontmatter (config) changes require recompilation vs. markdown-only (instruction) changes that take effect immediately?
4. **MCP configuration scoping** — When MCP is requested, does the skill suggest minimal viable configuration, scoped `allowed` lists, and secrets-aware env/header fields?
5. **Safe-output requirement detection** — Does the skill detect write-back scenarios and configure the correct safe-output type (create-issue, create-comment, update-label, create-pull-request)?
6. **Debugging categorization** — When a workflow fails, does the skill route to the correct diagnostic (compilation error → frontmatter fix; MCP failure → server/tool check; runtime failure → logs/audit)?
7. **Boundary enforcement** — Does the skill reject generic GitHub Actions YAML requests and stay scoped to agentic workflows only?

---

## Unresolved Gaps or Stop Recommendation

### Gaps Identified (Non-Blocking)

1. **Judge mode precision for mixed output types**: Some eval rows may produce both a workflow file (deterministic compilation) and instruction clarity (open-ended). The trainer contract suggests `judge_mode=llm_judge` for open-ended rows, but synthesis stage may need to split these into two rows or use custom scoring for the combined case.

2. **Test coverage for edge cases**: The engineer review mentions "workflow body instruction quality" but does not specify exact metrics (e.g., specificity score, branching logic depth). Synthesis stage should define rubric criteria for "good instruction body" independent of compilation success.

3. **MCP server availability in test environment**: Eval rows that test MCP configuration success (e.g., "add a Notion MCP server") may require a running MCP server in the test environment. Synthesis stage should clarify whether evals test configuration *writing* (deterministic) vs. MCP server *connection* (requires live server).

4. **Secrets and authentication in evals**: Some failure patterns involve missing secrets (e.g., `${{ secrets.API_KEY }}`). Evals should test that the skill detects missing-secret scenarios without requiring actual secrets in the test runner.

### Recommendation

**Proceed with synthesis.** All critical sources are available, well-maintained, and accessible. No external dependencies block eval authoring. The engineer review provides clear failure modes and dataset gaps that the synthesis stage can translate into specific eval row requirements. Start with the rubric focus areas above and author at least one eval row per focus area (7 minimum), plus additional coverage for MCP transports, safe-output types, and debugging scenarios.

---

## Saved Artifact Path

Research brief saved to:  
`skills/create-workflow/.trainer-workspace/SKILL/iterations/iteration-1/research/research-brief.md`

This brief is ready for the synthesis stage. Next steps:
1. Load synthesis task and review this brief for dataset row templates.
2. Author eval rows following the mapping notes above.
3. Create `skills/create-workflow/evals/evals.json` with synthesized rows.
4. Verify all rows follow the approved evals.json structure (id, prompt, expected_output, assertions, scoring, criteria).
5. Include at least one eval per rubric focus area and one per engineer-review failure mode.
