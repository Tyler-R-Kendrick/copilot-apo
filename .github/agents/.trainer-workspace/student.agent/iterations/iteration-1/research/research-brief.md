# Research Brief: Student Agent Optimization

## Target Summary

**Target file:** `.github/agents/student.agent.md`
**Role:** Teacher-guided candidate revision specialist inside trainer-led optimization loops.
**Optimization goal:** Improve revision discipline, evidence reading order, teacher-approval prediction accuracy, and loop-exit clarity.

## Research Plan and Approval Bar

The student agent is a repository-internal orchestration contract. No public dataset directly covers teacher-student loop behavior in AI prompt-optimization workflows. Research focuses on:

1. Established practices for iterative feedback-loop agents in LLM systems
2. Benchmark tasks for reasoning-trajectory exposure and critique-response fidelity
3. Schema guidance for teacher-student evaluation: input includes teacher critique, workspace context, and current candidate; output is a revised candidate plus reasoning trajectory

**Source approval bar:** accountable maintainer, traceable origin, and explicit or permissive license. Public benchmarks are used as schema inspiration only; actual training rows are synthesized from the agent contract requirements.

## Approved Sources (Schema Inspiration)

| Source | Role | Notes |
|--------|------|-------|
| Repository `skills/trainer-train/references/collaboration-contract.md` | Role boundary reference | Defines student-agent responsibilities in the trainer loop |
| Repository `.github/agents/teacher.agent.md` | Teacher handoff contract | Defines what the student should expect from teacher guidance |
| Repository `.github/agents/student.agent.md` | Target contract | Canonical role definition and constraints |
| Repository `.github/agents/.trainer-workspace/adversary.agent/iterations/iteration-1/synthesize/datasets/train.jsonl` | Format reference | Example `llm_judge` rows with `input`, `reference`, `criteria`, `scoring` |

## Rejected Sources

- Generic RLHF preference datasets: scope mismatch; those evaluate chat quality, not revision discipline in structured optimization loops.
- Open LLM benchmark suites (MMLU, BIG-Bench): no coverage of agentic revision or teacher-approval prediction tasks.

## Mapping Notes for Eval Authoring

- **Input field**: teacher critique text + current candidate excerpt + available workspace context note
- **Reference field**: description of the ideal revised candidate behavior, reasoning trajectory, and teacher-approval prediction
- **Criteria field**: observable, checkable assertions about revision discipline, scope, reasoning quality, and loop behavior
- **Scoring**: `llm_judge` for all rows (open-ended reasoning quality cannot be checked deterministically)

## Unresolved Gaps

None blocking synthesis. Rows will be synthesized from the contract requirements identified in `engineer-prompt/review.md`.

## Recommendation

Proceed to synthesis using the agent contract as the primary source and the adversary agent dataset as the format model.
