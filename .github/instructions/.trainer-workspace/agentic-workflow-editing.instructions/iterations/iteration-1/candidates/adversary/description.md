# Adversary Analysis — Iteration 1

## Exploits Found (3 total)

### Exploit 1 (STRONGEST): Comment-Only Edit Creates Impossible Verification Loop

When a comment-only edit produces a stable compile (byte-for-byte identical lockfile), `git diff HEAD --name-only` will NOT show the `.lock.yml`. But the original student candidate says "confirm both the source .md and the .lock.yml appear" — making verification impossible for this valid scenario. Agent would loop, scope-creep, or have false failure belief.

**Status**: FIXED — Added qualifier "If the .lock.yml is byte-for-byte unchanged after compile (stable-compile edit), stage only the .md."

### Exploit 2: Final Pre-PR Compile Drops Workflow Name

Bullet 3 said "Run `gh aw compile` one final time" without a `<workflow-name>` argument — the compile command would fail or behave unexpectedly without the name.

**Status**: FIXED — Changed to "Run `gh aw compile <workflow-name>` one final time before opening a pull request."

### Exploit 3: "Both" Verification Is Singular for Multi-Workflow Scenarios

Bullet 3's "confirm both the source .md and the .lock.yml appear" is singular language that misleads in multi-workflow contexts (6 files would appear, not 2).

**Status**: FIXED — Changed to "confirm each edited workflow's .md and .lock.yml pair both appear."

## Summary

The adversary found no exploit that beats the final candidate after fixes. All three exploits were addressed with minimal, surgical text changes.
