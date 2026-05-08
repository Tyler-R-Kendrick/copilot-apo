# Decision Summary: student.agent.md Optimization

## Target
`.github/agents/student.agent.md`

## Workspace
`.github/agents/.trainer-workspace/student.agent/`

## Selection Reason
First `.agent.md` file by path ascending with no existing `.trainer-workspace/student.agent/` directory. All `.instructions.md` targets already had workspaces.

## Optimization Result
**Status:** Complete — winning candidate written back to source.
**Iteration:** iteration-1
**Optimize mode:** manual_followup (VERL requires torch; APO not a FastAlgorithm in this environment)

## Changes Made to Source File

Five targeted improvements applied:

1. **Description tightened** (triggering accuracy): Changed from "from teacher guidance" to "after receiving a teacher critique or STEERING.md steering artifact" so callers can distinguish when to invoke student vs. teacher first.

2. **Exit criteria added** (loop-bounding): Added an explicit three-condition exit list to the Constraints section. Condition (b) requires a named, externally-written teacher STEERING.md artifact—not self-assessment—blocking the adversary's primary self-certification exploit.

3. **"Defensible" anchor added** (revision scope measurability): Inline observable test added: "the revision addresses exactly what the critique names and does not touch any part of the prompt the critique does not reference."

4. **Engineer handoff bounded** (handoff scope): Added "Do not take over execution; improve structure and clarity only." to the Request Engineer Guidance prompt.

5. **Output length guidance added**: Output Format closes with "Keep each section to 2-3 sentences unless the complexity requires more; omit sections with nothing material to report." with an explicit exception prohibiting brevity suppression of the approval-prediction section.

6. **Argument-hint updated** (residual teacher weakness): Matches the new description specificity—now references "STEERING.md steering artifact."

## Adversarial Review
Primary exploit found: self-certification deepening + brevity suppression compound. Predicted to score 0.87 vs 0.80 against the initial manual-followup candidate. Blocked in final candidate by:
- Condition (b) now requires a named teacher STEERING.md artifact (externally verifiable)
- Approval-prediction section explicitly cannot be abbreviated when a stop condition is invoked

## Validation
`python -m pytest -q` → 856 passed in 8.99s

## Remaining Open Items
- The engineer-handoff-as-critique-reading-proxy exploit surface is not yet covered by any eval row (multi-turn trajectory eval would be needed).
- The `argument-hint` update was also applied in the adversary candidate but is legitimate and kept.
