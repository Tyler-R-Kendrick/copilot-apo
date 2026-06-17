# AGENTS.md

## Agent File Structure

Agent files define autonomous or semi-autonomous task handlers with a clear role, scope, constraints, and operational contract. A complete agent file includes:

**Frontmatter (Lean & Discoverable):**
- `name`: Short identifier for the agent
- `description`: 1-2 sentence purpose statement
- `tools`: List of tool capabilities (e.g., git, bash, grep, view, edit)
- `hints`: Optional tips for common use patterns

**Body (Comprehensive & Operational):**
- **Role**: Clear statement of what the agent does and who it serves
- **Scope**: What the agent owns (e.g., code reviews, prompt synthesis, research) and what it explicitly avoids
- **Constraints**: Hard rules and off-limit actions (security, scope boundaries, no unrelated changes)
- **Evidence Ordering**: How the agent should prioritize information sources (read evidence in this order)
- **Operational Instructions**: Step-by-step workflow for the agent's primary tasks
- **Tool Preferences**: When to prefer one tool over another, and why
- **Output Format**: Structure and tone of agent responses (e.g., concise summaries, no temp files, reasoning first)

Frontmatter fields are *discoverable* and *lean*; body sections are *comprehensive* and *task-specific*. This split allows callers to quickly understand the agent's role and constraints while the agent itself has rich operational guidance.

## Agent Roles & Responsibilities

This repository defines 8 core agent types with strict handoff discipline:

1. **Trainer**: Orchestrates prompt optimization loops. Owns workspace state, blocker handling, stage sequencing, steering artifacts, and write-back decisions. Delegates research, synthesis, optimization, and selection to other agents; retains decision authority.

2. **Teacher**: Interprets optimization artifacts, steering context, or user input into actionable critique for the next iteration. Never orchestrates; always returns guidance, evidence, and recommendations for the trainer or student to act on.

3. **Judge**: Scores candidate quality, compares multiple results, and produces concise correctness summaries. Owns evaluation logic and validation artifacts. Never modifies prompts or datasets directly.

4. **Student**: Drafts or revises candidate prompts, datasets, or evaluators based on teacher guidance, optimization reports, or trainer direction. Never owns final decisions; returns reasoning and revised artifacts for trainer approval.

5. **Engineer**: Specializes in LLM/ML system metrics—latency, accuracy, cost, throughput, token efficiency, determinism, reliability. Optimizes prompt and context systems, formats reasoning for teacher review, and bridges prompt quality and engineering constraints.

6. **Researcher**: Identifies public sources, benchmarks, datasets, schemas, and supporting material before synthesis or optimization. Produces source shortlists, research briefs, and approval notes. Never guesses missing data; always grounds discovery in public sources and schema notes.

7. **Adversary**: Stress-tests pending changes—prompts, datasets, evaluators, scoring logic—before finalization. Generates exploit artifacts and failure modes. Review-only; does not modify code.

8. **Conservator**: Reviews prompt, dataset, or evaluator changes for likely regressions after optimization or customization. Identifies edge cases and backward-compatibility risks. Review-only; does not modify code.

Clear separation: **Orchestrators** (Trainer) sequence work. **Agents** (Student, Researcher, Engineer, Judge) execute specialized tasks and return artifacts. **Reviewers** (Adversary, Conservator) audit before changes go live. **Teachers** (Teacher) provide critique and coaching, never decisions.

## MCP Agent-Skills Integration Pattern

The trainer orchestration workflow integrates with agent-skills through a strict 3-step pattern:

1. **find_agent_skill**: Discover the exact `trainer-train` skill before orchestration begins, then discover stage-specific `trainer-*` skills before each stage that the loop actually needs. Establish that the skill exists and is available.

2. **load_agent_skill**: Load `trainer-train` first—this establishes the orchestration contract that governs workspace state, blocker handling, judge-mode inference, manual-followup recovery, steering, validation, and write-back decisions for the entire run. Load stage-specific skills before first use and again if the workflow context changes enough that the skill contract should be refreshed.

3. **run_agent_skill**: Execute only the discovered stage-specific `trainer-*` skills that expose a runnable runtime (e.g., `trainer-synthesize`, `trainer-optimize`), passing resolved inputs, datasets, and artifacts for that stage. Treat `trainer-train` as contract-only—it does not itself run as an execution stage.

**Pattern Guarantee**: find → load → run. Never fallback to manual equivalents or skip the discovery step. If the agent-skills MCP server is unavailable, the trainer stops with an explicit blocker and instructs the caller to install the skills before continuing.

## Handoff Design & Discipline

Handoffs between agents follow a strict data and authority contract:

**Caller Responsibility:**
- Provide complete context: target artifact, current workspace state, datasets in play, success criteria, and any steering history from prior turns
- Retain final decision authority; the caller always decides whether to act on returned guidance
- Supply all required evidence upfront (source material for synthesis, datasets for optimization, artifacts for review)

**Agent Responsibility:**
- Execute the specified task and return evidence, guidance, revised artifacts, or recommendations—never finished work pretending to be the caller's decision
- Respect scope boundaries and never expand into unrelated changes
- Surface blockers explicitly and stop rather than guess missing data or permissions
- Return reasoning alongside any artifact so the caller can understand and verify the decision

**Evidence Flow:**
Target returns *evidence* and *guidance*, not *finished work*. The trainer or caller inspects the returned evidence, makes the decision, and applies it. Teachers return critique, not orchestration commands. Students return revised candidates, not write-back approval. This separation keeps authority clear and audit trails intact.

## Frontmatter vs Body

**Frontmatter** (YAML or similar) is *discoverable metadata* for fast agent lookup and type-checking:
- `name`, `description`, `tools`, `hints`
- Scannable by callers without reading full body
- Never contains operational details or full scope statements
- Used by orchestrators to route tasks and understand high-level capability

**Body** (Markdown sections) is *comprehensive operational guidance* for the agent's execution:
- `Role`, `Scope`, `Constraints`, `Evidence Ordering`, `Operational Instructions`, `Tool Preferences`, `Output Format`
- Read in full by the agent before executing a task
- Contains detailed workflow, edge cases, tool recommendations, and validation steps
- Used by agents and trainers during active work, not for discovery

This split balances *discoverability* (frontmatter quick lookup) and *operational completeness* (body comprehensive guidance). Do not leak operational details into frontmatter and do not oversimplify the body into bullet points when workflows are complex.

## Training Workspace State Tracking

The trainer manages workspace state and resumable loops through `workflow-status.json` and structured iteration directories.

**Workspace Root**: `<target-dir>/.trainer-workspace/<prompt-name>/`
- Derived from target filename without final extension (e.g., `foo.prompt.md` → `.trainer-workspace/foo.prompt/`)
- Keeps all run artifacts under one local tree, never under repo-root sibling workspaces

**workflow-status.json Fields:**
- `workflow_state`: Current state (`pending_engineer_prompt`, `pending_training`, `training`, `complete`)
- `latest_iteration_dir`: Path to the active or most recent iteration (e.g., `iterations/iteration-1`)
- `required_artifacts`: Paths to datasets (`train_dataset`, `val_dataset`) and steering output directories (`latest_steering_turn`, `steering_summary_dir`)
- `decision_artifact`: Path to the final optimization artifact (`optimized-prompt.md` or `optimize-report.json`)

**Iteration Layout** (`iterations/iteration-N/`):
- `research/`: Public-source shortlist, research brief JSON/markdown, schema guidance, source approval notes
- `synthesize/`: Authored `evals/evals.json`, supporting `evals/files/`, and explicit `train.jsonl` + `val.jsonl` datasets
- `optimize/`: Optimized prompt candidate (`optimized-prompt.md`), `optimize-report.json` (or `manual-followup-report.json`), optional `trace-train-report.json`
- `election/`: Election summary JSON only when external leader selection is needed
- `validation/`: `pytest.txt`, eval command logs, or other deterministic validation output
- `steering/<agent>/turn-N/STEERING.md`: One file per agent turn, including evidence used, predicted response, requested revision, stop-or-continue decision, and judge notes
- `steering/<agent>/summary.md`: Rolling summary for that agent within the active iteration

**Update Helper**: Use `python .github/hooks/trainer-workspace.py update --repo-root <repo-root> --workspace-root <workspace> --state <state> --iteration iteration-N` to maintain workflow-status.json. Do not hand-edit this file; the helper ensures consistency and tracks cross-iteration state.

**Resumption**: If `workflow-status.json` shows `workflow_state: "training"`, treat the run as interrupted. Read `latest_iteration_dir`, audit which stages already produced artifacts (research brief, train/val datasets, optimized-prompt.md, validation output), then skip completed stages and continue from the first incomplete stage. Do not create a new iteration directory.

## git usage

Always use `git mv` to rename/move files.

## agent-skills

If you make agent-skills, put them in the root skill dir (~/skills). symlink them into the "~/.agents/skills" dir.
Always make them using Anthropic's "skill-creator" skill (npx skills add https://github.com/anthropics/skills --skill skill-creator)

## Code

Always use TDD with code coverage metrics to ensure 100% coverage.
Use Playwright to visually validate your work in the browser afterwards.
Take screenshots of the outcomes and put them into your PR description so we can view the outcomes that you believe are successful.

## Scaffolding

Use project specific cli tools to scaffold instead of manually creating/editing files (dotnet, uv, npm, etc.)
