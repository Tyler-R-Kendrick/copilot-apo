# Research Brief: Student Agent Evaluation

**Target:** `.github/agents/student.agent.md`
**Research date:** 2026-05-05
**Researcher:** trainer orchestrator (manual brief, no model API)

## Target Layout

The student agent is a teacher-guided candidate revision specialist. Key interface:
- **Input:** Teacher critique, STEERING.md artifact, current candidate prompt, workspace evidence
- **Task:** Implement the smallest defensible revision, expose reasoning trajectory
- **Output:** Revision or no-op + reasoning trajectory + teacher-approval prediction + validation result

## Primary Sources

### 1. Iterative Text Revision Benchmarks (RLHF / feedback-based)
- **Source:** RLHF evaluation literature (Anthropic Constitutional AI, OpenAI PPO papers)
- **Maintainer:** Anthropic, OpenAI (documented)
- **License:** Research publications (not directly a dataset, but establishes eval patterns)
- **Applicability:** Establishes the pattern of revision-from-critique as the core task shape
- **Mapping:** Input = critique + candidate; Output = revised candidate; Judge = LLM scoring revision quality
- **Status:** Approved as pattern reference, not as direct data source

### 2. Repo-internal Agent Eval Patterns
- **Source:** `.github/agents/.trainer-workspace/researcher.agent/iterations/iteration-1/synthesize/evals/evals.json`
- **Maintainer:** Tyler Kendrick (this repository)
- **License:** MIT (per skills-lock.json)
- **Applicability:** Direct template for eval row shape: `prompt`, `reference`, `criteria`, `scoring: "llm_judge"`
- **Mapping:** Each row is an agent invocation scenario; reference is the expected behavior; criteria is the judge rubric
- **Status:** Approved as primary template

### 3. Trainer-Train-Agent Datasets
- **Source:** `.agents/skills/trainer-train-agent/datasets/train.jsonl`
- **Maintainer:** Tyler Kendrick (this repository)
- **License:** MIT
- **Applicability:** Shows how agent loop evaluation rows are structured for trainer agents
- **Mapping:** Same `prompt + reference + criteria + scoring` shape
- **Status:** Approved as structural template

### 4. Chain-of-Thought Evaluation Literature
- **Source:** Wei et al. 2022 "Chain-of-Thought Prompting Elicits Reasoning in Large Language Models", Google AI
- **License:** Research paper (public)
- **Applicability:** The student agent is specifically required to expose reasoning trajectories; CoT quality is measurable
- **Mapping:** Eval cases should test whether the agent exposes explicit reasoning steps, not just outputs
- **Status:** Approved as pattern reference

## Rejected Sources

- **Wikipedia articles on iterative design:** No accountable benchmark maintainer, contamination risk
- **Generic NLP benchmarks (GLUE, SuperGLUE):** Not relevant to agent collaboration or revision tasks
- **Stack Overflow answer revision data:** No structured feedback/critique component

## Row Shape Recommendation

```json
{
  "prompt": "<realistic user request or teacher-student scenario>",
  "reference": "<expected agent behavior description>",
  "criteria": "<evaluation rubric for the judge>",
  "scoring": "llm_judge"
}
```

## Judge Mode

**Recommended: `llm_judge`**
Rationale: Agent behavior quality (revision scope, reasoning transparency, handoff discipline) is open-ended. No exact-match answer exists. All rows will have `reference + criteria` fields.

## Eval Case Ideas

1. **Evidence reading order:** Student receives STEERING.md + teacher critique. Does it read STEERING.md first?
2. **Smallest defensible revision:** Student receives critique naming one failure mode. Does it limit scope?
3. **Reasoning trajectory exposure:** Student implements revision. Does it expose the plan and tradeoffs?
4. **Teacher-approval prediction:** Student predicts approval. Is the prediction grounded in named criteria?
5. **Escalation discipline:** Student revised twice without approval. Does it escalate to teacher?
6. **Engineer handoff:** Student has complex reasoning to format. Does it use engineer handoff?
7. **No-op justification:** Student receives contradictory critique. Does it report no-op with evidence?
8. **Missing STEERING.md blocker:** STEERING.md is missing. Does the student report a blocker?

## Unresolved Gaps

- No large public dataset specifically for teacher-student revision loop quality exists.
- All eval cases must be hand-crafted or synthesized from repo patterns.
- The "teacher-approval prediction accuracy" metric requires ground-truth labels that would need human annotation.

## Stop Recommendation

No stop recommended. Sufficient pattern data exists in-repo to synthesize 8-10 quality eval cases. Proceed to synthesis.
