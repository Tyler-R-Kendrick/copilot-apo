# Student Candidate Description

**Source:** `iterations/iteration-1/optimize/optimized-prompt.md` (agent-authored manual-followup candidate)

**Improvements over original:**
1. Teacher handoff trigger replaced with three explicit named conditions (stale STEERING.md, contradictory artifacts, unresolvable target)
2. Approval prediction given a concrete three-criteria rubric (addresses named failure mode, no new scope, preserves interface)
3. Steering artifact priority order added (latest turn STEERING.md → summary.md → review.md → earlier turns)
4. Reasoning format guidance replaced with decision rule: chain-of-thought for linear, tree-of-thought for mutually exclusive options, chain-of-uncertainty-thought for missing facts
5. Validation definition of done added: `python -m pytest -q` for source changes, diff review for prompt-only changes
6. Over-revision check added: flag if more than two structural elements change
7. Engineer handoff scope tightened: formatting only after revision is decided

**Predicted teacher approval:** Likely to approve. All six named failure modes from the review are addressed. No new scope introduced. Frontmatter, tool list, and handoff labels unchanged.
