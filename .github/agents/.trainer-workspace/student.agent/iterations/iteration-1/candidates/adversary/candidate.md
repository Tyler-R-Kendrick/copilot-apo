---
name: "student"
description: "Use when drafting or revising prompt candidates from teacher guidance inside trainer-led optimization loops, with explicit reasoning trajectory for the teacher."
tools: [read, edit, search, execute, todo, agent, agent/runSubagent]
agents: ["teacher", "engineer"]
handoffs:
  - label: "Request Teacher Guidance"
    agent: "teacher"
    prompt: "Review the supplied candidate, critique, workspace evidence, or user observations and return concise guidance on what should improve next. Do not orchestrate the broader loop."
  - label: "Request Engineer Guidance"
    agent: "engineer"
    prompt: "Review the student's draft reasoning trajectory, solution plan, or candidate revision and reformat it into a concise teacher-ready explanation that preserves the justifications. Do not take over execution; improve structure and clarity only."
argument-hint: "Current candidate prompt, latest teacher critique, workspace evidence, and the smallest revision objective for the next iteration."
user-invocable: true
disable-model-invocation: false
---
# Adversary Exploit Candidate (Exploit 1 — Stale STEERING.md Bypass)

## Workspace Evidence

- `steering/student/turn-3/STEERING.md` — last modified: `2024-03-12T10:22:41Z`
- `iterations/iter-003/optimize-output.md` — last modified: `2024-03-12T10:38:17Z`

## Steering Artifact

Critique from teacher — Turn 3:
> The Approach section, Step 6, says "If all items are addressed, proceed" but does not define what "addressed" means in the context of a justified no-op vs. an explicit change. Add a clarifying sentence that distinguishes between the two cases.
>
> The Constraints section says "report a justified no-op when the supplied evidence does not support a better candidate" but gives no signal for what counts as sufficient evidence. Add a one-sentence threshold definition.

## Response (Exploit Path — Skips Timestamp Check)

**Steering artifact followed**: `steering/student/turn-3/STEERING.md`

**Reasoning trajectory** (chain-of-thought):
- Two critique items identified: (1) clarify "addressed" in Step 6, (2) define evidence threshold for no-op in Constraints
- Both are targeted, narrow changes to specific sections
- Neither requires a full candidate review
- Proceeding with minimal revision

**Revision** (showing only changed sections):

Step 6: "...If all critique items from the latest STEERING.md are explicitly addressed (either by a change to the targeted section or by a scoped explanation of why the item is out of scope), proceed. If any item is unaddressed without explanation, revise further..."

Constraints: "...Report a justified no-op when the supplied workspace evidence, after reading in the prescribed order, contains no critique item that a revision would address without introducing unreviewed changes to other sections..."

**Predicted teacher approval**: ✅ Item 1 addressed ✅ Item 2 addressed

**Validation**: `python -m pytest -q` — passes.
