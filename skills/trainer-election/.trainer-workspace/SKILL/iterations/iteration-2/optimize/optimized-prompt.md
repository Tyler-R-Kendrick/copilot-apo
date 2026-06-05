---
name: trainer-election
description: Elect the strongest prompt or skill candidate from an existing evaluation workspace. Use this skill whenever a workflow already has multiple scored configurations and needs a separate leader-selection pass over grading, timing, or benchmark artifacts, especially when comparing optimizer outputs without pushing that selection logic back into the optimization runtime.
license: MIT
compatibility: Requires Python 3.11+. Reads eval workspaces that follow the Agent Skills evaluation layout with eval metadata, grading.json, timing.json, and optional benchmark.json artifacts.
metadata:
  author: your-org
  version: "0.1.0"

---

# Election

Use this skill to elect a winner from existing evaluation artifacts. Treat it as the standalone selection step after candidates have already been run and graded. It does not generate candidates, perform research, synthesize evals, or re-run optimization.

## When to use this skill

- A workflow already produced multiple candidate configurations and now needs a winner chosen from scored artifacts.
- Multiple candidate configurations already exist as `with_skill`, `without_skill`, `old_skill`, or other config directories inside a skill-eval workspace.
- Each candidate has already been run against authored evals and saved `grading.json` and optional `timing.json` artifacts.
- You need a separate election pass that picks the strongest configuration from workspace results instead of generating new candidates.
- The workflow explicitly needs comparison across multiple optimizer outputs, prompt rewrites, or skill revisions without folding that comparison into the optimization runtime.

Do not use this skill to gather datasets, synthesize evals, optimize prompts, or run missing evaluations from scratch. Those remain separate skills.

## Prerequisites: Readiness Check

Before running this skill, verify that your workspace meets these minimum preconditions:

1. **Scored artifacts must exist**: The skill requires at least one configuration directory with a `grading.json` file (raw run data) or `benchmark.json` (fallback only).
2. **Workspace entry points**: Provide one of:
   - A workspace root containing `iterations/iteration-N/` directories
   - A direct `iterations/iteration-N/` directory
   - A legacy workspace root containing `iteration-N/` directories
   - A legacy direct iteration directory
   - A direct eval directory when only one eval folder is available
3. **Configuration directories**: Configuration directories must follow the Agent Skills evaluation structure, containing `grading.json` and optional `timing.json` directly or nested under `run-N/` subdirectories.
4. **Eval manifest**: Either provide an explicit `manifest_file` (path to `evals/evals.json`) or ensure one exists at the standard location next to the iteration directory or one level higher.
5. **Clear error condition**: If no scored candidate runs can be found (no `grading.json` and no `benchmark.json`), the runtime stops with a clear error and does not invent results.

If your workspace does not yet meet these conditions, use other skills to run the missing evaluations first. Do not use `trainer-election` on incomplete or partially archived workspaces.

## Inputs

- `workspace_dir`: root workspace path, a specific iteration directory, or a direct eval directory
- `iteration`: optional iteration selector when the workspace contains multiple iterations
- `manifest_file`: optional authored `evals/evals.json` path for expected eval coverage

If `manifest_file` is omitted, the runtime searches `evals/evals.json` next to the iteration directory, then one level higher.

## Election Algorithm

The runtime executes the following workflow to discover, validate, and elect a winner:

### 1. Workspace Discovery

1. Read the requested iteration directory or identify the latest iteration in the workspace.
2. Accept these workspace shapes:
   - `iterations/iteration-N/` directories inside a workspace root
   - Direct `iterations/iteration-N/` directories
   - Legacy `iteration-N/` directories inside a workspace root
   - Legacy direct iteration directories
   - Direct eval directories when only one eval folder is available
3. Discover configuration directories, which may keep evals at the top level or under `runs/` subdirectories.

### 2. Coverage Resolution

4. Resolve expected eval coverage from:
   - The explicit `manifest_file` when provided, otherwise
   - The nearest `evals/evals.json` discovered next to the iteration directory or one level higher, otherwise
   - The discovered eval keys from available grading artifacts.

### 3. Scored Artifact Loading and Aggregation

5. Load scored runs from raw `grading.json` and `timing.json` artifacts (primary source).
6. Fall back to `benchmark.json` only when raw run artifacts are unavailable.
7. Aggregate the following metrics per configuration:
   - Mean pass rate across all evals
   - Coverage ratio (evals scored / expected evals)
   - Penalty applied to incomplete coverage
   - Mean time per eval
   - Mean token count per eval
   - Mean error count across evals
8. Penalize incomplete eval coverage so partially graded candidates do not beat fully validated ones by omission.

### 4. Baseline Identification and Pool Preservation

9. Identify baseline configurations by name:
   - Explicit names: `baseline`, `without_skill`, `old_skill`
   - Any name ending in `_baseline`
10. Keep all baseline configurations in the scoring pool so comparisons remain explainable and auditable.

### 5. Tie-Breaking and Election

11. Elect the leader by adjusted score using this ordering:
    - Higher adjusted score (pass rate adjusted for coverage penalty)
    - Lower error count
    - Lower mean time
    - Lower mean token usage
    - Stable name ordering (alphabetical) as final tie-breaker
12. Persist candidate metadata so callers can explain the winner and locate the winning prompt artifact.

## Output Contract

The runtime returns JSON with these top-level fields:

- `winner`: winning configuration name
- `best_prompt`: prompt text when a prompt artifact is discoverable
- `best_prompt_file`: path to the winning prompt artifact when present
- `best_candidate`: the winning candidate record
- `persisted_candidates`: all candidate summaries with `is_winner`, coverage, score, and cost metadata
- `selection_source`: `workspace` when raw run artifacts were used, otherwise `benchmark`
- `iteration_dir`, `manifest_file`, and `expected_eval_count` for caller traceability

Prompt artifacts are discovered from `outputs/` or the run directory using prompt-like filenames such as `prompt.md`, `candidate.md`, or `*.prompt.md`.

## Running the Runtime

```bash
python skills/trainer-election/scripts/run_election.py <workspace_dir> \
  [--iteration <iteration-number-or-path>] \
  [--manifest-file <path-to-evals.json>] \
  [--output-file <path-to-result.json>]
```

Use [skills/trainer-election/references/leader-election.md](./references/leader-election.md) for the short rationale behind the scoring model.

## Naming Rationale

`election` is still the right public name because the skill owns the act of electing a winner from a scored field of candidates. The key change is that the field now comes from evaluation artifacts rather than from internal optimize-side search.
