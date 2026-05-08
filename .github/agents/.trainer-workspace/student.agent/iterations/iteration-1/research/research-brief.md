# Research Brief: student.agent.md Optimization

## Target
`.github/agents/student.agent.md` — a teacher-guided candidate revision specialist in trainer-led prompt optimization loops.

## Key Optimization Question
How should a student agent in a teacher-student optimization loop signal exit conditions, scope its revisions, and structure reasoning trajectories to produce useful, bounded output?

## Patterns from Repository Evidence

### Pattern 1: Explicit exit criteria prevent unbounded loops
The adversary, teacher, and researcher agents all use explicit stop conditions: "stop when teacher predicts no further improvement, student predicts teacher approval, or the active iteration reaches a reasonable turn cap." The student agent's current constraint ("do at most one extra self-check") is less precise and relies on subjective assessment.

**Recommendation:** Add a concrete multi-condition exit criterion that mirrors the repo's established loop-bounding pattern.

### Pattern 2: Approval prediction should reference specific signals
The teacher agent's self-check says "After drafting the steering once, do at most one extra self-check." The student's current wording ("if the draft still looks unsupported") is vague. Approval prediction should reference observable signals (critique gap coverage, no contradictions with latest STEERING.md, at least one measurable change in the candidate).

### Pattern 3: Triggering descriptions benefit from a positive task signal + context constraint
Looking at adversary.agent.md and teacher.agent.md: both include both the positive task ("Use when stress-testing…", "Use when reviewing optimization artifacts…") and an implicit scope signal. The student description is weaker because "from teacher guidance" is a qualifier, not a trigger condition. A better pattern: state the specific input artifact that triggers the agent.

### Pattern 4: Engineer handoff boundary needs a negative constraint
The engineer handoff description ("format your reasoning trajectory and solution plan") may cause students to offload the revision thinking itself rather than formatting. The adversary and researcher agents define the engineer handoff more explicitly with a concrete scope boundary.

### Pattern 5: "Smallest defensible" needs a measurable anchor
The researcher agent's "smallest defensible" guidance is anchored to specific evidence (critique gap, steering artifact): the constraint is checkable. The student's usage is unanchored. A practical anchor: "implements the change the critique names and does not touch any part of the prompt the critique does not reference."

## Source Shortlist
- Internal: `.github/agents/adversary.agent.md` (stop conditions pattern, artifact contract)
- Internal: `.github/agents/teacher.agent.md` (self-check boundary, engineer handoff scope)
- Internal: `.github/agents/researcher.agent.md` (triggering description precision, exit criterion)
- Internal: `engineer-prompt/review.md` (failure mode analysis and hypothesis)
- Internal: `.github/agents/.trainer-workspace/adversary.agent/iterations/iteration-1/steering/teacher/turn-1/STEERING.md` (teacher critique pattern reference)

## Stop Recommendation
No public-source data needed. The optimization target is a repository-internal agent contract. All supporting evidence is available from existing workspace artifacts and sibling agent contracts.

## Schema Notes for Synthesis
- Use `scoring: "llm_judge"` for all train/val rows.
- Each row: `input` (simulated student invocation context), `reference` (expected agent behavior), `criteria` (list of checkable signals), `scoring`.
- Cover: normal revision, escalation to teacher, no-op, engineer handoff trigger, exit condition signaling.
