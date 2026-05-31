# Research Brief: student.agent.md Optimization

## Target and Task Summary

**Target file:** `.github/agents/student.agent.md`
**Workspace:** `.github/agents/.trainer-workspace/student.agent/`
**Task:** Improve the student agent's revision discipline, reasoning transparency, engineer handoff precision, and loop-exit hygiene.

The student agent operates inside teacher/student trainer loops. It accepts teacher critique, produces revised prompt candidates, and exposes explicit reasoning trajectories. The main failure modes identified in the engineering review are: missing evidence reading order, ambiguous engineer handoff trigger, underspecified validation step, missing loop-exit criteria, no fallback for absent steering artifacts, and inconsistent no-op output format.

## Research Plan and Approval Bar

**Task boundary:** Prompt-optimization agent behavior — specifically, multi-turn LLM agent collaboration protocols for prompt critique and revision.

**Primary source types:**
- Academic benchmarks for multi-turn reasoning and instruction-following (e.g., MT-Bench, Alpaca Eval, IFEval)
- Studies on chain-of-thought, tree-of-thought, and explicit reasoning trajectory formats
- Collaborative agent protocol papers (teacher-student, RLHF critique-revision frameworks)

**Approval bar (all criteria must pass):**
1. Named, accountable maintainer, publisher, or standards body
2. Traceable data origin, schema, and label definitions
3. Explicit license or reuse terms
4. Stable version, date, or release identifier
5. Acceptable contamination, leakage, privacy, and bias risk for authored eval use

## Approved Sources

No external public dataset is required for this optimization target. The student agent's eval cases are synthetic behavioral scenarios drawn from the repository's own agent collaboration contracts. The eval rows test the agent's compliance with its own contract (reading order, engineer handoff, validation, loop-exit), not its factual recall or open-domain reasoning. Synthetic eval construction is appropriate here.

**Rationale:** Importing a public benchmark (e.g., MT-Bench) would introduce an unrelated scoring distribution. The target agent is evaluated on protocol compliance within this repository's trainer loop, which requires purpose-built synthetic cases.

## Rejected Candidates

| Source | Rejection Reason |
|--------|-----------------|
| MT-Bench (Zheng et al. 2023) | Multi-turn conversation benchmark; measures response quality, not agent protocol compliance |
| Alpaca Eval (Li et al. 2023) | Single-turn instruction following; incompatible with multi-agent loop format |
| IFEval (Zhou et al. 2023) | Evaluates instruction-following constraints in single messages; not multi-agent or loop-oriented |
| RLHF critique-revision datasets | Most are proprietary (InstructGPT, Anthropic Constitutional AI); not safely reusable for authored evals |

## Mapping Notes

Since we are using synthetic eval construction, the mapping is direct:
- `prompt` fields describe a realistic student invocation scenario (type of teacher critique received, workspace state)
- `expected_output` fields describe what a correctly behaving student agent should produce
- `assertions` fields describe observable, objective compliance checks
- `scoring` is `llm_judge` with `criteria` specifying behavioral compliance

**Eval row construction approach:**
Each row presents a concrete student scenario:
1. A teacher critique with a specific revision target
2. A workspace state (steering artifacts present, missing, or conflicting)
3. Expected student behavior: read evidence in order, apply smallest revision, expose reasoning, predict teacher approval

## Unresolved Gaps

None. Synthetic construction is sufficient. The main gap in the source agent contract itself (the gaps identified in the engineering review) informs the eval design: test cases should surface missing evidence reading order, ambiguous engineer handoffs, and underspecified loop exits.
