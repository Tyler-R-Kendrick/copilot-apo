# Research Brief: student.agent.md

## Target

`student.agent.md` — teacher-guided candidate revision agent inside trainer-led optimization loops.

## Public-Source Review

No public benchmark dataset exists specifically for "teacher-guided prompt candidate revision" behavior. The closest adjacent areas are:

1. **Iterative refinement / revision QA** — academic work on multi-turn revision (PEER, ArguEdit, SciPost revision datasets) covers revising text from feedback, but not agent-contract revisions within optimization workflows.
2. **Critique-and-revise datasets** — SELF-REFINE (Madaan et al., 2023) and CRITIC (Gou et al., 2023) cover self-critique but not teacher-mediated critique with explicit steering artifacts.
3. **Agent instruction following** — AgentBench and related eval suites cover instruction-following in tool-use settings, but not the specific handoff / approval-prediction loop of this agent.

## Conclusion

No grounded public primary source covers the `student.agent.md` task boundary (teacher critique → candidate revision → approval prediction → teacher handoff trigger). Synthesized eval cases from the agent contract are the appropriate dataset path.

## Schema Notes

All training rows should use `scoring: "llm_judge"` because:
- Agent behavior is open-ended (not exact-match)
- Quality criteria include reasoning clarity, handoff discipline, and approval prediction accuracy
- `reference` should describe the ideal behavior pattern; `criteria` should be checkable assertions

Row shape:
```json
{
  "input": "<scenario description>",
  "reference": "<description of correct behavior>",
  "criteria": ["<checkable assertion>", ...],
  "scoring": "llm_judge"
}
```

## Gap Report

No approved public source. Dataset must be synthesized from the agent contract, engineer-prompt review, and trainer-train-agent reference datasets (which cover adjacent orchestrator behavior for context).

## Stop Recommendation

Do not wait for a public source. Proceed with synthesis using the agent contract plus engineering review as the ground truth.
