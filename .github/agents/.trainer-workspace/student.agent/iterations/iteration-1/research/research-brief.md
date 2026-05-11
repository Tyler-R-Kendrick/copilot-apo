# Research Brief: student.agent.md

## Target

`.github/agents/student.agent.md` — teacher-guided candidate revision agent inside trainer-led prompt optimization loops.

## Primary Sources Found

No public benchmark directly addresses teacher-student prompt revision loop discipline, agent scope enforcement within multi-agent orchestration, or teacher-approval forecast accuracy. This domain is specific to agentic prompt-optimization workflows.

**Closest adjacent literature (informative only, not directly grounded):**

| Source | Relevance | Status |
|--------|-----------|--------|
| Constitutional AI (Anthropic, 2022) | Iterative self-critique and revision loop patterns | Rejected — no traceable eval annotation guide; secondary paraphrase risk |
| RLHF literature (InstructGPT, 2022) | Human-feedback incorporation into generation | Rejected — no eval row mapping; not behavioral-contract grounded |
| ReAct (Yao et al., 2022) | Reasoning trace and action interleaving | Rejected — trace evaluation doesn't map to revision scope enforcement |
| Self-Refine (Madaan et al., 2023) | Iterative self-improvement with feedback | Informative for "minimal revision" principle; rejected as primary dataset source — no annotation license |
| AgentBench (Liu et al., 2023) | Agent behavior benchmarking | Rejected — does not cover revision scope or teacher handoff discipline |

## Rejection Summary

All adjacent sources were rejected because:
1. No source provides a named benchmark maintainer with an explicit evaluation license for agent-contract behavioral tests.
2. No source maps directly to the student agent's specific behaviors (evidence reading order, scope enforcement, teacher-approval forecast).
3. Contamination risk from academic benchmark reuse in a proprietary training context.

**Stop recommendation**: No primary public dataset clears the approval bar for this target. Proceed with behavioral synthesis from the agent contract.

## Synthesis Plan

Synthesize eval cases from the student agent's behavioral contract directly. Each case should target one of five evaluable behaviors:

### Behavior 1: Evidence Reading Discipline
Cases where the student agent has incomplete or missing steering artifacts. Expected: hands off to teacher rather than drafting from incomplete context.

### Behavior 2: Scope Enforcement
Cases where a request tries to get the student to take over judging, adversarial review, or trainer orchestration. Expected: refuses and stays within revision role.

### Behavior 3: Reasoning Trajectory Quality
Cases where the student produces a revision. Expected: output includes explicit plan, tradeoffs, and uncertainty — not just the revised text.

### Behavior 4: Teacher-Approval Forecast Accuracy
Cases where the student forecasts approval. Expected: forecast is grounded in concrete criteria (smallest change, reasoning explicit, no scope expansion, no evaluator fields).

### Behavior 5: Minimal Revision Principle
Cases where multiple valid revision paths exist. Expected: student chooses the smallest defensible change that addresses the critique without expanding scope.

## Mapping Notes

All rows use `llm_judge` scoring because:
- Behavioral compliance cannot be checked by exact string match
- Reference + criteria format supports the necessary nuanced assessment

Row shape: `{ "input": "<scenario>", "reference": "<expected behavior>", "criteria": "<scoring rubric>", "scoring": "llm_judge" }`

## Unresolved Gaps

- No held-out test set exists for teacher-student loop dynamics; all cases are synthetically generated.
- Teacher-approval forecast accuracy is difficult to evaluate without actual teacher judgments; using behavioral proxies (explicit criteria mentioned, grounded in revision text).
