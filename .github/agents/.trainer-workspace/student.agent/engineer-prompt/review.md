# Engineer Prompt Review: student.agent.md

**Target:** `.github/agents/student.agent.md`
**Workspace:** `.github/agents/.trainer-workspace/student.agent/`
**Review date:** 2026-05-14

## Target goal

The `student` agent implements teacher-guided candidate revision inside a trainer-led optimization loop. Its primary responsibility is: absorb teacher critique → draft the smallest defensible revision → expose the reasoning trajectory → predict teacher approval. It must not orchestrate the loop or invoke engineer skills directly.

## Tool and MCP routing audit

**Tool list:** `[read, edit, search, execute, todo, agent, agent/runSubagent]`
**Named agents:** `["teacher", "engineer"]`
**Handoffs:** `Request Teacher Guidance` (→ teacher), `Request Engineer Guidance` (→ engineer)

### Findings

| Finding | Concern | Severity |
|---------|---------|----------|
| `agent/runSubagent` listed alongside explicit named `handoffs:` | Tool routing / bloat | Medium — redundant; named handoffs already bound delegation |
| `execute` tool listed with no scoping guidance | Tool routing | Medium — constraint says "do not use engineer skills directly" but execute could invoke them |
| Teacher handoff trigger includes "stale critique" as a condition alongside "unclear target" | Handoff behavior | Low-medium — overly broad trigger; student may delegate before attempting a draft |
| Constraint "Do not take over judging, adversarial review, or trainer-loop orchestration" | Prompt bloat | Low — implied by role description, not actionable routing guidance |
| Engineer handoff trigger: "task needs prompt-engineering or Trace-oriented expertise" | Tool routing | Low-medium — vague; does not distinguish student's own explanations from ones needing reformatting |

## Likely failure modes

1. **Premature teacher delegation**: The combined trigger conditions (incomplete, contradictory, stale, unclear) make it easy for the student to hand off immediately rather than attempting a draft first. This wastes a loop turn and shifts workload back to the teacher unnecessarily.
2. **`execute` used to invoke skills directly**: Without scope guidance, `execute` invocations could run `engineer-prompt` or `engineer-code` scripts, violating the "no direct skill invocation" constraint.
3. **Unfocused engineer handoff**: The current trigger is too vague — students may use the engineer handoff for stylistic clean-up rather than genuine structural coaching needs.
4. **Loop exit ambiguity**: Step 6 of the approach says "do at most one extra self-check only if the draft still looks unsupported" — this is correct but the self-check criteria are implicit, not explicit. Students may loop more than once or exit prematurely.
5. **Output verbosity without signal**: The output format requires reasoning trajectory but does not specify the minimum signal the teacher needs (e.g., which constraints were considered and rejected).

## Dataset gaps

- No train/val datasets exist for this agent. Must synthesize from the agent's three core tasks: (1) drafting a revision from clear critique, (2) requesting teacher guidance when revision target is unclear, (3) using the engineer handoff only for structural reformatting.
- judge_mode: `llm_judge` (open-ended agent behavior quality)
- Suggested eval dimensions: reasoning trajectory completeness, correct handoff trigger decision, smallest-defensible-revision adherence, teacher approval prediction accuracy

## Validation plan

- `python -m pytest -q` from repo root (currently 856 tests expected to pass)
- Regression check: agent file is YAML-frontmatter-valid after edit
- Spot check: handoffs still bound to named real agents (teacher, engineer only)

## Optimization hypothesis

**Primary**: Tighten tool routing — remove `agent/runSubagent`, add scope note for `execute`. This reduces ambiguity about which delegation path to use.

**Secondary**: Sharpen handoff triggers — teacher handoff only when revision target is undefined or critique actively contradicts workspace evidence; engineer handoff only when explanation structure needs prompt-engineering framing and plain language is insufficient.

**Tertiary**: Remove redundant prohibition constraints; replace with one positive routing rule per concern.

**Expected gain**: Fewer premature teacher delegations per loop, clearer execute scope, less decision ambiguity for the student agent.
