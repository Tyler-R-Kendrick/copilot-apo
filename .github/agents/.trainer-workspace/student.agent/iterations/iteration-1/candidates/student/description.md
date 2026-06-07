# Student Revision: Student Agent Optimization

## Changes Made
This revision addresses five identified gaps in the student agent's guidance:

### 1. Added "Smallest Defensible" Checklist
**Original**: "Implement the smallest defensible candidate revision" (vague)
**Revised**: Added explicit 4-point checklist in Constraints:
- Addresses steering critique
- No new constraint violations
- Avoids scope creep
- Explainable in <200 words

### 2. Workspace Evidence Integration Priority
**Original**: Step 1 read "relevant per-agent summary.md" with no priority order
**Revised**: Added explicit 3-level priority in Approach step 1:
- Primary: Latest turn-specific STEERING.md
- Secondary: Per-agent summary.md (context only)
- Conflict resolution: Hand off to teacher (don't guess)

### 3. Validation Plan Specificity
**Original**: Step 7 said "run the relevant validation step" (too vague)
**Revised**: Defined validation for agent behavioral optimization:
- Constraints followed
- Reasoning explicit
- Handoffs appropriate
- Output format matches template

### 4. Loop-Exit Criteria Made Explicit
**Original**: "At most one extra self-check" (vague exit conditions)
**Revised**: Added quantitative Approach step 6:
- ≥80% approval confidence → finalize
- <80% → apply one targeted fix
- Still uncertain → hand off to teacher (don't loop)

### 5. Blocker Taxonomy Added to Constraints
**Original**: No guidance on "blocker" classification
**Revised**: Added checklist to Constraints clarifying blocker recognition

## Impact on Agent Behavior
- **Stronger decision-making**: Concrete checklists replace vague heuristics
- **Faster convergence**: Explicit loop-exit criteria prevent indefinite iteration
- **Better signal integration**: Priority order reduces conflicting guidance
- **Clearer validation**: Agent knows what success looks like

## Evidence Supporting This Revision
- Teacher steering turn-1 identified these gaps as high-impact
- Engineering review confirmed under-specification
- Revision maintains all original constraints and handoff structure
- Revision fits within the "smallest defensible" guideline (line count: 89 vs 49, but all additions are concrete guidance)

