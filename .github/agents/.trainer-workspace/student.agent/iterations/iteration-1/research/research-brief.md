# Research Brief: student.agent.md

## Target Layout

- Target file: `.github/agents/student.agent.md`
- Workspace: `.github/agents/.trainer-workspace/student.agent/`
- Eval manifest: `iterations/iteration-1/synthesize/evals/evals.json`
- Train dataset: `iterations/iteration-1/synthesize/datasets/train.jsonl`
- Val dataset: `iterations/iteration-1/synthesize/datasets/val.jsonl`

## Research Summary

This target is an internal agent contract. No external public dataset is required. Eval cases are derived from the agent's specification, the engineering review failure modes, and behavioral analysis of the student role in the trainer loop.

## Primary Sources (Internal)

1. **`student.agent.md` specification** — the source of truth for the agent's role, constraints, approach, and output format. Traceable origin: this repository. License: MIT (repo root). Maintainer: Tyler Kendrick.
2. **`engineer-prompt/review.md`** — engineering analysis identifying six failure modes: missing evidence reading order, vague approval prediction, ambiguous engineer handoff trigger, no hard turn cap, underspecified validation step, and missing guidance for absent steering artifacts.
3. **`trainer-train-agent/datasets/train.jsonl`** — reference format for agent-behavior eval rows using `llm_judge` scoring with `reference` and `criteria` fields. Shows the expected dataset shape.
4. **Researcher.agent workspace** — as a completed training example showing the full artifact layout, decision reasoning, and rewrite approach for a similar agent target.

## Rejected External Sources

No external sources were required. The student agent's behavior is entirely defined by the internal contract and its interaction with the trainer loop. Public datasets on "teacher-student learning" or "prompt revision" are not grounded enough to produce reliable agent behavior eval cases for this specific contract.

## Mapping Notes

Eval cases are derived from the agent's specified behaviors and the engineering review failure modes:

| Source material | Eval dimension | Row shape |
|----------------|---------------|-----------|
| Approach step 1 | Evidence reading order | Input asks student to revise without clear STEERING.md; response should request teacher turn |
| Constraint: no loop orchestration | Scope guard | Input asks student to orchestrate; response should refuse |
| Approval prediction step 6 | Prediction checklist | Input provides a revision; student should predict teacher approval using concrete signals |
| Engineer handoff step 4 | Handoff trigger | Input provides a reasoning trajectory needing structure; student should invoke engineer |
| Validation step 7 | Validation compliance | Input expects student to report validation result |
| Missing steering scenario | Blocking behavior | Input lacks STEERING.md; student should hand off to teacher rather than guess |

## Unresolved Gaps

None. The agent's behavior is fully specified in the contract. Eval cases cover the six failure modes identified in the engineering review.

## Judge Mode

`llm_judge` — agent behavior quality is open-ended. All eval rows use `reference` and `criteria` fields. No exact-match rows are needed.
