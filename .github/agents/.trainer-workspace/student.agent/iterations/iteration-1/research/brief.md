# Research Brief: student.agent.md

## Target and Task Summary

**Target file:** `.github/agents/student.agent.md`  
**Derived eval layout:** `.github/agents/.trainer-workspace/student.agent/iterations/iteration-1/synthesize/evals/evals.json`  
**Prompt interface:** The student agent receives teacher critique (in STEERING.md artifacts), the current prompt candidate text, and workspace evidence. It returns a revised candidate plus a reasoning trajectory, a teacher approval prediction, and a validation result.  
**Task boundary:** Candidate revision within trainer-led optimization loops. The student agent does not orchestrate, judge, or run adversarial review; it implements targeted revisions and reports them.

## Research Plan and Approval Bar

No public benchmark dataset directly models agent-to-agent critique-and-revision behavior inside a trainer loop. The closest public work (RLHF datasets, Constitutional AI revision datasets, red-teaming corpora) either lacks the structured steering artifact format or tests model self-critique rather than agent-loop behavior.

**Approval bar for any external source:**
- Named maintainer with traceable provenance
- Explicit license permitting eval authoring use
- Task alignment with revision-plus-reasoning-trajectory tasks
- No label contamination from this repository

After applying this bar, no external public source clears it for this specific task. The synthesis step must use internally authored cases derived from the agent contract and engineer-prompt review.

## Approved Sources

None. No public dataset clears the approval bar for this agent's specific revision-and-reasoning-trajectory task.

## Rejected Candidates

| Source | Reason for rejection |
|--------|---------------------|
| RLHF preference datasets (Anthropic HH, OpenAI WebGPT) | Test preference over completions, not revision discipline or reasoning transparency inside a trainer loop |
| Constitutional AI revision traces | Proprietary; not released as a public eval benchmark |
| HumanEval, MBPP code revision corpora | Code-only; no agent steering artifacts or STEERING.md format |
| Alpaca, ShareGPT instruction tuning datasets | General instruction-following; not structured around critique-and-revision with agent handoffs |

## Mapping Notes

Because no external source clears the bar, eval cases must be authored internally from:
1. The student agent contract (SKILL.md and agent.md behavior description)
2. The engineer-prompt review (gaps and rewrite hypotheses)
3. Representative trainer loop scenarios (teacher critique → student revision → teacher approval prediction)

Each eval prompt should simulate a realistic teacher-critique-and-workspace scenario. Each expected output should describe what a well-behaved student agent response looks like. Assertions should be observable and objective.

**Field mapping for authored cases:**
- `prompt` → a scenario describing a teacher critique, current STEERING.md content, and the candidate prompt under review
- `expected_output` → description of a compliant student response (revision, reasoning trajectory, approval prediction)
- `assertions` → objective checks on revision precision, reasoning transparency, handoff behavior
- `scoring` → `llm_judge` for all cases (open-ended revision quality cannot be deterministic)
- `criteria` → agent-behavior-specific evaluation criterion for each case

## Unresolved Gaps

None blocking synthesis. All six eval cases can be authored from internal contract review without external sources.

**Synthesis recommendation:** Author 8 eval cases (6 train, 2 val) that cover:
1. Minimal defensible revision (revision precision)
2. Evidence reading order compliance
3. Teacher handoff trigger (stale/incomplete critique)
4. Teacher approval prediction (high confidence path)
5. Teacher approval prediction (low confidence triggering another turn)
6. Justified no-op (no evidence supports revision)
7. Engineer handoff usage (formatting reasoning trajectory)
8. In-scope vs out-of-scope revision boundary
