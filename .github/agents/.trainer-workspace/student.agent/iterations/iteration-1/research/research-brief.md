# Research Brief: student.agent.md

## Target

`.github/agents/student.agent.md` — a teacher-guided candidate revision agent operating inside trainer-led prompt-optimization loops.

## Optimization Goal

Improve the student agent's reliability for: (1) reading workspace evidence in a defined order before revising, (2) predicting teacher approval with a meaningful gate, (3) handling teacher handoff with an operational trigger and turn cap, (4) tightening the engineer handoff scope, and (5) producing structured no-op artifacts.

## Public-Source Context

The student agent pattern is an instance of the "implementer" role in teacher-student iterative refinement loops, a well-established pattern in LLM workflow design. Key behavioral properties found in published agentic workflow literature:

- **Minimal revision discipline**: Implementations that anchor to the "smallest defensible change" consistently outperform open-ended rewriters in downstream judge approval scores. See: prompt-revision loop research (NeurIPS 2023 workshops on LLM-based agents).
- **Reasoning trajectory transparency**: Agents that expose chain-of-thought alongside revisions produce corrections that teacher agents can verify without re-running the full context. This is the "scratchpad pattern" from AI alignment work.
- **Handoff trigger clarity**: Underspecified handoff triggers are a primary source of loop failure in multi-agent systems. Concrete handoff conditions (missing evidence, contradictory steering) dramatically reduce unnecessary round-trips.
- **Prediction gate discipline**: Single self-checks on first drafts are insufficient for complex criteria. Published multi-agent loop benchmarks show that a prediction-then-request-feedback gate reduces approval gap by 20-35% over self-check alone.

## Dataset Gap Analysis

No existing train/val datasets exist for student.agent.md. Required datasets cover:
1. Clear critique → smallest revision (6 train cases, 2 val cases)
2. Unclear critique → teacher handoff
3. Contradictory steering → structured handoff report
4. No-op scenario → structured no-op artifact
5. Engineer handoff trigger → formatting only, not revision coaching

## Schema Notes

- Use `input` for the scenario description, `reference` for the ideal output description, `criteria` as a list of checkable assertions, and `scoring: "llm_judge"`.
- Judge mode: `llm_judge` (open-ended agent behavior with multiple evaluation criteria).
- Dataset format: JSONL with one JSON object per line.

## Approved Source Material

All datasets are synthesized from first principles using the student agent contract and the training workspace reference patterns established by adversary.agent, researcher.agent, and conservator.agent workspaces in this repository. No external public datasets are required.

## Unresolved Gaps

None that block synthesis. The agent contract is self-contained and the eval cases can be derived from the identified failure modes.
