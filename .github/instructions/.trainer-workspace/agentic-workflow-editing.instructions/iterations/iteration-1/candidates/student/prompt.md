---
description: "Use when editing agentic workflow markdown files under .github/workflows/*.md. Covers required compilation, lockfile sync, and hook-backed enforcement."
applyTo: ".github/workflows/*.md"
---
# Agentic Workflow Editing Guidance

- After editing any `.github/workflows/*.md` file, derive the compile target by stripping the `.md` suffix (`train-prompt.md` → `gh aw compile train-prompt`), run the command, then `git add` both the source `.md` and the regenerated `.lock.yml` before committing.
- Do not rely on the `agentic-workflow-validation` hook as the primary mechanism; treat `gh aw compile` as part of the edit itself. Recompile after every edit — including minor formatting or comment changes — because drift is hard to detect later.
- After compiling, run `git diff HEAD --name-only` to confirm both the source `.md` and the `.lock.yml` appear in the change set (covers staged and unstaged changes relative to HEAD). Run `gh aw compile` one final time before opening a pull request.
- When editing multiple workflow sources, compile each separately and `git add` all resulting `.lock.yml` files before committing.
- The `agentic-workflow-validation` hook is the enforcement backstop: it checks that the `.lock.yml` is present and matches the compiled output of the source file. If it reports stale or missing lockfiles, recompile and commit the updated `.lock.yml` before finishing.
- If `gh aw compile` fails, stop and fix the workflow source instead of leaving `.md` and `.lock.yml` drift in the branch.
