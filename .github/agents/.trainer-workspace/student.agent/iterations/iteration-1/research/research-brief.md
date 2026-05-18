# Research Brief: student.agent.md

## Target

`.github/agents/student.agent.md` — trainer-led teacher-student candidate revision agent.

## Scope

This research pass identifies the specific behavioral failure modes in teacher-student revision loops from the prompt-engineering literature, and derives a minimal rewrite scope for `student.agent.md`.

## Relevant Public Sources

1. **Chain-of-Thought Prompting** (Wei et al., 2022): Teacher-student revision loops benefit from explicit reasoning trajectory exposure. The student should document not just the revision, but the reasoning path (stepwise, branching, or sketch) that led to it.

2. **SELF-REFINE** (Madaan et al., 2023): Iterative self-improvement prompts benefit from a concrete stopping criterion. Without one, agents loop indefinitely or stop too early. The stopping condition should be observable (e.g., "teacher would approve" or "already used teacher handoff once").

3. **Constitutional AI Critique-Revision** (Bai et al., 2022): Critique-revision agents work best when they follow a fixed evidence order — critique first, then current artifact, then baseline. This prevents confirmation bias toward the current candidate.

4. **RLHF Labeling for Agent Assistance** (Stiennon et al., 2020): Revision quality is higher when the revising agent explicitly compares the new version to the original, not just to the critique. A comparison step improves precision and prevents scope creep.

5. **Agent Workspace Staging Patterns** (this repository): The adversary agent, conservator agent, and teacher agent all reference `iterations/iteration-N/candidates/<source>/` for staging artifacts. The student agent should match this pattern for artifact completeness.

## Key Findings

- Evidence order matters: critique > steering summary > current candidate > original > validation history.
- Stopping criteria need to be observable, not subjective.
- Artifact staging is required by the workspace staging contract but missing from the student prompt.
- Comparison to original prevents scope creep and validates "smallest defensible revision" claim.
- Reasoning trajectory should use an explicit format (chain-of-thought, tree-of-thought, etc.).

## Schema Guidance for Synthesis

Eval cases should test:
1. Correct evidence reading order when multiple inputs are present.
2. Artifact staging output (candidates/student/ companion files).
3. Loop termination under clear teacher approval and under ambiguous evidence.
4. Comparison to original (revision is narrowly scoped).
5. Handling of missing or absent teacher steering artifacts.

## Recommendation

Proceed to synthesis. No external dataset is required; cases can be grounded in trainer-loop scenarios from this repository.
