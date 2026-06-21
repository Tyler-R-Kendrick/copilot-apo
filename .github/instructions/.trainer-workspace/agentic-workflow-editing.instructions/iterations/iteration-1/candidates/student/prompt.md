---
description: "Use when editing agentic workflow markdown files under .github/workflows/*.md. Covers required compilation, lockfile sync, and hook-backed enforcement."
applyTo: ".github/workflows/*.md"
---
# Agentic Workflow Editing Guidance

## The Core Requirement

After **any** edit to a `.github/workflows/*.md` file, you **must** compile and include the regenerated `.lock.yml` in the same commit. The source file and lockfile must always stay in sync.

## Step-by-Step Workflow

Follow this sequence every time you edit a workflow file:

### 1. Edit the workflow source
Make your changes to the `.github/workflows/<workflow-name>.md` file. Changes include:
- Any line in the workflow definition itself
- Frontmatter (the YAML block at the top)
- Step instructions, imports, or metadata
- Even trivial changes like comments or whitespace

**Rule: When in doubt, recompile. The cost is low and drift is hard to detect later.**

### 2. Compile the lockfile
Run this command immediately after your edits:
```bash
gh aw compile <workflow-name>
```

Replace `<workflow-name>` with the name of your workflow file without the `.md` extension. For example:
- If you edited `train-prompt.md`, run: `gh aw compile train-prompt`
- If you edited `sync-skills.md`, run: `gh aw compile sync-skills`

If the compile command fails, **stop immediately**. Fix the error in your `.md` file, then recompile. Never leave your source file and lockfile out of sync.

### 3. Verify the lockfile changed
Check that both files appear in your change set:
```bash
git status
# or
git diff --name-only
```

You should see both:
- `.github/workflows/<workflow-name>.md` (the source you edited)
- `.github/workflows/<workflow-name>.lock.yml` (the regenerated lockfile)

If only the `.md` appears but the `.lock.yml` does not, something went wrong. Rerun `gh aw compile <workflow-name>` and check again.

### 4. Commit both files together
Include both the source and the lockfile in the same commit. Do not commit one without the other.

### 5. Before opening a pull request: Final verification
**This step is required every time.** Even if you compiled early in your editing session and then made more changes:

```bash
gh aw compile <workflow-name>
git diff --name-only
```

Confirm that no changes remain. If files appear in the diff, commit the updated lockfile, then re-verify. Do not open a pull request until `git diff` shows a clean state.

## The Validation Hook

The `agentic-workflow-validation` hook is the enforcement backstop. It checks:
- That each `.lock.yml` file is present in the repository
- That each `.lock.yml` matches the compiled output of its corresponding `.md` source file

**You should not rely on the hook as the only safety net.** Run `gh aw compile` yourself after every edit. The hook catches drift that was missed, but your job is to prevent drift from happening in the first place.

If the hook rejects your pull request with a "stale lockfile" message:
1. Run `gh aw compile <workflow-name>` locally
2. Verify `git status` shows the updated `.lock.yml`
3. Amend your commit to include the fresh lockfile
4. Rerun the validation hook

## Common Scenarios

### Scenario: "I made a tiny change, do I really need to compile?"
**Yes.** Compile after every edit, including comments, whitespace, or formatting changes. Consistency here prevents drift from accumulating undetected.

### Scenario: "I edited the file three times. Do I need to recompile each time?"
**Yes.** After each edit, run `gh aw compile`. It is safe to run multiple times. If you forget and make several edits before compiling, just compile once after all edits are done—the lockfile will reflect all changes.

### Scenario: "The hook rejected my PR but it says 'stale lockfile.' What now?"
Run `gh aw compile <workflow-name>` immediately. Check `git status` to confirm the lockfile was regenerated. Amend your commit, then push again. The hook will pass on the next run.

### Scenario: "gh aw compile returned an error"
**Do not proceed.** Stop and fix the error in your workflow source file. The error message will tell you what is wrong. Once fixed, rerun `gh aw compile <workflow-name>`. Repeat until the compile succeeds before moving on.

## Summary

| Step | Action | Why |
|------|--------|-----|
| 1 | Edit `.github/workflows/<name>.md` | Make your intended change |
| 2 | Run `gh aw compile <name>` | Regenerate the lockfile to match your edits |
| 3 | Check `git status` — both files present? | Verify the lockfile was actually regenerated |
| 4 | Commit both files together | Keep source and lockfile in sync in the repository |
| 5 | Before PR: Run `gh aw compile <name>` one more time, check `git diff` is clean | Prevent opening a PR with a stale lockfile |

Never rely on the hook as the primary mechanism. Proactive compilation prevents failures.
