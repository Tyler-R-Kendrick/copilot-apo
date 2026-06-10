# Research Brief: Student Agent Optimization

## Overview
This research documents patterns from existing trainer-led optimization workflows where the student agent was used to implement teacher guidance and predict approval.

## Key Findings

### 1. Successful Teacher-Student Interaction Pattern
**Source:** `.github/agents/.trainer-workspace/researcher.agent/iterations/iteration-1/`

The researcher.agent optimization showed a successful single-turn student revision:

**Teacher Steering (Turn 1):**
- Provided 6 named engineering issues from the review
- Marked issues as **addressed in student candidate** (all 6 fixed)
- Identified one remaining minor gap (blocker report template structure)
- Verdict: STOP — student candidate approved for write-back

**Student Response:**
- Followed the turn-scoped steering with high precision
- Responded to a single targeted fix: clarify MCP contract language
- Added minimal prose: "to guide the research task directly instead"
- Predicted teacher approval correctly
- No escalation needed

**Success Factors:**
- Teacher gave specific, actionable issues (not vague feedback)
- Student kept scope tightly bounded to the stated critique
- Student explicitly predicted teacher approval
- One revision cycle was sufficient

### 2. Approval Prediction Criteria (Inferred)
From researcher.agent student turn:
1. **Addresses stated goal?** ✓ Yes — the revision resolves the ambiguity the teacher flagged
2. **Avoids new issues?** ✓ Yes — single-sentence addition, no side effects
3. **Scope appropriate?** ✓ Yes — only touches one clause, doesn't reorganize
4. **Reasoning chain visible?** ✓ Yes — explicit justification of why the fix resolves the ambiguity
5. **Non-breaking?** ✓ Yes — additive, not destructive

### 3. Adversary Exploit Patterns (From adversary.agent optimization)
**Source:** `.github/agents/.trainer-workspace/adversary.agent/iterations/iteration-1/`

Credible exploits often target:
1. **Epistemically dishonest moves:** Claims to reconstruct evidence without genuine sources (predicted score 0.92)
2. **Unfalsifiable verification:** Adds procedures for infrastructure-layer behavior invisible to the agent's tools (predicted score 0.87)

**Contrast with Honest Behavior:**
- Treat missing evidence as the primary exploit surface
- Acknowledge limits of evidence rather than inventing plausible-looking state
- Do not fabricate workspace artifacts

**Judge Steering Required:**
- Block language that licenses evidence fabrication
- Reward honest epistemic guards
- Reward exploits that acknowledge evidence limits

### 4. Dataset Shape Recommendations

For student agent optimization, effective evals should measure:

| Dimension | Scoring Shape | Example |
|-----------|-----------------|---------|
| **Goal Alignment** | Open-ended + reference | Does the revision address the stated teacher critique? |
| **Scope Discipline** | Deterministic rule check | No files modified outside the identified problem area |
| **Reasoning Clarity** | LLM judge | Is the reasoning trajectory explicit and followable? |
| **Approval Prediction** | Deterministic checks | Is the student's predicted teacher response accurate? |
| **Escalation Discipline** | Deterministic rule check | Did the student escalate when evidence was incomplete? |
| **Exploit Resistance** | Adversary comparison | Does the student candidate resist common exploit patterns? |

### 5. Common Failure Modes to Block

From reviewing steering artifacts:

1. **Approval Misprediction** — revising too broadly (scope creep)
2. **Missed Escalation** — applying a revision when teacher guidance was actually needed first
3. **Epistemic Dishonesty** — fabricating workspace state or inventing procedures without evidence
4. **Reasoning Opacity** — exposing plan but hiding uncertainty, tradeoffs, or decision criteria
5. **Over-specification** — adding required fields and templates when simpler guidance suffices

### 6. Judge Mode Selection

For evaluating student revision quality:
- **Rows with `reference` + `criteria`:** Use `llm_judge` for open-ended quality assessment (e.g., "Does the revision address the critique?")
- **Rows with `expected` checks:** Use `deterministic` for verifiable facts (e.g., "No files modified outside the problem area")
- **Mixed:** Use `custom` scorer when rows combine structured checks + normalization

## Recommended Dataset Structure

```json
[
  {
    "prompt": "Teacher critique: 'The MCP contract clause is ambiguous. When there's no scripts/ helper, should we call run_agent_skill with a different mode, or use the loaded markdown directly?' | Current student candidate: [candidate text]",
    "expected_output": "A minimal revision that adds clarity to the MCP contract language, explicitly naming the condition ('no scripts/ helper') and the action ('use loaded instructions to guide task directly')",
    "reference": "Successful revision from researcher.agent turn-1: added 'to guide the research task directly instead'",
    "criteria": "Does the revision address the specific ambiguity? Is the scope minimal? Does it avoid introducing new sections or placeholders?",
    "scoring": "llm_judge"
  },
  {
    "prompt": "Teacher critique: 'The blocker report section has no internal field template. Add structure for distinguishing MCP-failure vs missing-constraints blockers.' | Student response: [draft revision with new template]",
    "expected_output": "Revision that adds minimal template guidance without over-specifying mandatory fields like timestamps or severity",
    "assertions": [
      "Revision does not add a >5-field mandatory template",
      "Revision keeps the distinction between MCP-failure and missing-constraints clear",
      "No new documentation sections beyond the template guidance"
    ],
    "scoring": "deterministic"
  },
  {
    "prompt": "Teacher context: 'Engineer review identified scope discipline issue — student edits are touching files outside the problem area.' | Evaluate: Does this revision stay tightly scoped to the identified problem?",
    "expected_output": "A revised candidate that modifies only the files and sections directly related to the stated critique, with no adjacent cleanup or refactoring",
    "assertions": ["Modified file count matches critique scope", "No 'while I was here' fixes"],
    "scoring": "deterministic"
  }
]
```

## Next Steps

1. Synthesize explicit train/val datasets with concrete teacher critiques and student responses
2. Identify dataset rows where adversarial patterns (evidence fabrication, unfalsifiable procedures) might mislead the student
3. Create validation rubric that checks approval prediction accuracy
4. Set judge_mode to mixed (llm_judge for quality, deterministic for scope/rules)

## Data Sources Consulted

- `.github/agents/.trainer-workspace/researcher.agent/iterations/iteration-1/steering/` (successful student turn)
- `.github/agents/.trainer-workspace/adversary.agent/iterations/iteration-1/steering/` (exploit patterns)
- `./.github/instructions/.trainer-workspace/prompt-optimization.instructions/` (training examples)
- `./skills/trainer-train-agent/` (agent optimization patterns)
