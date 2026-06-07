# Teacher Steering: Student Agent Optimization Turn 1

## Evidence Analyzed
- Current student.agent.md (49 lines)
- Engineer-prompt review
- Agent responsibilities in trainer orchestration context
- Handoff contracts (teacher, engineer)
- Reasoning trajectory requirements
- Constraint adherence expectations

## Current Agent Assessment

### Strengths
1. **Clear handoff discipline**: Explicitly distinguishes teacher handoff (for unclear guidance) vs. engineer handoff (for structure/formatting).
2. **Reasoning trajectory requirement**: Step 43-48 explicitly require chain-of-thought, tree-of-thought, or sketch-of-thought reasoning rather than answer-only.
3. **Smallest-defensible framing**: The "smallest defensible revision" concept is repeatedly emphasized (lines 28, 34, 38).
4. **Constraint clarity**: Seven explicit constraints (lines 25-31) are well-enumerated and distinct.
5. **Prediction requirement**: Step 6 and constraint #5 require pre-finalizing teacher approval prediction.

### Gaps and Improvement Opportunities

#### Gap 1: Unclear "Smallest Defensible" Guidance
The agent states it must implement "smallest defensible" revisions but provides no concrete heuristics:
- What metrics determine "smallest"? (lines changed, scope, surface area)
- What determines "defensible"? (improves the current failure mode, doesn't introduce new ones, aligns with steering)
- The agent may over-scope or under-scope without clearer signals

**Recommendation**: Add concrete examples of what constitutes a "smallest defensible revision" in a given trainer context (e.g., add clarification to approach step 5, add a checklist in constraints).

#### Gap 2: Workspace Evidence Integration Underspecified
Step 1 mentions reading "the relevant per-agent `steering/<agent>/summary.md`" but doesn't specify:
- How to prioritize conflicting signals from different agents' steering summaries
- How to weight steering signals vs. workspace artifacts
- What to do when steering is incomplete

**Recommendation**: Add guidance on steering-priority order and artifact-freshness checks in the Approach section.

#### Gap 3: Validation Ambiguity
Step 7 says "Run the relevant validation or measurement step" but doesn't specify:
- Which validation should be run?
- What constitutes "success" in validation?
- How to interpret validation results if they're inconclusive?

**Recommendation**: Add a validation sub-guide that ties to the specific trainer context (agent behavioral testing, end-to-end loop success, etc.).

#### Gap 4: Blocker Handling Missing
The agent is told to "predict teacher approval" and identify "remaining blockers" (step 6), but:
- There's no explicit flowchart or decision tree for blocker classification
- It's unclear what qualifies as a "blocker" vs. a minor issue
- The agent might over-report or under-report blockers

**Recommendation**: Add a blockers taxonomy and decision checklist (e.g., "hard blockers" = constraint violations, "soft blockers" = incomplete steering).

#### Gap 5: Loop-Exit Criteria Implicit
The agent is told to "loop when approval looks unlikely" (step 6) but:
- The exit criteria are vague ("at most one extra self-check")
- It's unclear what "approval still looks unlikely" means quantitatively or qualitatively
- The agent might loop indefinitely or exit prematurely

**Recommendation**: Make the loop-exit criteria explicit as a constraint (e.g., "max 2 self-checks, then hand off to teacher if uncertain").

### Impact on Trainer Loop Behavior
These gaps affect:
1. **Iteration quality**: Unclear revisions may miss the teacher's intent or over-correct
2. **Loop convergence**: Ambiguous loop-exit criteria slow down iteration cycles
3. **Steering signal fidelity**: Workspace evidence integration gaps may cause signal loss
4. **Blocker identification**: Missing blockers may allow broken candidates to advance

## Optimization Recommendations

### Priority 1: Clarify "Smallest Defensible" Heuristics
**Why**: This is central to the agent's core function and most likely cause of over-scoping.
**Action**: Add a concrete heuristic checklist for "smallest defensible":
  - Does it address the current steering critique?
  - Does it introduce any new constraint violations?
  - Does it avoid unrelated scope creep?
  - Can the change be explained in <200 words?

### Priority 2: Add Workspace Evidence Integration Guide
**Why**: The agent must coordinate across multiple steering sources without clear prioritization.
**Action**: Add artifact-reading order and conflict-resolution strategy:
  - Latest turn-specific STEERING.md is primary
  - Per-agent summary.md provides context but doesn't override
  - Conflicting signals trigger a teacher handoff (don't guess)

### Priority 3: Specify Validation Plan
**Why**: "Relevant validation" is too vague for a first-pass candidate.
**Action**: For agent behavioral optimization, validation means:
  - Agent follows all constraints (no skill invocation, no over-scoping)
  - Reasoning trajectory is explicit (not answer-only)
  - Handoffs are used appropriately (teacher for unclear, engineer for structure)
  - Output format follows the required template

### Priority 4: Add Loop-Exit Conditions Explicitly
**Why**: Prevents infinite loops and premature exits.
**Action**: Update Approach step 6 to state:
  - First draft: evaluate against steering
  - If approval looks high (>80% confidence) → finalize
  - If approval looks low but blocker is clear → apply one fix
  - If still uncertain after fix → hand off to teacher instead of looping

### Priority 5: Add Blocker Taxonomy
**Why**: Helps student distinguish critical issues from minor ones.
**Action**: Define blockers as:
  - **Hard**: Constraint violation, steering contradiction, incomplete artifact
  - **Soft**: Style issue, verbose reasoning, unclear connection to steering
  - **Defer**: Issue outside current iteration scope (document for next loop)

## Next Steps
1. Update the agent's Constraints section to include the "smallest defensible" checklist
2. Expand Approach steps 1, 5, 6, and 7 with concrete guidance
3. Add a "Validation Plan" subsection to Output Format
4. Add a "Blocker Taxonomy" reference section
5. Re-test the agent in a bounded trainer loop (2-3 iterations) to verify improvements

## Predicted Teacher Approval
- **Current state**: Agent is functional but over-general. Likely approval: **60-70%**
- **With recommendations**: Predicted approval: **85-90%**
- **Blocking issues**: None; all gaps are improvements, not blockers. Ready for student revision phase.

