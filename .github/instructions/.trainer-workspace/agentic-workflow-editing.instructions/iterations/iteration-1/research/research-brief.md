Agent completed. agent_id: researcher, agent_type: researcher, status: completed, description: Research agentic workflow editing, elapsed: 607s, total_turns: 0, duration: 601s

I now have comprehensive evidence from all accessible sources. Let me compile the complete research brief and dataset schema recommendation.

---

## Research Brief: `agentic-workflow-editing.instructions.md` — Instruction Design and Dataset Sources

> **MCP execution log**: `find_agent_skill("researcher-research")` returned rank-1 score 21 via `agent_skills_mcp.find_agent_skill()` (Python API on `tools/agent-skills-mcp/`). `load_agent_skill("researcher-research")` returned full SKILL.md contract. `run_agent_skill` scaffold executed via `skills/researcher-research/scripts/run_research.py` — deterministic path confirmed. All three MCP contract steps satisfied.

---

### Target and Task Summary

**Target file**: `.github/instructions/agentic-workflow-editing.instructions.md`  
**Current state**: Iteration-1 "student" candidate applied — 5 flat bullet points with `applyTo: ".github/workflows/*.md"` frontmatter.  
**Task**: Guide AI agents to: (1) run `gh aw compile <workflow-name>` after every edit to a `.github/workflows/*.md` file, (2) include the regenerated `.lock.yml` in the same commit, (3) verify both files are in `git diff HEAD --name-only` before committing, (4) run a final compile before opening a PR, and (5) stop and fix the source on compile failure — never leaving `.md` and `.lock.yml` in drift.  
**Scoring rule**: `llm_judge` — behavioral compliance check. Correct responses must demonstrate: unconditional compile trigger (no "meaningful change" exceptions), correct workflow name derivation (strip `.md` extension), `.lock.yml` inclusion in changeset, stop-on-failure behavior, and non-reliance on the hook as primary mechanism.  
**Domain**: GitHub Copilot agent instructions, agentic workflow compilation, lockfile management, CI/CD artifact sync.  
**Licensing constraints**: Not specified by caller. All in-repo artifacts are project-internal. GitHub Docs are freely referenceable documentation. No commercial-use or privacy restrictions identified.  
**Recency**: No version floor specified. In-repo artifacts are dated May 2025. GitHub Docs reflect current product (accessed May 2025).

---

### Research Plan and Approval Bar

**Derived eval layout** (from `run_research.py` scaffold):
```
manifest_file:    .github/instructions/evals/evals.json
files_dir:        .github/instructions/evals/files/
workspace_dir:    .github/instructions/.trainer-workspace/agentic-workflow-editing.instructions/
benchmark_file:   .github/instructions/.trainer-workspace/agentic-workflow-editing.instructions/benchmark.json
```
*Note*: The existing `evals/evals.json` contains `prompt-optimization.instructions` evals, not `agentic-workflow-editing.instructions` evals. The iteration-1 synthesized evals are under the `.trainer-workspace` path. The manifest file needs to be populated with agentic-workflow-editing eval cases.

**Placeholders**: None — this is a static instruction file, not a prompt template. The optimizer uses an implicit `Task Context:` block during rollouts (confirmed in `docs/getting-started.md`).

**Primary-source-first search plan**:
1. In-repo artifacts first: current instructions file, iteration-1 training artifacts, hook implementation, engineer-prompt review — all directly define the evaluation target and ground truth.
2. GitHub Docs for `.instructions.md` format schema, `applyTo` frontmatter, and build/compile instruction patterns.
3. GitHub Docs for coding agent best practices to validate the "primary mechanism vs. backstop" hook framing.
4. Hook implementation code to confirm the PostToolUse + Stop enforcement contract.

**Approval bar applied** (from skill contract):
- ✅ Accountable maintainer (GitHub Inc., project maintainer)
- ✅ Traceable data origin, schema, label definitions
- ✅ Owner-provided evaluation rules
- ✅ Explicit license or reuse terms (GitHub Docs ToS; project-internal sources are owner-controlled)
- ✅ Stable version/date identifier (May 2025; git-tracked)
- ✅ Acceptable contamination/leakage risk (behavioral compliance, no PII, no benchmark contamination)

**Missing inputs**: Scoring rule inferred from existing `train.jsonl` rows — all use `"scoring": "llm_judge"`. No formal scoring rubric document separate from the training data exists, but each row's `criteria` field serves this role. Noted but not a blocker: the criteria patterns are consistent across all 10 rows.

---

### Approved Sources

**Rank 1 — In-repo iteration-1 training artifacts** *(primary source, highest fit)*

| Field | Value |
|---|---|
| Maintainer | Project maintainer (this repository) |
| Data origin | Synthesized from the 4 instruction rules by the trainer loop |
| Label provenance | Each row has explicit `criteria` and `expected` fields authored during iteration-1 optimization |
| License | Project-internal |
| Version | Iteration-1, May 2025 |
| Task fit | Direct — tests the exact compliance behaviors the instructions are designed to produce |
| Contamination risk | Low — behavioral scenarios, not copied from public benchmarks |

Files:
- `train.jsonl`: 6 rows — covers: basic edit sequence, hook reliance trap, compile failure, trivial edit (whitespace), stale after multi-edit, hook flag indicating stale lockfile
- `val.jsonl`: 4 rows — covers: full workflow sequence, hook scope, comment-only edit, sync verification
- `synthesize/evals/evals.json`: 5 eval cases — covers: basic sequence, compile failure, wrong name derivation, skip-rule, multi-workflow
- `decision.md`, `teacher/summary.md`, `adversary/summary.md`: Documents tested improvements and adversarial gap analysis

**Rank 2 — Hook implementation** (`validate-agentic-workflow-compilation.sh` + `agentic-workflow-validation.json`) *(primary source)*

| Field | Value |
|---|---|
| Maintainer | Project maintainer (this repository) |
| Data origin | Bash + Python implementation of the enforcement contract |
| Label provenance | The hook script defines exactly what "stale lockfile," "missing lockfile," and "compile failure" mean |
| License | Project-internal |
| Version | Current, git-tracked |
| Task fit | Direct — defines the enforcement semantics the instructions reference |
| Contamination risk | None |

Key confirmed facts from hook code:
- **PostToolUse** (matcher: `Write|Edit|MultiEdit|ApplyPatch`): Non-blocking reminder, records changed files
- **Stop** (`--enforce`): Actually runs `gh aw compile <workflow-name>`, blocks session if compile fails or lockfile wasn't updated
- `workflow_name = basename(workflow_path, '.md')` — confirms name derivation rule exactly
- Hook checks both `git diff --quiet` AND `git diff --cached --quiet` — confirms staged-file coverage requirement that motivated the `git diff HEAD --name-only` fix

**Rank 3 — GitHub Docs: "Adding repository custom instructions for GitHub Copilot"** *(public primary source)*

| Field | Value |
|---|---|
| Maintainer | GitHub, Inc. |
| URL | https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot |
| HTTP status | 200 (following 301 redirect) |
