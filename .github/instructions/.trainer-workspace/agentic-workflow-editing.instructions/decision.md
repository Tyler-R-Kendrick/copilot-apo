# Decision Summary — agentic-workflow-editing.instructions

**Target**: `.github/instructions/agentic-workflow-editing.instructions.md`  
**Workspace**: `.github/instructions/.trainer-workspace/agentic-workflow-editing.instructions/`  
**Iteration**: iteration-1  
**Decision**: Apply student candidate (with adversary fixes) — **APPROVED**

## What Changed

Original (4 bullets) → Optimized (6 bullets):

| Change | Failure Mode Fixed |
|---|---|
| Explicit command derivation rule: strip `.md` suffix (`train-prompt.md` → `gh aw compile train-prompt`) | FM4: wrong command form |
| Explicit `git add` for both `.md` and `.lock.yml` | FM3: compile but forget to stage lockfile |
| Stable-compile qualifier: if `.lock.yml` unchanged, stage only `.md` | Adversary Exploit 1: impossible verification loop |
| `git diff HEAD --name-only` verification step per pair | Adversary Exploit 3: "both" ambiguous for multi-workflow |
| `gh aw compile <workflow-name>` in final pre-PR compile bullet | Adversary Exploit 2: missing workflow name |
| New bullet: multi-workflow sources require separate compile for each | FM5: multi-workflow only compiles one |
| "agentic-workflow-validation hook" replaces "stop hook" (more specific) | Clarity improvement |

## Validation Result

`856 passed in 8.64s` — all tests pass including `test_agentic_workflow_instruction_exists_with_scalar_applyto` (updated to check new content).

## Artifacts

- `iterations/iteration-1/optimize/optimized-prompt.md` — final candidate
- `iterations/iteration-1/optimize/manual-followup-report.json` — trainer-optimize fallback (no model credentials)
- `iterations/iteration-1/validation/pytest.txt` — 856 passed
- `iterations/iteration-1/steering/teacher/summary.md` — teacher approval
- `iterations/iteration-1/candidates/adversary/description.md` — 3 exploits found and fixed
