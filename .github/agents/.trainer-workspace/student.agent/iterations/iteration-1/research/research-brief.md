# Research Brief: student.agent.md Optimization

## Target Layout

- **Target file**: `.github/agents/student.agent.md`
- **Workspace**: `.github/agents/.trainer-workspace/student.agent/`
- **Eval manifest**: `iterations/iteration-1/synthesize/evals/evals.json`
- **Train dataset**: `iterations/iteration-1/synthesize/datasets/train.jsonl`
- **Val dataset**: `iterations/iteration-1/synthesize/datasets/val.jsonl`

## Task Description

`student.agent.md` is a teacher-guided candidate revision agent. Its core task is:
1. Absorb teacher critique and workspace steering
2. Inspect current candidate and evidence
3. Implement the smallest defensible revision
4. Expose the reasoning trajectory for teacher review
5. Predict teacher approval before finalizing

## Query Plan

Primary task: evaluate whether a student agent follows evidence reading order, implements correctly-scoped revisions, predicts teacher approval accurately, and reports justified no-ops when appropriate.

### Grounding Sources

This agent does not have a published academic benchmark directly applicable. The eval cases are grounded in the agent's own contract:
- The agent contract (student.agent.md) defines expected behavior
- The engineer-prompt review (engineer-prompt/review.md) defines failure modes and desired improvements
- Sibling agent contracts (teacher.agent.md, trainer.agent.md) define the collaboration protocol

### Approval Bar

Cases are internally grounded — sourced from the agent contract, engineering review, and sibling agent contracts. No external datasets required for this optimization pass.

## Approved Sources

1. **student.agent.md** (current contract) — primary source for current behavior baseline
   - Maintainer: repository authors
   - License: repository license
   - Fit: defines expected input/output contract and constraints
   - Risk: none (first-party artifact)

2. **engineer-prompt/review.md** — identified failure modes and optimization hypotheses
   - Maintainer: this trainer run
   - License: repository license
   - Fit: documents gaps in evidence reading order, loop exit, no-op condition, engineer handoff

3. **teacher.agent.md** — collaboration protocol and steering format
   - Fit: defines what teacher critique looks like, what STEERING.md artifacts contain

4. **trainer.agent.md** — orchestration contract
   - Fit: defines the loop the student operates within

## Rejected Candidates

- External academic datasets on iterative refinement (COMET, WMT): too distant from agent-contract revision tasks
- Generic RLHF datasets: not grounded in this specific agent's contract and evidence-reading discipline

## Mapping Notes

| Source Field | Eval Row Field |
|---|---|
| Teacher critique scenario | `input` |
| Expected compliant revision behavior | `reference` |
| Behavioral compliance checks | `criteria` |
| Judge mode | `scoring: llm_judge` |

All rows use `llm_judge` because the student agent's output is an open-ended natural-language revision with reasoning trajectory — exact-match scoring is not appropriate.

## Unresolved Gaps

None. The agent contract and engineering review provide sufficient grounding for 6–8 eval cases covering the main failure modes.
