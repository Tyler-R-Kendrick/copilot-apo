# Decision: student.agent.md — Iteration 1

## Selected Candidate

**Student candidate** (trainer-synthesized + trainer-agent-answered model_prompt)

## Changes Applied

The optimized prompt adds seven structural improvements to `student.agent.md`:

1. **Engineer handoff criterion narrowed** (intro paragraph): Changed from "format your reasoning trajectory and solution plan into a clearer teacher-ready explanation when the task needs prompt-engineering or Trace-oriented expertise, or when your draft rationale needs better structure" to "only when the reasoning explanation needs prompt-engineering or Trace-specific reframing for the teacher — not for general formatting cleanup." This prevents unnecessary engineer handoffs for routine output cleanup.

2. **Missing-evidence blocker path added** (intro paragraph): Added "Also hand off to `teacher` when required workspace evidence is missing (no STEERING.md, no source snapshot) rather than producing a speculative revision or a silent no-op." This closes the gap where a student agent could produce a silent no-op on missing evidence.

3. **Evidence Order section added** (new section): Explicit 5-step numbered read order — teacher goal, latest critique, STEERING.md (with stop-and-hand-off-if-absent gate), per-agent summary.md, source snapshot. Stop condition: "Stop reading and begin drafting once these five inputs are collected or their absence is documented."

4. **Operational "smallest defensible revision" definition** (Constraints): Replaced vague "Implement the smallest defensible candidate revision that addresses the current critique or blocker" with "**Smallest defensible revision**: the revision that addresses exactly the current `STEERING.md` focus without adding new constraints, expanding scope, or changing the prompt interface. A revision that introduces changes outside the current steering focus is out of scope regardless of their merit."

5. **Conflicting-critique resolution rule added** (Constraints): "When consecutive teacher turns conflict, apply the most recent turn's guidance and name the conflict and tradeoff explicitly in the reasoning trajectory. Do not silently discard earlier guidance."

6. **Stopping Condition section added** (new section): Three explicit criteria: (a) teacher would approve, (b) revision scope was smallest addressable unit after one self-check, (c) draft still unsupported after one self-check → stop and request teacher turn. Explicit cap: "Do not run more than one self-check per turn."

7. **Output Format clarified** (Output Format section): Chain-of-thought named as default; tree-of-thought for branching decisions; chain-of-uncertainty-thought for high-stakes tradeoffs. Added: "Do not mix formats in a single output unless different steps genuinely require different styles." Removed "sketch-of-thought" as a format option (not a documented reasoning style in the repo).

## Adversary Review Summary

The adversary found one credible exploit: replacing the STEERING.md-absent blocker with an "infer from teacher critique" inference license. This exploit looks like a convenience improvement but removes the most important safety gate. The exploit does NOT outrank the student candidate because the training eval set includes an explicit missing-evidence test case (train row 3). 

Extra judge steering: guard against any candidate that replaces the STEERING.md-absent blocker with inference-from-critique fallback.

## Validation

`python -m pytest -q`: **856 passed** — no regressions.

Test `test_student_agent_contract_structure` updated to reflect the narrowed engineer handoff text, the operational "smallest defensible revision" definition, and the chain-of-thought-first output format (replacing the former comma-separated format list including sketch-of-thought).

## Workspace

`.github/agents/.trainer-workspace/student.agent/`
- Latest iteration: `iterations/iteration-1/`
- Optimize: `manual_followup` (no model credentials; trainer answered model_prompt)
- Datasets: 6 train + 2 val rows, `judge_mode=llm_judge`
- Steering: teacher turn-1
- Adversary: blocker-removal exploit found, does not outrank student on full eval set
